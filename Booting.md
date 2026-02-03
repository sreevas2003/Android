# Android Booting Process
<img width="433" height="545" alt="image" src="https://github.com/user-attachments/assets/e3f16872-c1cb-4ba8-a664-29dc9958867e" />

**High-Level Boot Flow**

Power ON
↓
Boot ROM
↓
Bootloader
↓
Linux Kernel
↓
init process
↓
Zygote
↓
System Server
↓
Launcher (Home Screen)

## 1️⃣ Power ON & Boot ROM

**📌 What happens**
- Power is supplied to the device
- CPU starts execution from a fixed ROM address
- This code is called Boot ROM

**🧠 Responsibilities**
- Basic hardware initialization
- Verify bootloader integrity (secure boot)
- Load bootloader into RAM

📌 Boot ROM is hardware-specific and cannot be modified

## 2️⃣ Bootloader
**📌 What it is**
- Small program stored in flash
- Device-specific (OEM controlled)

**🧠 Responsibilities**
- Initialize RAM, display, peripherals
- Verify kernel & ramdisk (Verified Boot)
- Load Linux kernel + initramfs
- Support fastboot / recovery mode

📌 Examples:
- Little Kernel (LK)
- U-Boot (some devices)

## 3️⃣ Linux Kernel Initialization
**📌 What happens**
- Kernel is decompressed into memory
- Kernel starts execution

**🧠 Kernel Tasks**
Initialize:
- CPU scheduling
- Memory management
- Interrupts
- Device drivers
- Mount root filesystem (ramdisk)

📌 Kernel launches init as PID 1

## 4️⃣ init Process (PID = 1)
**📌 What is init?**
- First user-space process
- Android-specific init (not systemd)

**🧠 Responsibilities**
Parse init configuration files:
- /init.rc
- /system/etc/init/*.rc
- Start native daemons:
  - vold (storage)
  - netd (network)
  - logd (logging)
  - Set system properties
- Mount system partitions:
  - /system
  - /vendor
  - /data

📌 If init fails → device bootloops

## 5️⃣ Zygote Process (MOST IMPORTANT)
**📌 What is Zygote?**
- A preloaded Android runtime process
- Started by init

**🧠 Responsibilities**
- Start ART runtime

Preload:
- Core Java classes
- Framework resources
- Fork new app processes

**📌 Why Zygote?**
- Faster app startup
- Shared memory between apps

📌 Every Android app is forked from Zygote

## 6️⃣ System Server
📌 What it is
- Core Android framework process
- Spawned by Zygote

**🧠 Responsibilities**
Starts all major system services:
| Service                | Function      |
| ---------------------- | ------------- |
| ActivityManagerService | App lifecycle |
| PackageManagerService  | App install   |
| WindowManagerService   | UI windows    |
| PowerManagerService    | Power control |
| LocationManagerService | GPS           |
| NotificationManager    | Notifications |

📌 Runs in system_server process

## 7️⃣ SurfaceFlinger & Media Services
**📌 What happens**

SurfaceFlinger starts:
- Composites UI layers
Media services initialize:
- Audio
- Camera
- Video

📌 UI system becomes functional

## 8️⃣ Launcher (Home Screen)
**📌 Final Stage**
- Launcher app is started
- Home screen is displayed
- Device is now READY

📌 User interaction begins here

**Android boot process starts with Boot ROM, followed by Bootloader, Linux kernel initialization, init process, Zygote startup, system server initialization, and finally launching the home screen. Each stage verifies and prepares the system for the next stage.**
