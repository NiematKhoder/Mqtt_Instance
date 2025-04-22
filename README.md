# Installing and Configuring Mosquitto on Windows (Step-by-Step)

This guide explains how to install, configure, and secure a Mosquitto MQTT broker instance on Windows 10/11, including setting up firewall rules and connecting via MQTT Explorer.

---

## 1. Download and Install Mosquitto

1. Open a browser and **search "Download Eclipse Mosquitto Windows"**.
2. Go to the official page: https://mosquitto.org/download/
3. Download the Windows installer (EXE format) under "Binary installation".
4. Run the installer with default settings. It will install to:
```
C:\Program Files\mosquitto

 ```

---

## 2. Add Mosquitto to System PATH (optional but helpful)

1. Open **Start → type "environment variables" → Edit the system environment variables**.
2. In **System Properties**, click **Environment Variables**.
3. Under **System variables**, find `Path` → click **Edit**.
4. Click **New** → Add:
```
C:\Program Files\mosquitto

   ```
5. Click **OK** to save and close.

---

## 3. Edit the Mosquitto Configuration File

1. Open **Notepad as Administrator**:
   - Click Start → type `notepad`
   - Right-click → **Run as Administrator**
2. Open:
```
C:\Program Files\mosquitto\mosquitto.conf

   ```
3. Find and **uncomment** the following line:
```
#listener
   ```
   - Replace it with:
```
listener 1993
   ```
4. Search for:
```
#allow_anonymous true
   ```
   Uncomment it and change to:
```
allow_anonymous true
   ```
5. Save the file.

---

## 4. Restart Mosquitto Service

Open Command Prompt as Administrator:
```bat
net stop mosquitto
net start mosquitto
```

---

## 5. Configure Windows Firewall Rules

1. Open **Windows Defender Firewall with Advanced Security**.
2. Click **Inbound Rules → New Rule**.
3. Choose **Predefined → Windows Management Instrumentation (WMI)** → click **Next**.
4. Select **Allow the connection**.
5. Add your local IP address (found via `ipconfig /all`).
6. Click Finish.

**Then add a port rule:**
1. New Rule → **Port**.
2. Protocol: **TCP**. Specific port: `1993`.
3. Allow connection → Next.
4. Name it `MQTT` → Finish.

---

## 6. Connect with MQTT Explorer

1. Download MQTT Explorer: https://mqtt-explorer.com
2. Open MQTT Explorer → click **+ Add new connection**.
3. Fill in:
   - **Name**: `MQTT_TEST`
   - **Host**: your local IPv4 address (from `ipconfig /all`)
   - **Port**: `1993`
4. Click **Connect** → try publishing to a topic (e.g. `test/topic`).

---

## 7. Enable Authentication (Disable Anonymous)

1. Edit `mosquitto.conf` again → change:
```
allow_anonymous false
   ```
2. Save the file.
3. Restart Mosquitto:
```bat
net stop mosquitto
net start mosquitto
```

---

## 8. Create Users with Password File (on D: Drive)

1. Open an **Administrator Command Prompt**.
2. Switch to your custom config folder:
```bat
D:
cd /D "D:\My Data\Desktop\TSP-Internship-E4C-2024\Di-Hydro\D2.2\MQTT_Reconfiguration\Mosquitto\config"
```
3. Run Mosquitto’s password utility:
```bat
"C:\Program Files\mosquitto\mosquitto_passwd.exe" -c passwd test_Reconfig
```
4. Enter and confirm a password.

5. Confirm the password file:
```bat
type passwd
```
You should see a line like:
```
test_Reconfig:$6$...hashed_password...
```
6. Open the mosquitto.conf file again in Notepad as administrator.

7. Search for:
```
#password_file
```

8. Uncomment it and set the full path to your `passwd` file:
```
password_file D:\My Data\Desktop\TSP-Internship-E4C-2024\Di-Hydro\D2.2\MQTT_Reconfiguration\Mosquitto\config\passwd
```
---

## 9. Use Username/Password in MQTT Explorer

1. Open MQTT Explorer.
2. Edit your connection:
   - Add **Username**: `test_Reconfig`
   - Add the password you set
3. Save and reconnect.

✅ You should now be securely connected using authentication.

---
