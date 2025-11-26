# 🧹 Resetting the Agent Configuration

There may be scenarios where you need to reset the DataLynX Agent back to a clean, default state.  
This procedure is safe when done correctly and is often used when:

- The configuration becomes corrupted  
- Mappings or logic flows are severely broken  
- BACnet networks were misconfigured  
- You want to completely rebuild the integration  
- A test environment needs to be reset  
- Restoring from backup is not desired  

This guide explains **exactly how to perform a safe reset** and avoid accidentally overwriting changes.

---

# ⚠️ 1. Important Warnings

### ❗ The Agent must NOT be running when resetting
If you delete `datalynx.cfg` while the Supervisor Service is running:
- The service may rewrite the file during shutdown  
- You may lose the intended reset  
- You risk generating partial or invalid configuration states  

### ❗ Always stop the Supervisor Service first  
This ensures the agent cannot write to the config directory.

### ❗ Make a backup before resetting  
Even if you don’t plan to restore the old configuration, keeping a copy is good practice.

---

# 🧭 2. Reset Procedure (Safe Method)

### Step 1 — Open Windows Services

1. Press **Win + R**
2. Type:

```
services.msc
```

3. Locate:

- **DataLynX Supervisor**
- **DataLynX Web**

### Step 2 — Stop the Supervisor Service

Right‑click:

```
DataLynX Supervisor → Stop
```

> 🟡 You may also stop the Web Service, but it is not required for config reset.

---

### Step 3 — Navigate to the Agent Directory

Go to:

```
C:\ProgramData\DataLynX\agents\default\
```

This folder contains:

- `datalynx.cfg` (core configuration)
- `logs/`
- `backups/`
- `snapshots/`

> 📁 You can view the directory structure here:
>
> ![Config Files](../img/Windows - Agent Config Files.PNG)

---

### Step 4 — Back Up the Existing Config (Recommended)

Copy the file:

```
datalynx.cfg
```

to a safe location.

---

### Step 5 — Delete the Config File

Delete:

```
datalynx.cfg
```

This wipes all user configuration including:

- BACnet networks
- Hosted device settings
- Mappings
- Logic processing state
- User roles and credentials (depending on version)
- BasiX devices  

---

### Step 6 — Restart the Supervisor Service

Go back to **services.msc** and Start:

```
DataLynX Supervisor
```

Upon startup, the agent will:

- Detect the missing configuration
- Generate a brand‑new default `datalynx.cfg`
- Initialize with a clean state
- Load default BACnet driver and data processing settings  

Once complete, you can log into the UI and begin a fresh setup.

---

# 🧪 3. Verifying the Reset

After restart, confirm:

### ✔ A brand‑new config file was created  
Timestamp should reflect the restart time.

### ✔ No previous networks or mappings appear  
BACnet networks and BasiX devices will be empty.

### ✔ Logs show initialization messages:
- `Initializing agent...`
- `Loading default configuration…`
- `BACnet driver started`

### ✔ UI loads without errors
Navigate to:

```
http://localhost
```

(Port 80 by default, or port 8050/8020 if configured differently)

---

# 🛑 4. What NOT to Delete

Do **NOT** remove:

- `logs/` directory  
- `backups/` directory  
- `snapshots/` directory  
- `driver` folders  
- Any executable or DLL files under Program Files  

These are managed by the system and should not be tampered with.

---

# 🔄 5. Optional: Full Hard Reset

If you want to completely wipe the agent state:

1. Stop both **Supervisor** and **Web** services  
2. Delete the entire folder:

```
C:\ProgramData\DataLynX\agents\default\
```

3. Restart services  
4. DataLynX will recreate all needed directories and files

Only use this method for:

- Test environments  
- Lab deployments  
- Pre‑commissioning resets  

Avoid this for production unless absolutely necessary.

---

# 📝 Summary

Resetting the configuration is safe when done correctly:

1. **Stop the Supervisor Service**  
2. **Delete `datalynx.cfg`**  
3. **Restart the Supervisor Service**  
4. **Agent recreates a fresh config**  

This allows you to quickly return to a clean state and rebuild your BACnet or BasiX integration from scratch.

---

## ➡️ Next Step

Proceed to:

👉 **Administration → Security & User Management**  
to learn how DataLynX handles authentication, roles, and user permissions.

