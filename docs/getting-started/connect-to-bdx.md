# 🔗 Connecting DataLynX to BDX

This guide explains how to connect your local DataLynX Agent to the **BuildingLogiX Data eXchange (BDX)** platform.
Once connected, DataLynX will begin securely transmitting normalized point data (BasiX) to BDX for analytics, dashboards, alarming, and long‑term storage.

**Note:** BDX can be deployed as a cloud-based service or on-premise within your organization's infrastructure.

---

## 📘 1. Overview

DataLynX communicates with BDX using a secure, authenticated connection.  
Each local Agent appears as a managed device inside BDX.

To connect the Agent, you will:

1. Configure the Agent URL in BDX  
2. Provide authentication credentials  
3. Enable the Agent in both DataLynX and BDX  
4. Verify communication health

BDX supports multiple Agents, and one Agent may serve many buildings or systems.

---

## 🌐 2. Prerequisites

Before connecting, ensure:

- DataLynX installation is complete  
- Both **DataLynX Supervisor** and **DataLynX Web Service** are running  
- You can access the UI at `http://localhost` (port 80, or alternatively port 8050 or 8020 if port 80 is unavailable)  
- BACnet network is configured and functioning (optional but recommended)  
- You have a BDX account with permission to add or manage agents  

---

## 🔧 3. Configure BDX to Recognize Your Agent

1. Log in to **BDX**.
2. Navigate to:

```
Management → Agents
```

3. Click **Add Agent**.

4. When prompted for the Agent URL, enter:

```
http://<your-agent-ip>/agent/bdxAgent
```

### ✔ Important Notes

- `/agent/bdxAgent` is required — this is the DataLynX API endpoint.
- Default port is 80. If port 80 is unavailable, you can use port 8050 or 8020 instead.
- For local machines, you may use:

```
http://localhost/agent/bdxAgent
```

- If the agent is on a remote device (ex: server, gateway, VM), enter its LAN or WAN IP.

5. BDX will attempt to contact the Agent and request authentication.

---

## 🔑 4. Authentication Setup

BDX and DataLynX exchange:

- Username
- Password
- Token authorization (generated automatically after connection)

In the DataLynX UI:

1. Navigate to:

```
Connections → BDX
```

2. Enter the following details:
   - **BDX Server URL**
   - **Username**
   - **Password**

![BDX Agent Configuration](../img/Connections - BDX - Agent Config.PNG)

3. Save the configuration.

4. Enable the connection using the **Enable BDX** toggle.

---

## 🔄 5. Establishing Communication

Once both sides (BDX and DataLynX) have matching credentials and agent URLs:

- DataLynX registers itself with BDX  
- BDX validates the Agent identity  
- A persistent, secure channel is created for sending:
  - BasiX point updates  
  - Device health events  
  - Alarms and faults  
  - Logs (if configured)

You should now see the Agent listed in the BDX Agent screen with status indicators such as:

| Status | Meaning |
|--------|--------|
| 🟢 **Online** | Agent is actively sending data |
| 🟡 **Warning** | Intermittent communication |
| 🔴 **Offline** | Agent unreachable for extended time |

---

## 🧪 6. Validate the Connection

You can confirm the connection by checking:

### **BDX Side**  
- The Agent appears under **Management → Agents**  
- Device list begins populating under **Devices**  
- BasiX profiles show up as they are mapped and sent  

### **DataLynX Side**  
Navigate to:

```
System → Logs → BDX Communication
```

You should see log entries for:

- Registration  
- Authentication success  
- Device and point uploads  
- Any communication warnings or failures  

---

## 🔒 7. Security Best Practices

For production environments:

- Restrict inbound access to the web port (80, 8050, or 8020) to trusted networks
- Use random, complex passwords for BDX credentials
- Ensure Windows firewall allows only required inbound traffic

---

## 🚑 8. Troubleshooting

### ❌ "Agent Unreachable" in BDX
- Verify firewall rules for the configured web port (80, 8050, or 8020)
- Confirm correct agent URL (`/agent/bdxAgent`)
- Ensure the DataLynX Web Service is running  

### ❌ Authentication failures  
- Check credentials in `System → BDX Connection`  
- Update password in both BDX and DataLynX  
- Restart the DataLynX Supervisor Service  

### ❌ No data flowing to BDX  
- Ensure BasiX mappings are present  
- Make sure polling is active in BACnet networks  
- Verify device discovery is completed  

---

## 🎉 Next Step

Move on to:

👉 **Concepts → Architecture**  
to understand how DataLynX communicates internally through its data transformation layer, BasiX layer, and BACnet driver.

