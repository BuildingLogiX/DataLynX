# ![DataLynX Logo](../assets/datalynx_logo.svg){ width="50" } Resetting the Agent Configuration

!!! danger "DESTRUCTIVE OPERATION - READ CAREFULLY"
    **Resetting the configuration will PERMANENTLY DELETE all your DataLynX settings.** This includes:

    - **All BACnet network configurations**
    - **All discovered devices and points**
    - **All BasiX mappings and profiles**
    - **All Flow View logic and calculations**
    - **All user accounts and credentials**
    - **All custom settings and preferences**

    **This action cannot be undone.** You will be starting completely from scratch.

---

## When to Reset

Reset is a **last resort** option, typically used only when:

- The configuration is severely corrupted and cannot be restored from backup
- Mappings or logic flows are so broken that rebuilding is faster than fixing
- You intentionally want to completely wipe the system and start fresh
- A test environment needs to be cleared for new testing

**Before resetting, consider:**

- Can you restore from a backup instead? See [Backup & Restore](backup-restore.md)
- Can you fix the specific issue without losing everything?
- Have you exported any configurations you might need later?

---

## 1. Critical Warnings

!!! warning "STOP - Back Up First!"
    **Before proceeding, you MUST manually back up your configuration files if you want any chance of recovery.**

    1. **Use the UI backup feature first:** Go to **System → backup_service** and trigger a manual backup
    2. **Copy the .cfg file:** Navigate to `C:\ProgramData\DataLynX\agents\default\` and copy `datalynx.cfg` to a safe location
    3. **Copy the entire folder (recommended):** Copy the entire `default` folder including `backups/` and `snapshots/` subdirectories

### ❗ The Agent must NOT be running when resetting

If you delete `datalynx.cfg` while the Supervisor Service is running:

- The service may rewrite the file during shutdown
- You may lose the intended reset
- You risk generating partial or invalid configuration states

### ❗ Always stop the Supervisor Service first

This ensures the agent cannot write to the config directory.

### ❗ There is NO automatic recovery

Once you delete the configuration and restart the service, the old configuration is **gone forever** unless you have a backup copy.

---

## 2. Reset Procedure (Safe Method)

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

!!! danger "Point of No Return"
    **This step will permanently delete your configuration.** Make absolutely sure you have backed up your files before proceeding. Once deleted and the service restarts, your old configuration cannot be recovered.

Delete:

```
datalynx.cfg
```

This **permanently wipes** all user configuration including:

- All BACnet networks and device configurations
- Hosted device settings
- All BasiX mappings and profiles
- All Flow View logic and calculations
- User roles and credentials
- All custom settings  

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

## 3. Verifying the Reset

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

## 4. What NOT to Delete

Do **NOT** remove:

- `logs/` directory  
- `backups/` directory  
- `snapshots/` directory  
- `driver` folders  
- Any executable or DLL files under Program Files  

These are managed by the system and should not be tampered with.

---

## 5. Optional: Full Hard Reset

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

## Summary

Resetting the configuration is safe when done correctly:

1. **Stop the Supervisor Service**  
2. **Delete `datalynx.cfg`**  
3. **Restart the Supervisor Service**  
4. **Agent recreates a fresh config**  

This allows you to quickly return to a clean state and rebuild your BACnet or BasiX integration from scratch.

---

## ➡️ Next Step

Proceed to:

👉 **[Security & User Management](security-users.md)**
to learn how DataLynX handles authentication, roles, and user permissions.

