**Android is:**
- A Linux-based, open-source operating system used on smartphones, tablets, smart TVs, wearables, and more.
- Known for its versatility and global reach powering billions of devices in over 190 countries.
- Supported by a rich set of development tools, libraries, and frameworks that help streamline app creation from UI design to performance optimization.

<img width="1384" height="2038" alt="image" src="https://github.com/user-attachments/assets/c0f8318f-86d8-4272-9cf9-ba4d9195fe0c" />

## Application Layer

The Application Layer is the topmost layer of the Android architecture.

**it Contains all Android applications**
- System apps (pre-installed)
- User-installed apps

**Written mainly in:**
- Java
- Kotlin

Each app runs in its own sandboxed process for security.

**Examples:**
- Phone
- Contacts
- SMS
- Browser
- Camera
- Your custom apps 🚀

### Key Characteristics
1. **User Interface (UI)** - This layer directly interacts with the end user.
2. **Uses Android Framework APIs** - Applications do not access hardware directly. Instead, they use: Application Frame work.
3. **APK-Based Deployment** - Apps are packaged as APK (Android Package)
4. **Security & Sandboxing** - Each app: i) Has a unique Linux UID ii) Runs in a separate process

**Summary (Interview-Ready)**

The Application Layer is the topmost layer of Android architecture that contains system and user-installed applications. It provides the user interface and uses Android Framework APIs to access system services, while remaining isolated and secure through sandboxing and permissions.

-------------------------
## Application Framework Layer

<img width="1400" height="1090" alt="image" src="https://github.com/user-attachments/assets/15a8f619-da3a-48a5-b57d-9ceccca3be29" />

The Application Framework Layer sits between applications and the Android runtime / HAL.

It is a collection of Java/Kotlin APIs + system services that:
1. **Activity Manager** - Activity Manager controls application lifecycle and manages activity stack.
2. **Window Manager** - Manages windows & screen layout
3. **View System** - Builds the UI **Includes:** Views (Button, TextView, ImageView), ViewGroups (LinearLayout, ConstraintLayout), Event handling (touch, key).
4. **Notification Manager** - Manages notifications.
5. **Resource Manager** - Manages non-code resources **Handles:** Strings, Colors, Layouts, Drawables, Dimensions.
6. **Package Manager** - Manages APK files **Responsibilities:** App installation / uninstallation, Permission validation, App signatures, App metadata
7. **Content Providers** - Data sharing between apps
8. **Location Manager** - Provides location services.
9. **Telephony Manager** - Handles cellular communication. **Manages:** Calls, SMS, Network info, SIM details
10. **Sensor Manager** - Access to device sensors **Examples:** Accelerometer, Gyroscope, Proximity, Light sensor

**Interview Summary (Short & Strong)**

The Application Framework layer provides high-level APIs and system services such as Activity Manager, Window Manager, Content Providers, and Notification Manager. It enables application development by managing UI, lifecycle, resources, IPC, and controlled access to hardware through system services.

---------------------------------
## Android Runtime (ART)
**🔹 What is Android Runtime (ART)?**

ART (Android Runtime) is the managed runtime environment that:
- Executes Android applications
- Converts app bytecode into native machine code
- Manages memory
- Handles garbage collection
- Improves performance & battery life

**Responsibilities**
- Garbage Collection
- Memory management
- Thread handling
- Exception handling

📌 Old Android → Dalvik
📌 New Android → ART

**ART vs Dalvik**

| Feature       | Dalvik        | ART          |
| ------------- | ------------- | ------------ |
| Compilation   | JIT only      | AOT + JIT    |
| App startup   | Slower        | Faster       |
| Battery usage | Higher        | Lower        |
| GC            | Basic         | Advanced     |
| Introduced    | Early Android | Android 5.0+ |

**ART Execution Model**

Android apps are written in:
- Java / Kotlin

Compiled into:
- DEX (Dalvik Executable) bytecode

ART executes DEX bytecode using:

✅ AOT (Ahead-Of-Time) Compilation
✅ JIT (Just-In-Time) Compilation
✅ Profile-guided optimization

### 🔹 Key Components of ART

**1️⃣ AOT Compilation**
📌 Happens:
- During app installation

📌 What it does:
- Converts DEX → native machine code

📌 Advantage:
- Faster app startup
- Less runtime overhead

📌 Drawback:
- Slower app install
- More storage usage

**2️⃣ JIT Compilation (Hybrid Mode)**
Introduced later (Android 7+):

📌 Happens:
- While app is running

📌 What it does:
- Compiles hot paths (frequently used code)

📌 Advantage:
- Smaller install size
- Better runtime optimization

📌 Uses:
- Execution profiles

**Garbage Collection (GC)**
- ART uses concurrent and generational garbage collection to minimize pause times.

**Zygote Process (VERY IMPORTANT)**
📌 Zygote = parent process for all apps

What happens:
- System boots
- Zygote starts
- Preloads:
    - Core Java classes
    - Framework classes

Benefits:
- Faster app startup
- Shared memory pages
- Less RAM usage

**Zygote is a core Android process started during system boot that preloads framework classes and forks to create new application processes. When an app is launched, ActivityManagerService requests Zygote to fork a new process, which initializes the app runtime and starts the main activity, enabling fast startup and efficient memory usage.**

**Interview-Ready Summary**

Android Runtime (ART) is the managed runtime environment that executes Android applications. It uses a hybrid compilation approach with AOT and JIT, supports profile-guided optimizations, manages memory through advanced garbage collection, and improves application performance, startup time, and battery efficiency.

------------------------------------
## Native Libraries in Android
**🔹 What Are Native Libraries?**
- Written in C / C++
- Compiled as .so (shared object) files
- Run in native space

Used by:
- Android Framework
- System services
- Apps (via JNI / NDK)

They act as a bridge between:
- ART (managed world)
- HAL / Kernel (hardware world)

**🔹 Why Android Uses Native Libraries**
1️⃣ Performance
- Faster than Java/Kotlin
- Used for:
    - Media processing
    - Graphics rendering
    - Bluetooth & networking
    - Cryptography

2️⃣ Hardware Access
- Talk to HAL
- HAL talks to drivers

3️⃣ Code Reuse

Same libraries reused across:
- Devices
- OEMs
- Android versions

**🔹 Major Native Libraries (Important for Interviews)**

**1️⃣ Bionic libc (Android C Library)**

📌 Android’s own C library
📌 Replaces glibc (used in desktop Linux)

**Provides:**
- malloc(), free()
- pthread
- File I/O
- System calls wrapper

**Why not glibc?**
- Smaller than glibc
- Faster compared glibc
- Mobile-optimized

**2️⃣ Media Libraries** - Handles audio & video **Includes:** Media codecs, Audio playback, Video decoding/encoding, Camera pipeline

**3️⃣ SurfaceFlinger** - 📌 Display compositor

Responsibilities:
- Combines app surfaces

Handles:
- UI layers
- Animations
- Transparency
- Sends final frame to display

Works with:
- Window Manager
- GPU drivers

**4️⃣ OpenGL ES / Vulkan**

📌 Graphics rendering libraries

Used for:
- 2D / 3D graphics
- Games
- UI acceleration

**5️⃣ SQLite**

📌 Lightweight database engine

Features:
- Embedded
- Serverless
- Zero configuration

Used by:
- Contacts
- Messages
- App local storage

**6️⃣ WebKit / Chromium Libraries**
📌 Web rendering engine

Used by:
- WebView
- Browser
- Hybrid apps

**7️⃣ SSL / Crypto Libraries**

📌 Security libraries

Used for:
- HTTPS
- Encryption
- Certificates
- Secure communication

**Interview-Ready Summary**

Native Libraries in Android are C/C++ libraries that provide core system functionality such as graphics, media, database, and networking. They are used by the Android Framework and applications via JNI, enabling high-performance execution and controlled access to hardware through HAL and the Linux kernel.

------------------------------------
## 
