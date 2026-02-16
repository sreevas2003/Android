## Stage-1  Printing text in APP
1️⃣ Enable Developer Options (if not already)

Settings → About phone

Tap "MIUI version / Build number" 7 times

2️⃣ Go to Developer Options

Settings → Additional settings → Developer options

3️⃣ ENABLE THESE (VERY IMPORTANT)

Turn ON all of these:
✅ USB debugging
✅ Install via USB ← 🔴 THIS ONE IS THE KEY

5️⃣ Verify from PC

Run:
adb devices

🔁 NOW TRY AGAIN
In Android Studio:
Run ▶

When you press ▶ Run:

1️⃣ APK installs successfully
2️⃣ App opens (blank UI is OK)
3️⃣ Logcat shows (later):
NDK_TEST: Hello from native C++

--------------------------------------------------

## Stage-2 Detect Camera from Native C++

### native-lib.cpp
```c++
#include <jni.h>

extern "C"
JNIEXPORT void JNICALL
Java_com_example_newcam_MainActivity_processFrame(
        JNIEnv *env,
        jobject,
        jintArray pixels,
        jint width,
        jint height) {

    jint *buffer = env->GetIntArrayElements(pixels, NULL);

    int size = width * height;

    for (int i = 0; i < size; i++) {
        int pixel = buffer[i];

        int A = (pixel >> 24) & 0xff;
        int R = (pixel >> 16) & 0xff;
        int G = (pixel >> 8) & 0xff;
        int B = pixel & 0xff;

        // Invert colors
        R = 255 - R;
        G = 255 - G;
        B = 255 - B;

        buffer[i] = (A << 24) | (R << 16) | (G << 8) | B;
    }

    env->ReleaseIntArrayElements(pixels, buffer, 0);
}
```
### MainActivity.kt
```kt
package com.example.newcam

import android.Manifest
import android.content.ContentValues
import android.content.Context
import android.content.pm.PackageManager
import android.graphics.Bitmap
import android.graphics.BitmapFactory
import android.hardware.camera2.*
import android.media.ImageReader
import android.os.*
import android.provider.MediaStore
import android.view.Surface
import android.view.TextureView
import android.widget.*
import androidx.appcompat.app.AppCompatActivity
import androidx.core.app.ActivityCompat
import java.nio.ByteBuffer

class MainActivity : AppCompatActivity() {

    private lateinit var textureView: TextureView
    private lateinit var captureBtn: Button
    private lateinit var saveBtn: Button
    private lateinit var backBtn: Button
    private lateinit var capturedImage: ImageView

    private lateinit var cameraDevice: CameraDevice
    private lateinit var captureSession: CameraCaptureSession
    private lateinit var imageReader: ImageReader

    private lateinit var cameraThread: HandlerThread
    private lateinit var cameraHandler: Handler

    private var capturedBitmap: Bitmap? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        textureView = findViewById(R.id.textureView)
        captureBtn = findViewById(R.id.captureBtn)
        saveBtn = findViewById(R.id.saveBtn)
        backBtn = findViewById(R.id.backBtn)
        capturedImage = findViewById(R.id.capturedImage)

        cameraThread = HandlerThread("CameraThread")
        cameraThread.start()
        cameraHandler = Handler(cameraThread.looper)

        captureBtn.setOnClickListener { capturePhoto() }

        saveBtn.setOnClickListener {
            capturedBitmap?.let { saveImage(it) }
        }

        backBtn.setOnClickListener {
            capturedImage.visibility = ImageView.GONE
            captureBtn.visibility = Button.VISIBLE
            saveBtn.visibility = Button.GONE
            backBtn.visibility = Button.GONE
        }

        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.CAMERA)
            != PackageManager.PERMISSION_GRANTED) {

            ActivityCompat.requestPermissions(
                this,
                arrayOf(Manifest.permission.CAMERA),
                100
            )
        } else {
            textureView.surfaceTextureListener = surfaceListener
        }
    }

    override fun onDestroy() {
        super.onDestroy()
        if (::cameraDevice.isInitialized) cameraDevice.close()
        cameraThread.quitSafely()
    }

    override fun onRequestPermissionsResult(
        requestCode: Int,
        permissions: Array<out String>,
        grantResults: IntArray
    ) {
        if (requestCode == 100 &&
            grantResults.isNotEmpty() &&
            grantResults[0] == PackageManager.PERMISSION_GRANTED) {

            textureView.surfaceTextureListener = surfaceListener
        }
    }

    private val surfaceListener = object : TextureView.SurfaceTextureListener {
        override fun onSurfaceTextureAvailable(surface: android.graphics.SurfaceTexture,
                                               width: Int, height: Int) {
            openCamera()
        }
        override fun onSurfaceTextureSizeChanged(surface: android.graphics.SurfaceTexture,
                                                 width: Int, height: Int) {}
        override fun onSurfaceTextureDestroyed(surface: android.graphics.SurfaceTexture) = false
        override fun onSurfaceTextureUpdated(surface: android.graphics.SurfaceTexture) {}
    }

    private fun openCamera() {
        val manager = getSystemService(Context.CAMERA_SERVICE) as CameraManager
        val cameraId = manager.cameraIdList[0]

        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.CAMERA)
            != PackageManager.PERMISSION_GRANTED) return

        manager.openCamera(cameraId, stateCallback, cameraHandler)
    }

    private val stateCallback = object : CameraDevice.StateCallback() {
        override fun onOpened(camera: CameraDevice) {
            cameraDevice = camera
            startPreview()
        }
        override fun onDisconnected(camera: CameraDevice) {}
        override fun onError(camera: CameraDevice, error: Int) {}
    }

    private fun startPreview() {
        val surfaceTexture = textureView.surfaceTexture!!
        surfaceTexture.setDefaultBufferSize(640, 480)
        val previewSurface = Surface(surfaceTexture)

        imageReader = ImageReader.newInstance(
            640, 480,
            android.graphics.ImageFormat.JPEG,
            2
        )

        val requestBuilder =
            cameraDevice.createCaptureRequest(CameraDevice.TEMPLATE_PREVIEW)
        requestBuilder.addTarget(previewSurface)

        cameraDevice.createCaptureSession(
            listOf(previewSurface, imageReader.surface),
            object : CameraCaptureSession.StateCallback() {
                override fun onConfigured(session: CameraCaptureSession) {
                    captureSession = session
                    session.setRepeatingRequest(
                        requestBuilder.build(),
                        null,
                        cameraHandler
                    )
                }
                override fun onConfigureFailed(session: CameraCaptureSession) {}
            },
            cameraHandler
        )
    }

    private fun capturePhoto() {
        val captureBuilder =
            cameraDevice.createCaptureRequest(CameraDevice.TEMPLATE_STILL_CAPTURE)
        captureBuilder.addTarget(imageReader.surface)

        imageReader.setOnImageAvailableListener({ reader ->
            val image = reader.acquireLatestImage()
            val buffer: ByteBuffer = image.planes[0].buffer
            val bytes = ByteArray(buffer.remaining())
            buffer.get(bytes)

            val bitmap = BitmapFactory.decodeByteArray(bytes, 0, bytes.size)
            capturedBitmap = bitmap

            runOnUiThread {
                capturedImage.setImageBitmap(bitmap)
                capturedImage.visibility = ImageView.VISIBLE

                captureBtn.visibility = Button.GONE
                saveBtn.visibility = Button.VISIBLE
                backBtn.visibility = Button.VISIBLE
            }

            image.close()
        }, cameraHandler)

        captureSession.capture(captureBuilder.build(), null, cameraHandler)
    }

    private fun saveImage(bitmap: Bitmap) {
        val resolver = contentResolver
        val contentValues = ContentValues().apply {
            put(MediaStore.MediaColumns.DISPLAY_NAME,
                "NewCam_${System.currentTimeMillis()}.jpg")
            put(MediaStore.MediaColumns.MIME_TYPE, "image/jpeg")
            put(MediaStore.MediaColumns.RELATIVE_PATH, "Pictures/NewCam")
        }

        val uri = resolver.insert(
            MediaStore.Images.Media.EXTERNAL_CONTENT_URI,
            contentValues
        )

        uri?.let {
            resolver.openOutputStream(it)?.use { stream ->
                bitmap.compress(Bitmap.CompressFormat.JPEG, 100, stream)
            }
            Toast.makeText(this, "Saved to Gallery", Toast.LENGTH_SHORT).show()
        }
    }
}
```
### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextureView
        android:id="@+id/textureView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

    <ImageView
        android:id="@+id/capturedImage"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:visibility="gone"
        android:scaleType="fitCenter"/>

    <LinearLayout
        android:layout_gravity="bottom|center"
        android:layout_marginBottom="30dp"
        android:orientation="horizontal"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">

        <Button
            android:id="@+id/captureBtn"
            android:text="Capture"
            android:layout_width="120dp"
            android:layout_height="60dp"/>

        <Button
            android:id="@+id/saveBtn"
            android:text="Save"
            android:layout_width="120dp"
            android:layout_height="60dp"
            android:visibility="gone"
            android:layout_marginStart="20dp"/>

        <Button
            android:id="@+id/backBtn"
            android:text="Back"
            android:layout_width="120dp"
            android:layout_height="60dp"
            android:visibility="gone"
            android:layout_marginStart="20dp"/>
    </LinearLayout>

</FrameLayout>
```
### AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest package="com.example.newcam"
    xmlns:android="http://schemas.android.com/apk/res/android">

    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-feature android:name="android.hardware.camera"/>

    <application
        android:allowBackup="true"
        android:label="NewCam"
        android:theme="@style/Theme.NewCam">

        <activity
            android:name=".MainActivity"
            android:exported="true">

            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>

        </activity>
    </application>
</manifest>
```
### CMakeLists.txt
```txt
cmake_minimum_required(VERSION 3.22.1)

project("newcam")

add_library(
        native-lib
        SHARED
        native-lib.cpp)

find_library(
        log-lib
        log)

target_link_libraries(
        native-lib
        ${log-lib})
```
### build.gradle.kts
```kts
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.android)
}

android {
    namespace = "com.example.newcam"
    compileSdk = 36

    defaultConfig {
        applicationId = "com.example.newcam"
        minSdk = 31
        targetSdk = 36
        versionCode = 1
        versionName = "1.0"

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_11
        targetCompatibility = JavaVersion.VERSION_11
    }
    kotlinOptions {
        jvmTarget = "11"
    }
    externalNativeBuild {
        cmake {
            path = file("src/main/cpp/CMakeLists.txt")
            version = "3.22.1"
        }
    }
    buildFeatures {
        viewBinding = true
    }
}

dependencies {

    implementation(libs.androidx.core.ktx)
    implementation(libs.androidx.appcompat)
    implementation(libs.material)
    implementation(libs.androidx.constraintlayout)
    testImplementation(libs.junit)
    androidTestImplementation(libs.androidx.junit)
    androidTestImplementation(libs.androidx.espresso.core)
}
```
