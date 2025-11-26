# 🛠️ Installation & Windows Services

This guide walks you through installing **DataLynX**, understanding the required Windows services, and verifying the system is running correctly. This is the recommended first stop after setting up your GitHub-based documentation.

---

## 📥 1. Install DataLynX

DataLynX is distributed as a Windows **.msi installer**.

### **Steps:**

1. Download the installer from BuildingLogiX (internal distribution or BDX portal).
2. Double‑click the `.msi` file.
3. Follow the prompts:
   - Accept the license
   - Choose installation directory (default recommended)
   - **Configure Web Port** (if prompted):
     - Default is port **80**
     - If port 80 is already in use by another application (like IIS, Apache, or another web server), choose an alternative port such as **8050** or **8020**
     - Make note of the port you select for browser access and firewall configuration
   - Proceed with installation

### **What the installer creates**

The installer sets up the following:

- Program files (typically under `C:\Program Files\DataLynX`)
- Configuration directory and default agent:
  ```
  C:\ProgramData\DataLynX\agents\default\
  ```
- Windows Services (see next section)
- Web UI host
- Log directories

---

## ⚙️ 2. Required Windows Services

DataLynX installs **two** Windows services.
Both must be running for the UI to be accessible.

### ✅ **DataLynX Supervisor Service**
- Manages communication between the agent and BDX
- Handles remote management commands
- Provides the infrastructure for the agent to run
- Process: `supervisor.exe`

### ✅ **DataLynX Web Service**
- Hosts the web-based user interface
- Provides local configuration and diagnostics
- Required for logging into the UI via browser
- Process: `agent.exe`

### 📍 How to check services

1. Open **Services.msc**
2. Look for:
   - `DataLynX Supervisor Service`
   - `DataLynX Web Service`
3. Ensure both are:
   - **Status:** Running
   - **Startup Type:** Automatic

If needed:
- Right‑click → **Start**
- Right‑click → **Restart**

---

## ⚠️ **IMPORTANT: Services vs Agent**

**The Windows Services and the DataLynX Agent are different:**

- **Windows Services** (DataLynX Supervisor Service & DataLynX Web Service) provide the infrastructure and UI access
  - Run as Windows processes: `supervisor.exe` and `agent.exe`
- **The DataLynX Agent** (controlled from the UI) runs the actual BACnet driver, data processing, and BuildingLogiX processes

**You can have the Windows Services running but the DataLynX Agent stopped.**

The DataLynX Agent is controlled from the **Supervisor** page in the UI (not from Windows Services). See Section 4 below for how to start/stop the DataLynX agent.

---

## 🔥 3. Firewall Configuration

**IMPORTANT:** DataLynX requires firewall configuration to access the Web UI from a browser.

### Windows Firewall Setup

The installer may automatically create firewall rules, but if the UI is not accessible, you need to manually configure Windows Firewall:

#### Option 1: Using Windows Defender Firewall (GUI)

1. Press **Win + R** and type:
   ```
   wf.msc
   ```

2. Click **Inbound Rules** in the left pane

3. Click **New Rule...** in the right pane

4. Select **Port** and click **Next**

5. Select **TCP** and enter the port number:
   - Enter **80** (or **8050** or **8020** if you chose an alternative during installation)
   - Click **Next**

6. Select **Allow the connection** and click **Next**

7. Check all profiles (Domain, Private, Public) and click **Next**

8. Name the rule:
   ```
   DataLynX Web UI
   ```

9. Click **Finish**

#### Option 2: Using PowerShell (Quick Method)

Open PowerShell as Administrator and run:

```powershell
# For port 80 (default)
New-NetFirewallRule -DisplayName "DataLynX Web UI" -Direction Inbound -Protocol TCP -LocalPort 80 -Action Allow

# OR for port 8050
New-NetFirewallRule -DisplayName "DataLynX Web UI" -Direction Inbound -Protocol TCP -LocalPort 8050 -Action Allow

# OR for port 8020
New-NetFirewallRule -DisplayName "DataLynX Web UI" -Direction Inbound -Protocol TCP -LocalPort 8020 -Action Allow
```

### Testing Firewall Access

After configuring the firewall:
- From the local machine: Open a browser and navigate to `http://localhost`
- From another machine on the network: Use `http://<server-ip-address>` (using the configured port if not 80)

---

## 🌐 4. Accessing the Web UI

After installation and service startup:

1. Open a browser
2. Navigate to:

```
http://localhost
```

Or if using an alternative port:

```
http://localhost:8050
```

**Default Credentials:**

DataLynX ships with default login credentials provided by BuildingLogiX. Use these credentials for first login:
- Username: *[Provided by BuildingLogiX during installation/setup]*
- Password: *[Provided by BuildingLogiX during installation/setup]*

**IMPORTANT**: After first login, you should change the default password for security.

### Initial Login and Agent Startup

When you first log in, you will land on the **Supervisor** page. This is the administrative control interface where you start/stop the agent and view its status.

![Supervisor Page](../img/Supervisor.PNG)

**⚠️ Critical Understanding:**
- The **Windows Services** provide infrastructure and UI access
- The **Agent** runs the BACnet driver, data processing, and BuildingLogiX processes
- The services can be running while the agent is stopped
- You control the agent from the Supervisor page (NOT from Windows Services)

**If the agent is not running:**
1. You will see a **Start** button on the Supervisor page
2. The status will show "Stopped" or "Not Running"
3. Click **Start** to launch the agent
4. Wait for the agent to initialize (BACnet driver, data processing, etc.)
5. Once started, the full DataLynX Explorer tree will become visible, showing:
   - BACnet networks and devices
   - BasiX profiles
   - System configuration
   - User management

**If the agent is already running:**
- The status will show "Running"
- The full DataLynX Explorer tree will be immediately visible
- You can navigate through all features and begin configuration

**To stop the agent:**
- Click the **Stop** button on the Supervisor page
- This stops the BACnet driver and data processing (but leaves Windows Services running)

---

## 📁 5. Important Folders

| Purpose | Path |
|--------|------|
| Agent Config | `C:\ProgramData\DataLynX\agents\default\datalynx.cfg` |
| Agent Logs | `C:\ProgramData\DataLynX\agents\default\logs\datalynx.log` |
| Backup Snapshots | `C:\ProgramData\DataLynX\agents\default\backups\` |
| Services Executables | `C:\Program Files\DataLynX\` |

**Note:**  
If you delete `datalynx.cfg` while the agent is running, it will **rebuild the file on shutdown** using in‑memory settings. Always **stop the service first** before deleting the config to perform a reset.

---

## 🧪 6. Verify Successful Installation

Perform the following checks:

### ✔ Services Running
Both DataLynX services must show "Running".

### ✔ UI Loads Successfully
1. Visit: `http://localhost` (port 80, or 8050/8020 if configured)
2. Login with default credentials provided by BuildingLogiX
3. You should see the Supervisor interface

### ✔ Agent Starts Successfully
1. In the Supervisor view, click **Start** (if agent is not already running)
2. Wait for the agent to start (may take a few seconds)
3. Verify the full DataLynX Explorer tree appears with:
   - BACnet section
   - BasiX section
   - System section
   - Users section

### ✔ Log Activity
Open:
`C:\ProgramData\DataLynX\agents\default\logs\datalynx.log`
You should see entries such as:
- Service startup
- Driver initialization
- Network listener opening
- Agent started successfully

### ✔ Default Agent Exists
Ensure the folder exists:

```
C:\ProgramData\DataLynX\agents\default\
```

If all checks pass, your installation is complete and you're ready to configure BACnet networks.

---

## 🚑 7. Troubleshooting

### **UI will not load**
- Ensure **DataLynX Web Service** is running in Windows Services
- **Check Windows Firewall** - verify inbound rule exists for the web port (see section 3 above)
- Verify no other application is using the same port
- Check the configured port in `datalynx.cfg`
- Try accessing from localhost first: `http://localhost`
- Look for errors in: `C:\ProgramData\DataLynX\agents\default\logs\http.log`

### **Services fail to start**
- Check Event Viewer → Windows Logs → Application
- Ensure .NET and Visual C++ runtimes are installed
- Look for permission issues in `ProgramData\DataLynX`
- Verify both services are set to Automatic startup
- Check Task Manager → Details for `supervisor.exe` and `agent.exe` processes

### **Logs show no activity**
- The service may not have write access to the log directory
- Check folder permissions under `C:\ProgramData`

---

## 🎉 You're Ready for the Next Step

Move on to:

👉 **Getting Started → First BACnet Network**  
to configure your hosted device, add networks, and begin discovery.

