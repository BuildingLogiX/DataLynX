## Security & User Management

DataLynX provides a simple but effective security model designed for on-premises building automation environments.  
This guide explains how authentication works, how to manage user accounts, and best practices for securing your installation.

---

## 1. Overview

Security features include:

- Username/password authentication  
- Two user roles (User & Supervisor/Admin)  
- Token-based API authentication (JWT)  
- Role-based UI permissions  
- Ability to create, edit, and delete users  
- Logging for authentication and access events  
- Network access restrictions for secure environments  

Security in DataLynX is intentionally lightweight but robust enough for operational environments.

---

## 2. User Roles

DataLynX supports **two primary user roles**:

## **1. User (Standard User)**

- Can log in to the UI  
- Can view devices, points, mappings, and logic  
- Cannot modify system configuration  
- Cannot manage users  
- Cannot restart services  
- Cannot edit BACnet or BasiX settings  

This role is ideal for:

- Operators  
- Engineers in monitoring-only mode  
- Contractors performing read-only inspections  

---

## **2. Supervisor (Administrator)**
Full access, including:

- Add/edit/delete users  
- Create or modify BACnet networks  
- Configure BasiX devices  
- Modify mappings  
- Add logic in Flow View  
- Reset the agent configuration  
- Create or restore backups  
- Configure BDX communication  
- Change system settings (ports, themes, etc.)  

This role is required for commissioning and integration work.

---

## 3. Authentication Model

### ✔ Username + Password  
All users must authenticate with a unique username and password.

### ✔ Token-Based Access (JWT)  
Once authenticated:

- The UI receives a JSON Web Token (JWT)
- Token is required for API calls
- Token expires after a fixed interval for security

### ✔ Password Requirements  
Admin can set the complexity level (depends on version):

- Minimum length  
- Case sensitivity  
- Required special characters  

---

## 4. User Management Interface

Navigate to:

```
System → Users
```

### User Overview  
Shows all existing accounts with:

- Username  
- Role (User / Supervisor)  
- Status (active/disabled)  

![User Overview](../img/Manage Users - Overview.PNG)

### User Details  
Used for editing:

- Password  
- Role  
- Account status  
- Metadata  

![User Details](../img/Manage Users - Details.PNG)

---

## 5. Adding a New User

1. Open:

```
System → Users → Add User
```

2. Enter:

- Username  
- Password  
- Role  

3. Save

The user can now log in immediately.

---

## 6. Deleting or Disabling Users

You can remove users entirely or disable them temporarily.

### Disabling is recommended when:

- Employees leave temporarily  
- Contractors complete their work  
- Access needs to be paused without deleting history  

---

## 7. Auditing & Logging

Authentication events are logged, including:

- Successful logins  
- Failed logins  
- Invalid tokens  
- Configuration changes (admin actions)

Logs are stored under:

```
C:\ProgramData\DataLynX\agents\default\logs\
```

Use these logs to review suspicious activity or access issues.

---

## 8. Security Best Practices

### ✔ Restrict network access to trusted subnets
### ✔ Always assign strong passwords  
### ✔ Restrict Supervisor role to trusted users  
### ✔ Rotate passwords frequently  
### ✔ Keep OS and firewall rules updated  
### ✔ Use backups before major config changes  
### ✔ Limit network exposure of the web UI port  

---

## Summary

DataLynX provides:

- Simple authentication  
- Clear role-based access control  
- Logging for accountability  
- Recommended practices for secure deployments  

Together, these tools ensure your Agent and building integration remain protected.

---

## ➡️ Next Step

Proceed to the **Reference Section**, starting with:

👉 **reference/bacnet-driver.md**

