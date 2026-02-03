**Android is:**
- A Linux-based, open-source operating system used on smartphones, tablets, smart TVs, wearables, and more.
- Known for its versatility and global reach powering billions of devices in over 190 countries.
- Supported by a rich set of development tools, libraries, and frameworks that help streamline app creation from UI design to performance optimization.

**🆚 Android vs Embedded Linux (Key Differences)**

| Feature          | Android   | Embedded Linux |
| ---------------- | --------- | -------------- |
| Hardware access  | Via HAL   | Direct         |
| UI               | Mandatory | Optional       |
| App runtime      | ART       | None           |
| C library        | Bionic    | glibc/musl     |
| Customization    | Limited   | High           |
| Embedded control | Weak      | Strong         |


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
##  Hardware Abstraction Layer (HAL)
👉 HAL is the boundary between Android software and hardware drivers.
**🔹 What is HAL?**

HAL (Hardware Abstraction Layer) is a set of standardized interfaces that allow Android system services to communicate with hardware-specific drivers without knowing hardware details.

HAL enables hardware abstraction and portability across different devices.

**🔹 What HAL Actually Contains**

- Written in C / C++
- Implemented by:
    - Device manufacturers
    - SoC vendors (Qualcomm, MediaTek, etc.)
- Packaged as:
    - Shared libraries (.so)
- Located in: /vendor/lib  and /vendor/lib64

**🔹 HAL Responsibilities**

✔ Translate framework requests into driver calls

✔ Hide hardware-specific details

✔ Provide stable interfaces to Android

✔ Enforce separation between system & vendor code

**🔹 HAL Architecture (Old vs New)**
🟡**Legacy HAL (Pre-Android 8)**

- Function pointers
- Direct .so loading
- Tight coupling

**🟢 Modern HAL (Android 8+)**

Uses:
- HIDL (HAL Interface Definition Language)
- AIDL (newer versions)

Benefits:
- Versioned interfaces
- Binder-based IPC
- Better security
- Easier updates (Project Treble)

**🔹 Common HAL Modules (Interview Must-Know)**

| HAL Module    | Purpose              |
| ------------- | -------------------- |
| Audio HAL     | Speaker, mic         |
| Camera HAL    | Camera pipeline      |
| Bluetooth HAL | BLE, BR/EDR          |
| Wi-Fi HAL     | WLAN                 |
| Sensor HAL    | Accelerometer, gyro  |
| GPS HAL       | Location             |
| Lights HAL    | LEDs, backlight      |
| Power HAL     | CPU frequency, sleep |

**Interview-Ready Summary (Strong)**

The Hardware Abstraction Layer (HAL) provides a standardized interface between Android system services and hardware-specific drivers. It allows the Android framework to remain hardware-independent while enabling device manufacturers to implement hardware functionality using vendor-specific code, improving portability, security, and system stability.

-------------------------------------------------------------------
## Linux Kernel in Android

**What is the Linux Kernel in Android?**
- Android uses a modified Linux kernel as its core operating system.
It provides:
- Process management
- Memory management
- Device drivers
- Networking
- Security
- Power management

**🔹 Core Responsibilities of Linux Kernel in Android**

**1️⃣ Process Management**

Each Android app:
- Runs as a Linux process

Has:
- Unique PID
- Unique UID

Kernel handles:
- Process creation (fork())
- Scheduling
- Context switching

Every Android application runs as a separate Linux process.

**2️⃣ Memory Management**

Kernel manages:
- Virtual memory
- Paging
- Shared memory
- Low memory handling

Android-specific additions:

- LMKD (Low Memory Killer Daemon)
- Pressure-based memory reclaim

Goal:
✔ Prevent system freeze
✔ Kill background apps gracefully

**3️⃣ Device Drivers**

Kernel includes drivers for:
- Display
- Touch
- Camera
- Bluetooth
- Wi-Fi
- Sensors
- Audio

**4️⃣ Power Management (Very Important for Mobile)**

Android adds:
- Wakelocks
- CPU frequency scaling
- Suspend / resume

Why?

- Save battery 🔋
- Prevent sleep during critical work

Android extends the Linux kernel with wakelocks to manage power efficiently.

**5️⃣ Security & Isolation**

Linux Security Features:
- UID / GID
- File permissions
- Capabilities

Android Additions:

- SELinux (enforcing mode)
- Mandatory Access Control (MAC)
- App sandboxing

Each app:

- Runs as its own Linux user
- Cannot access others’ data

**6️⃣ IPC Mechanisms**

Linux provides:
- Pipes
- Sockets
- Shared memory
- Signals

Android adds:
- Binder IPC (MOST IMPORTANT)

Binder:
- Fast
- Secure
- Object-based IPC

Used by:
- ActivityManager
- BluetoothService
- HAL services

**7️⃣ Android-Specific Kernel Components**

These do NOT exist in standard Linux 👇

🔸 Binder Driver

- Kernel-level IPC mechanism
- Enables framework ↔ service communication

🔸 Ashmem (Anonymous Shared Memory)

- Efficient shared memory
- Used for graphics & media

🔸 Wakelocks

- Prevent CPU from sleeping

🔸 Logger (logcat)

- Kernel logging support

Binder, Ashmem, and wakelocks are Android-specific kernel extensions.

**Interview-Ready Summary (Strong)**

Android uses a modified Linux kernel as its core operating system. The Linux kernel provides essential services such as process scheduling, memory management, device drivers, power management, security, and IPC. Android extends the kernel with features like Binder IPC, wakelocks, and SELinux to support mobile-specific requirements while maintaining Linux stability and performance.

---------------------------------------------------


## Binder IPC (Inter-Process Communication)

**🔹 What is Binder IPC?**

Binder is a high-performance, secure, object-oriented IPC mechanism used in Android for communication between processes.

Android uses Binder instead of traditional Linux IPC for performance, security, and object-based communication.

**🔹 Core Components of Binder IPC**

Binder has 4 main parts 👇

**1️⃣ Binder Driver (Kernel Space)**

📌 Lives in:  /dev/binder

Responsibilities:

- Transfers data between processes
Manages:
- Thread synchronization
- Reference counting
- Security checks (UID/PID)

👉 This is the heart of Binder

**2️⃣ Service Manager**

📌 Acts like a directory / phonebook

Responsibilities:
- Registers system services
- Lets clients discover services

Example services:
- ActivityManager
- BluetoothService
- LocationService

**3️⃣ Binder Libraries (User Space)**

Used by:
- Framework
- Native services
- Apps (indirectly)

They:
- Marshal data
- Talk to binder driver
- Handle transactions

**4️⃣ Proxy & Stub Objects**

Binder uses object-oriented IPC.
- Proxy → client side
- Stub → service side

--------------------------------------

**AIDL (Android Interface Definition Language)**
📌 What is AIDL?

AIDL is used to define interfaces for communication between Android applications and system services.

📍 Layer:

Application ↔ Application
Application ↔ Framework Service

**HIDL (HAL Interface Definition Language)**
📌 What is HIDL?

HIDL is used to define interfaces between Android Framework and Hardware Abstraction Layer (HAL).

📍 Layer:

Framework ↔ HAL
System services ↔ Vendor hardware

**AIDL vs HIDL**

| Feature         | AIDL                                  | HIDL                              |
| --------------- | ------------------------------------- | --------------------------------- |
| Full Form       | Android Interface Definition Language | HAL Interface Definition Language |
| Used Between    | Apps & services                       | Framework & HAL                   |
| Introduced For  | App-level IPC                         | Project Treble                    |
| Runs In         | App / system_server                   | Native HAL service                |
| Hardware Access | ❌                                     | ✅                                 |
| Versioning      | ❌                                     | ✅                                 |
| File Extension  | `.aidl`                               | `.hal`                            |
| Transport       | Binder                                | Binder                            |

⚠️ Modern Android Note (VERY IMPORTANT)

👉 HIDL is being replaced by AIDL for HALs (called AIDL HALs) in newer Android versions.

So today:

AIDL → Apps + Framework + New HALs

HIDL → Legacy HALs

-------------------------------------
## Zygote Process in Android
**What is Zygote? (Definition)**

Zygote is a core Android runtime process that starts during system boot, preloads common framework classes and resources, and forks to create new application processes.

**🔹 How Zygote Starts (Boot-Time)**
1️⃣ Bootloader
- Loads Linux kernel

2️⃣ Linux Kernel
- Starts init (PID 1)

3️⃣ init Process
- Reads init.rc

Starts Zygote using:   /system/bin/app_process

Now Zygote is alive 🧠

**🔹 What Zygote Does at Startup**

Before any app runs, Zygote:

✔ Initializes ART
✔ Preloads:
- Core Java classes
- Android framework classes
- Resources
    ✔ Opens Binder connection

Why preload?
- Faster app startup
- Shared memory (Copy-On-Write)
- Lower RAM usage

Zygote preloads common classes to enable fast app startup using copy-on-write memory.

**🔹 Zygote Forks system_server First**

Zygote’s first child is:  system_server

system_server hosts:
- ActivityManagerService (AMS)
- PackageManagerService (PMS)
- WindowManagerService (WMS)
- BluetoothService
- PowerManagerService

👉 Without system_server, Android UI cannot run.

**🔹 Zygote & App Launch (Step-by-Step)**

🟢 Step 1: User taps app icon : Launcher sends request to AMS

🟢 Step 2: AMS checks process : If app not running → request Zygote

🟢 Step 3: AMS → Zygote (Binder IPC) : Request to fork a new app process

🟢 Step 4: Zygote Forks Process : 

Zygote
 ├── Parent (Zygote)
 └── Child (App Process)

Uses:
- Linux fork()
- Copy-on-Write memory

🟢 Step 5: Child Process Setup

In the new app process:
- Assign UID / GID
- Apply SELinux context
- Initialize ART
- Load app DEX & native libs

🟢 Step 6: App Starts

AMS tells app to start:  MainActivity

Lifecycle :  onCreate → onStart → onResume

🎉 App UI appears
