# ANDROID DEBUGGING COMMANDS – CHEAT SHEET

---

# 1. ADB BASICS

**Device connection and server control**

```commands
adb devices
```

List connected devices

```commands
adb start-server
adb kill-server
```

Start / stop adb server

```commands
adb reboot
adb reboot bootloader
adb reboot recovery
```

Reboot device into different modes

---

# 2. DEVICE & SYSTEM INFORMATION

**General system details**

```commands
adb shell
```

Enter Android shell

```commands
adb shell getprop
```

Show system properties

```commands
adb shell getprop ro.build.version.release
```

Android version

```commands
adb shell uname -a
```

Kernel information

```commands
adb shell uptime
```

System uptime

---

# 3. APPLICATION MANAGEMENT

**Install, uninstall and inspect apps**

```commands
adb install app.apk
```

Install APK

```commands
adb uninstall com.example.app
```

Uninstall application

```commands
adb shell pm list packages
```

List installed packages

```commands
adb shell pm path com.example.app
```

Get APK path

```commands
adb shell dumpsys package com.example.app
```

Detailed package information

---

# 4. ACTIVITY & APP LAUNCH DEBUGGING

**Activity and task control**

```commands
adb shell am start -n com.pkg/.MainActivity
```

Launch activity

```commands
adb shell am force-stop com.pkg
```

Force stop application

```commands
adb shell am kill com.pkg
```

Kill application process

```commands
adb shell am stack list
```

View task and stack information

---

# 5. PROCESS & MEMORY DEBUGGING

**CPU and memory usage analysis**

```commands
adb shell ps
```

List running processes

```commands
adb shell top
```

CPU usage monitoring

```commands
adb shell dumpsys meminfo
```

System memory usage

```commands
adb shell dumpsys meminfo com.pkg
```

Application memory usage

```commands
adb shell cat /proc/meminfo
```

Kernel memory details

---

# 6. LOGGING (LOGCAT)

**View and filter logs**

```commands
adb logcat
```

View logs

```commands
adb logcat -c
```

Clear logs

```commands
adb logcat ActivityManager:I *:S
```

Filter logs by tag

```commands
adb logcat | grep "CRASH"
```

View crash logs

```commands
adb logcat -b events
```

Event logs buffer

---

# 7. SYSTEM SERVICES DEBUGGING (DUMPSYS)

**Inspect Android system services**

```commands
adb shell dumpsys
```

List all system services

```commands
adb shell dumpsys activity
```

Activity Manager details

```commands
adb shell dumpsys window
```

Window Manager information

```commands
adb shell dumpsys power
```

Power management status

```commands
adb shell dumpsys battery
```

Battery information

---

# 8. UI & GRAPHICS DEBUGGING

**Display and rendering analysis**

```commands
adb shell dumpsys SurfaceFlinger
```

Graphics compositor information

```commands
adb shell dumpsys gfxinfo com.pkg
```

UI rendering statistics

```commands
adb shell screencap /sdcard/screen.png
adb pull /sdcard/screen.png
```

Capture and pull screenshot

---

# 9. PERMISSIONS & SECURITY

**Permission and SELinux debugging**

```commands
adb shell pm list permissions
```

List permissions

```commands
adb shell dumpsys package com.pkg | grep permission
```

Application permissions

```commands
adb shell getenforce
```

Check SELinux mode

```commands
adb shell setenforce 0
```

Set SELinux permissive (root only)

---

# 10. FILESYSTEM & STORAGE

**File operations and disk usage**

```commands
adb shell ls /system
```

List system directory

```commands
adb pull /sdcard/file.txt
```

Copy file from device

```commands
adb push file.txt /sdcard/
```

Copy file to device

```commands
adb shell df -h
```

Disk usage

---

# 11. NATIVE DEBUGGING

**Low-level debugging tools**

```commands
adb shell strace -p <pid>
```

Trace system calls

```commands
adb shell lsof
```

List open files

```commands
adb shell cat /proc/<pid>/maps
```

Process memory map

---

# 12. BINDER & IPC DEBUGGING

**Binder-related diagnostics**

```commands
adb shell service list
```

List Binder services

```commands
adb shell dumpsys binder
```

Binder statistics

```commands
adb shell dumpsys activity services
```

Running services information

---

# 13. BOOT & CRASH DEBUGGING

**Kernel and boot issues**

```commands
adb logcat -b kernel
```

Kernel logs

```commands
adb shell dmesg
```

Kernel messages

```commands
adb shell cat /proc/last_kmsg
```

Last boot crash log

```commands
adb shell reboot bootloader
```

Reboot into bootloader

---

# INTERVIEW MUST-KNOW COMMANDS

```commands
adb devices
adb logcat
adb shell dumpsys activity
adb shell dumpsys meminfo
adb shell am start
adb shell ps
adb shell top
```
