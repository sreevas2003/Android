# Cameara Stack

1. Sensor captures RAW data
2. ISP processes it
3. Kernel driver pushes frame to buffer
4. HAL wraps buffer + metadata
5. CameraService sends to app Surface
6. SurfaceFlinger composites frame
7. Display shows it



**📷 ISP (Image Signal Processor)**

👉 What it is

ISP = the brain of the camera

👉 What it does

It takes raw ugly data from the camera sensor and makes it a nice photo/video.

👉 Without ISP
- Image looks noisy
- Wrong colors
- Too dark or too bright

👉 ISP fixes
- Colors
- Brightness
- Noise
- Focus
- HDR

**🎥 V4L2 (Video4Linux2)**
👉 What it is

V4L2 = the language Linux uses to talk to cameras

👉 What it does

It lets a program say:

“Open the camera”

“Give me video frames”

“Change brightness/exposure”

👉 Where it runs

Inside Linux kernel

🔌 MIPI CSI
👉 What it is

MIPI CSI = the cable between camera and processor

👉 What it does

It carries camera data very fast from sensor to CPU/SoC.

👉 Used in

Mobile phones

Raspberry Pi camera

Embedded boards

👉 Not used in

USB webcams
