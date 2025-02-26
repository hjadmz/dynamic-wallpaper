# **Dynamic Wallpaper Automation for macOS**

This project automates switching between Light and Dark mode wallpapers on macOS based on the system's appearance settings. It uses AppleScript and macOS LaunchAgents to run the script automatically at startup.

---

## **Features**
- Automatically updates the desktop wallpaper when macOS switches between Light and Dark mode.
- Runs in the background using a LaunchAgent.

---

## **Setup Instructions**

Follow these steps to set up the dynamic wallpaper automation on your macOS system.

---

### **Step 1: Prepare Your Wallpapers**
1. Choose two wallpapers:
   - **Dark Mode wallpaper** (e.g., `dark-wallpaper.jpg`)
   - **Light Mode wallpaper** (e.g., `light-wallpaper.jpg`)

2. Place the wallpapers in a folder:
   ```plaintext
   ~/Pictures/DynamicWallpapers/
   ```
   - Full paths:
     - Dark Mode wallpaper: `~/Pictures/DynamicWallpapers/dark-wallpaper.jpg`
     - Light Mode wallpaper: `~/Pictures/DynamicWallpapers/light-wallpaper.jpg`

---

### **Step 2: Create the AppleScript**
1. Open the **Script Editor** app on your Mac (search for it using Spotlight or find it in Applications > Utilities).
2. Paste the following code into the editor:

   ```applescript
   -- Check system appearance (Dark Mode or Light Mode)
   tell application "System Events"
       set darkMode to (dark mode of appearance preferences)
   end tell

   -- Set wallpaper based on appearance
   if darkMode then
       -- Dark Mode wallpaper
       tell application "System Events"
           set picture of every desktop to "~/Pictures/DynamicWallpapers/dark-wallpaper.jpg"
       end tell
   else
       -- Light Mode wallpaper
       tell application "System Events"
           set picture of every desktop to "~/Pictures/DynamicWallpapers/light-wallpaper.jpg"
       end tell
   end if
   ```

3. Save the script to the following location:
   ```plaintext
   ~/Scripts/dynamic-wallpaper.scpt
   ```
   - If the `Scripts` folder doesn’t exist, create it:
     ```bash
     mkdir -p ~/Scripts
     ```

---

### **Step 3: Create the LaunchAgent**
1. Open **Terminal**.
2. Create a new LaunchAgent file:
   ```bash
   nano ~/Library/LaunchAgents/com.dynamic-wallpaper.plist
   ```

3. Paste the following content into the file:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
   <dict>
       <key>Label</key>
       <string>com.dynamic-wallpaper</string>
       <key>ProgramArguments</key>
       <array>
           <string>/usr/bin/osascript</string>
           <string>/Users/YOUR_USERNAME/Scripts/dynamic-wallpaper.scpt</string>
       </array>
       <key>RunAtLoad</key>
       <true/>
   </dict>
   </plist>
   ```

   Replace `YOUR_USERNAME` with your macOS username. You can find your username by running:
   ```bash
   whoami
   ```

4. Save the file and exit.

---

### **Step 4: Load the LaunchAgent**
1. Make sure the `.plist` file is valid:
   ```bash
   plutil ~/Library/LaunchAgents/com.dynamic-wallpaper.plist
   ```
   - If it’s valid, you’ll see: `~/Library/LaunchAgents/com.dynamic-wallpaper.plist: OK`

2. Load the LaunchAgent:
   ```bash
   launchctl load ~/Library/LaunchAgents/com.dynamic-wallpaper.plist
   ```

3. Verify it’s running:
   ```bash
   launchctl list | grep com.dynamic-wallpaper
   ```

---

### **Step 5: Test the Setup**
1. Toggle between Light and Dark mode in **System Settings > Appearance**.
2. Your wallpaper should automatically switch based on the system appearance.

---

### **Troubleshooting**
- **To unload the LaunchAgent**:
  ```bash
  launchctl unload ~/Library/LaunchAgents/com.dynamic-wallpaper.plist
  ```

- **To debug errors**, check the system logs:
  ```bash
  log show --predicate 'subsystem == "com.apple.launchd"' --info | grep dynamic-wallpaper
  ```

---

## **Folder Structure**
Here’s the simplified folder structure for this project:

```plaintext
~/Pictures/DynamicWallpapers/
    ├── dark-wallpaper.jpg
    ├── light-wallpaper.jpg
~/Scripts/
    ├── dynamic-wallpaper.scpt
~/Library/LaunchAgents/
    ├── com.dynamic-wallpaper.plist
```

---

## **License**
This project is licensed under the MIT License.