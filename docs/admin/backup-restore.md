# 💾 Backup & Restore

DataLynX includes built-in support for configuration backups, logic processing snapshots, and system restore options.  
These features ensure that the agent can recover from failures, configuration corruption, hardware issues, or accidental misconfiguration without losing critical integration work.

---

# 🧭 1. Overview

DataLynX automatically manages:

- Configuration file backups
- Logic processing state snapshots
- Rotating backup sets
- Safe restore procedures  

Administrators can also manually trigger backups or restore from a previous version using the DataLynX UI.

---

# 💼 2. What Gets Backed Up?

Backups include:

| Component | Description |
|----------|-------------|
| **datalynx.cfg** | Core agent configuration including BACnet networks, mappings, settings |
| **Logic Processing Snapshot** | Saved block states, property containers, watches |
| **Driver State** | Cached device lists, router table, polling metadata |
| **Metadata** | Version, timestamps, backup descriptors |

The backup directory is located at:

```
C:\ProgramData\DataLynX\agents\default\backups\
```

---

# 🛠️ 3. Automatic Backup Behavior

DataLynX maintains **rotating backup sets** to prevent excessive disk usage.  
Backups are triggered automatically when:

- The agent configuration changes
- Logic processing state changes significantly
- A service restart occurs
- Manual backup is triggered from the UI  

You can configure these settings via the **Backup Service** panel.

![Backup Service](../img/System - Backup Service.PNG)

---

# 🧪 4. Manual Backups

To create a manual backup:

1. Navigate to:

```
System → Backup Service
```

2. Click:

```
Create Backup
```

A new backup entry will appear in the list with the timestamp.

Use manual backups when:

- Performing risky configuration changes  
- Editing logic blocks  
- Preparing for a software update  
- Testing complex mappings  

---

# 🔄 5. Restoring from Backup

Restoration allows you to revert the agent to a previously saved working state.

### Steps to restore:

1. **Stop the DataLynX Supervisor Service**  
   via **services.msc**

2. Navigate to:

```
C:\ProgramData\DataLynX\agents\default\backups\
```

3. Choose the backup you want to restore

4. Copy the files into:

```
C:\ProgramData\DataLynX\agents\default\
```

5. **Start the DataLynX Supervisor Service**

6. Open the UI and confirm the agent loads correctly

---

# ⚠️ Important Warnings

### ❗ Never restore while the service is running  
The running agent may overwrite files upon shutdown.

### ❗ Always stop the Supervisor Service  
The backup files must be restored with the agent fully offline.

### ❗ Ensure compatibility  
Restoring very old backups may conflict with new agent versions.

---

# 🛑 6. Resetting the Agent (Alternative to Restore)

A full reset is helpful if:

- The configuration is severely corrupted  
- Networks or mappings are broken beyond repair  
- You want to start from scratch  

Steps:

1. Stop the **Supervisor Service**  
2. Delete:

```
C:\ProgramData\DataLynX\agents\default\datalynx.cfg
```

3. Start the service again  
4. The agent will regenerate a fresh default configuration  

---

# 📝 Summary

DataLynX’s backup and restore system ensures:

- Reliable configuration recovery  
- Protection from corruption or hardware failure  
- Safe experimentation with logic and mappings  
- Fast return to a known good state  

Use backups frequently—especially before major changes.

---

## ➡️ Next Step

Proceed to:

👉 **Administration → Resetting the Agent Config**  
to learn how to safely perform a clean configuration reset.

