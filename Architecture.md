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

