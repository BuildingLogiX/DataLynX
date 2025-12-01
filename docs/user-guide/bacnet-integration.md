# ![DataLynX Logo](../assets/datalynx_logo.svg){ width="50" } BACnet Integration Workflow

This guide walks through a complete **end-to-end BACnet integration** in DataLynX — from enabling the driver, to discovering devices, to validating live values and preparing for BasiX mapping.

Use this as a practical checklist for new deployments.

---

## 🧭 1. Prerequisites

Before starting:

- DataLynX is installed and both services are **Running**  
- You can access the UI at `http://localhost` (port 80, or alternatively port 8050 or 8020 if port 80 is unavailable)  
- At least one BACnet/IP network segment is reachable from the agent  
- You have basic network information:
  - UDP port (typically `47808`)  
  - Network number(s)  
  - Router / BBMD details (if applicable)

If you haven’t done so yet, complete:

- **Getting Started → Installation & Windows Services**  
- **Getting Started → First BACnet Network**

---

## ⚙️ 2. Enable the BACnet Driver

1. Open the DataLynX UI.
2. In the Explorer tree, locate:

   ```
   Drivers → BACnet
   ```

3. Confirm the driver is **Enabled**.
4. Check driver status for any startup errors.

![BACnet Driver](../img/BACnet.PNG)

If the driver is disabled, enable it, then refresh the view.

---

## 🏠 3. Configure the Hosted BACnet Device

The hosted device is the identity of the DataLynX agent on the BACnet network.

1. Navigate to:

   ```
   BACnet → Hosted Device
   ```

2. Configure:

   - **Device Name** – Friendly name (e.g., `DLX-Agent-1`)
   - **Device ID** – Unique across the BACnet internetwork  
   - **Vendor / Model Info** – Optional but recommended
   - **Max APDU / Segmentation** – Leave defaults unless required

![Hosted Device](../img/BACnet - Configuration - hosted device.PNG)

> 💡 Tip: Use a Device ID range that does not overlap with field controllers or existing gateways.

---

## 🌐 4. Configure BACnet/IP Network(s)

1. Navigate to:

   ```
   BACnet → Configuration → BACnet IP Network
   ```

2. Add or edit a **BACnet IP Network**:

   - **UDP Port**: Typically `47808`  
   - **Interface / IP**: NIC or IP for the agent host  
   - **Network Number**: BACnet network number (unique per segment)  
   - **Device Type**:  
     - Standard Device  
     - Foreign Device (if registering with a remote BBMD)  
   - **FD Registration**: If using Foreign Device, set BBMD address and TTL.

![BACnet IP Network](../img/BACnet - Configuration - BACnet IP Network.PNG)

3. Save and ensure the network shows as **Enabled**.

> ⚠️ Make sure firewalls allow UDP on the selected port.

---

## ⏱️ 5. Configure Polling & Health Monitoring

### Poll Service

1. In the same BACnet IP Network view, open the **Poll Service** tab.

![BACnet Poll Service](../img/BACnet - Configuration - BACnet IP Network - poll service.PNG)

2. Configure:

   - Global poll interval(s)  
   - Stale-time rules  
   - Timeout and retry behavior

### Health Monitoring

1. Open the **Health Monitoring** tab.

![BACnet Health Monitoring](../img/BACnet - Configuration - health_monitoring.PNG)

2. Configure:

   - Device offline thresholds  
   - Retry / backoff strategies  
   - Fault flags for unreachable devices

> 💡 A good starting point is a moderate poll interval and conservative offline timeout until you understand network stability.

---

## 🔍 6. Device Discovery

1. Navigate to:

   ```
   BACnet → Devices
   ```

2. Click **Discover** and choose:

   - **Who-Is** for broad discovery  
   - **Ranged Who-Is** for large networks or targeted ranges

![Device Discovery](../img/BACnet - discovery.PNG)

3. As devices respond, they will populate the **Devices** list:

![BACnet Devices](../img/BACnet - devices.PNG)

4. Verify:

   - Each expected controller is present  
   - Device IDs and names look correct  
   - Network numbers / MACs are as expected

---

## 🔁 7. Router Table & MSTP Devices (If Applicable)

If your BACnet environment includes routers:

1. Navigate to:

   ```
   BACnet → Configuration → Router Table
   ```

2. Review discovered routers and network paths.

![Router Table](../img/BACnet - Configuration - router table.PNG)

3. For MSTP devices behind routers:

   - Ensure routers are online  
   - Verify remote networks appear in the table  
   - Add manual router entries if required by your topology

---

## 📟 8. Point Discovery for Devices

Once devices are discovered, drill into each device to find its points.

1. From **BACnet → Devices**, select a device.
2. Open the **Point Discovery** / **Objects** view.

![Point Discovery](../img/BACnet - devices - point discovery.PNG)

3. For each point, you can see:

   - Object type and instance
   - Present value
   - Units
   - Reliability
   - Status flags

4. Use filters or search to focus on key points (e.g., temperatures, flows, valve commands).

---

## 🧩 9. Inspect Properties & Live Values

Use the **Property View** for detailed insight and debugging.

![Property View](../img/Property View.PNG)

You can:

- Open a single point’s full property sheet  
- See all relevant BACnet properties  
- Watch live updates as the device changes state  
- Verify units and scaling  

> 💡 This is the best place to confirm a point behaves correctly before mapping it to BasiX.

---

## 🎛️ 10. Flow View & Data Transformation (Optional at this Stage)

If you need to:

- Scale values  
- Compute derived points (ΔT, kW/ton, etc.)  
- Clean up units or reliability

You can add logic in **Flow View**:

![Flow View](../img/Flow View.PNG)

Use the **Toolbox**:

![Toolbox](../img/DataLynX - Toolbox.PNG)

Common tasks:

- Divide by 10 for scaled temperatures  
- Compute ΔT between two sensors  
- Apply default/fallback value if device is offline  
- Normalize enum or binary states

---

## ✅ 11. Integration Checklist

Before moving on to BasiX mapping, verify:

- [ ] BACnet driver is enabled and healthy  
- [ ] Hosted Device has a unique Device ID  
- [ ] BACnet/IP network(s) are configured and enabled  
- [ ] Polling and health monitoring settings are reasonable  
- [ ] All expected devices appear in the Devices list  
- [ ] Router table is correct for routed / MSTP networks  
- [ ] Key points have been discovered and show live values  
- [ ] Units and scaling have been validated (and corrected if needed)  

If all items are checked, your BACnet integration is ready for the next stage.

---

## 🚀 12. Next Step: BasiX Mapping Workflow

You are now ready to:

👉 Proceed to **[BasiX Mapping Workflow](basix-mapping-workflow.md)**
to normalize BACnet points into BasiX device profiles and push them to BDX.

