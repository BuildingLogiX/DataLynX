# Setting Up Your First BACnet Network

This guide walks you through configuring DataLynX for BACnet/IP communication, creating your first network, discovering devices, and validating point data.

---

## 1. Overview

After installing DataLynX and ensuring services are running, your next step is to configure your first **BACnet network**.  
This process includes:

1. Configuring the **Hosted BACnet Device**  
2. Creating a **BACnet/IP Network**  
3. Performing **device discovery**  
4. Reviewing discovered devices and point properties  
5. Validating polling and value updates  

This is the essential foundation for all later mapping and BasiX modeling.

---

## 2. Configure the Hosted Device

The **hosted device** is DataLynX’s identity on the BACnet network.

1. Open the DataLynX UI  
2. Navigate to:

```
Connections → BACnet → configuration → Hosted Device
```

3. Configure the following fields:

| Field | Description |
|-------|-------------|
| **Device Name** | Friendly name of the agent |
| **Device ID** | Must be unique on the BACnet network |
| **Vendor ID** | Defaults to BuildingLogiX |
| **Model / Firmware Info** | Optional but recommended |
| **Max APDU** | Leave default unless required otherwise |

### ✔ Recommendations

- Use an available Device ID range, for example in the **1530000–1530999**  
- **Device ID must be unique** on the BACnet network — verify availability before use
- Recommended Device Name: **DataLynX**
- If using multiple agents, assign each a different Device ID (e.g., 1530001, 1530002)
- Do not reuse the device ID of a field controller or plant controller  

---

## 3. Add a BACnet/IP Network

1. Navigate to:

```
Connections → BACnet → configuration → bacnet_networks
```

2. Add a new BACnet/IP Network

3. Configure the required fields:

![BACnet IP Network Configuration](../img/BACnet - Configuration - BACnet IP Network - configuration.PNG)

| Setting | Description |
|--------|-------------|
| **UDP Port** | Typically **47808** (0xBAC0). Must match field devices. |
| **Network Number** | Required for routing. Must be unique per network segment. |
| **Interface / IP Address** | The NIC the agent should bind to. |
| **Device Type** | Standard Device, Foreign Device, or BBMD participant. |
| **Foreign Device Lifespan** | Only applicable if using FD mode. |

### ✔ Important Notes

- Use **decimal** for the UDP port (e.g., 47808), *not* hex notation.  
- If you are on a routed network, ensure router tables eventually appear.  
- Only enable Foreign Device mode if joining a remote BBMD.

After saving, make sure the network shows as **Enabled**.

---

## 4. Configure Polling (Optional for First Setup)

You may keep default polling settings.

Defaults include:

- Poll interval for analog values  
- Timeout settings  
- Stale-time logic  
- Fallback behavior for unresponsive devices  

Polling can be modified later under:

```
Connections → BACnet → Networks → Poll Service
```

---

## 5. Discover Devices

Now you are ready to find controllers on the network.

1. Go to:

```
Connections → BACnet → Devices
```

2. Click:

```
Discover → Who-Is
```

3. (Optional) Use **Ranged Who-Is** for large networks.

### During discovery, DataLynX will:

- Send Who-Is requests  
- Listen for I-Am responses  
- Auto-populate the device list  
- Build router table entries as routers respond  

### After discovery, you should see:

- Device name
- Device ID
- Network number
- MAC address
- Vendor information

### Organizing Devices with Folders

By default, all discovered devices appear under the top-level `devices` node. For larger sites, this can become difficult to manage.

**Best Practice:** Use **BACnet Device Folders** to organize controllers logically:

- **By building or floor:** `Building-A`, `Floor-1`, `Floor-2`
- **By system type:** `AHUs`, `VAVs`, `Chillers`, `Lighting`
- **By network segment:** `Network-1`, `MSTP-Trunk-A`

**To create a device folder:**

1. Right-click on `devices` node
2. Select **Add → BACnet Device Folder**
3. Name the folder appropriately
4. Drag discovered devices into the folder, or run discovery with the folder selected

**Why organize devices?**

- Easier navigation on sites with 50+ controllers
- Logical grouping matches your building structure
- Simplifies BasiX mapping workflows
- Cleaner hierarchy in the eXplorer tree
- Better team collaboration when multiple users work on the same agent

!!! tip "Plan your folder structure before large discoveries"
    On sites with many controllers, create your folder structure first, then discover devices directly into the appropriate folders. This saves time compared to reorganizing later.

---

## 6. View Device Details

Click any discovered device to open the **device detail view**, where you can:

- Browse object lists  
- View all BACnet properties  
- Monitor live values  
- Check faults and reliability fields  
- Inspect priority array values  
- Look at device status and health

You can also add devices manually if needed:

```
Devices → Add Device
```

Manual addition is useful for MSTP devices behind remote routers.

---

## 7. Validate Point Data

Choose a device and open its **Point List**.  
Verify:

- Values update over time  
- Units appear correctly  
- Reliability = “No Fault Detected”  
- Status is healthy  
- Analog values look reasonable  
- Binary values respond when the device changes state  

If values do not change:

- Confirm polling is enabled  
- Check that the device is reachable  
- Ensure no firewalls are blocking UDP 47808  
- Verify router table entries exist for remote networks  

---

## 8. Router Table Verification (If Using Routers)

Navigate to:

```
Connections → BACnet → Configuration → Router Table
```

![Router Table Configuration](../img/BACnet - Configuration - router table.PNG)

If routers are present on your network:

- Entries will populate automatically from I-Am-Router-To-Network messages
- You may add manual entries if network topology requires it
- Remote stations (for MSTP) may appear here as well

### Pinning Router Table Entries

Router table entries learned automatically can be purged or overwritten when routers restart or network conditions change. To prevent this:

**Use the "Pinned" flag** on critical router entries:

1. Select the router table entry
2. Enable the **Pinned** checkbox
3. Save the configuration

**When to pin entries:**

- Static routes that should never change
- Manual overrides for complex network topologies
- Entries for routers that may temporarily go offline
- Critical paths to MSTP trunks or remote networks

**Pinned entries will:**

- Never be automatically purged
- Not be overwritten by learned routes
- Persist across agent restarts
- Take priority over dynamically learned entries

!!! warning "Use pinning carefully"
    Only pin entries you're certain are correct. Incorrect pinned routes can prevent communication with devices on those networks.

---

## 9. Next Steps

After discovery and point validation, you can proceed with:

👉 **[Connect to BDX](connect-to-bdx.md)**
to configure the agent connection to the BuildingLogiX Data eXchange.

👉 **[BasiX Mapping Workflow](../user-guide/basix-mapping-workflow.md)**
to normalize your BACnet points into standardized BasiX devices and send them to BDX.

---

