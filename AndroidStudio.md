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
