# ![Windows Logo](../assets/windows_logo.svg){ width="40" } Windows Services & Logs

DataLynX runs as a pair of Windows Services that must be operational for BACnet communication, the Web UI, BasiX mapping, and BDX communication to function properly.  
This guide explains where these services run, how to verify they are healthy, and where to find logs and configuration files.

---

## ⚙️ 1. DataLynX Windows Services

When installed via the `.msi` installer, DataLynX creates **two** Windows services:

### ✅ **1. DataLynX Supervisor Service**

- Manages the Agent runtime
- Handles data processing and logic state
- Oversees BACnet drivers  
- Performs communication handling for BDX  
- Helps recover gracefully from errors or faults  

### ✅ **2. DataLynX Web Service**

- Hosts the Web UI (HTTP interface)  
- Exposes REST API endpoints  
- Allows you to configure networks, users, mappings, and system settings  
- Must be running to access the UI at `http://localhost` (port 80, or alternatively port 8050 or 8020)

---

## 🔍 2. Viewing Services in Windows

To view and manage DataLynX services:

1. Press **Win + R**  
2. Type:

```
services.msc
```

3. Locate the following entries:

- **DataLynX Supervisor**
- **DataLynX Web**

![Windows Services](../img/DataLynX Windows Services.PNG)

### Both should be:

- **Status:** Running  
- **Startup Type:** Automatic  

---

## 🟢 3. Restarting or Controlling Services

Right-click any service to:

- Start  
- Stop  
- Restart  
- View properties  

Restarting services is often useful after modifying configuration files.

> ⚠️ Important:  
> If you edit or delete the config file **while the service is running**, the service may overwrite it when shutting down.  
> Always stop the Supervisor service before making major config edits.

---

## 📁 4. File Locations

The DataLynX agent stores config files, logs, snapshots, and backups under:

```
C:\ProgramData\DataLynX\agents\default\
```

### Key directories:

| Purpose | Path |
|--------|------|
| **Main Config File** | `agents\default\datalynx.cfg` |
| **Logs Directory** | `agents\default\logs\` |
| **Snapshots** | `agents\default\snapshots\` |
| **Backups** | `agents\default\backups\` |

![Config Files](../img/Windows - Agent Config Files.PNG)

---

## 🗂️ 5. Log Files

Logs are located under:

```
C:\ProgramData\DataLynX\agents\default\logs\
```

![Agent Log Files](../img/Windows - Agent Log Files.PNG)

Typical logs include:

- **datalynx.log**  
  Main log for driver events, network discovery, status, and engine operations.

- **bdx.log**  
  BDX communication events, authentication, data pump activity.

- **http.log**  
  Web service activity and HTTP API events.

Logs are rotated automatically and timestamped for easier debugging.

---

## 🧪 6. What to Look For in Logs

Logs can help troubleshoot:

- BACnet communication issues  
- BDX authentication failures  
- Configuration errors  
- Device offline events  
- Polling or routing failures  
- Evaluation engine execution problems  

### Common entries you may see:

- `Started DataLynX Supervisor`  
- `BACnet driver initialized`  
- `BDX connection established`  
- `Device offline: <DeviceName>`  
- `Router table updated`  
- `Polling interval = ...`  

---

## 📦 7. Backup & Restore

DataLynX maintains rotating backups of:

- The configuration file (`datalynx.cfg`)  
- Evaluation engine snapshot  
- Driver state  

You can view the backup settings under:

```
System → Backup Service
```

![Backup Service](../img/System - Backup Service.PNG)

Restoration involves:

1. Stopping services  
2. Copying backup files into the active config directory  
3. Restarting services

---

## 🛑 8. Resetting Configuration (Safely)

To reset the DataLynX configuration:

1. Stop **DataLynX Supervisor Service**  
2. Delete:

```
C:\ProgramData\DataLynX\agents\default\datalynx.cfg
```

3. Start the service again  
4. The agent will recreate a new, clean config file

> ⚠️ Do **NOT** delete the config while the service is running —  
> it will be rewritten from memory during shutdown.

---

## 📝 Summary

The Windows Services & Logs section helps you:

- Ensure the Supervisor and Web services are running  
- Quickly find logs and config files  
- Diagnose communication or mapping issues  
- Manage backups and restore states  
- Safely reset configuration when needed  

These tools form the backbone of DataLynX system administration.

---

## ➡️ Next Step

Move on to:

👉 **[Backup & Restore](backup-restore.md)**
to learn how DataLynX preserves configuration and processing state.

