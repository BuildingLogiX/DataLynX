# 📡 BACnet Driver Reference

The **BACnet Driver** in DataLynX provides full BACnet/IP client functionality, enabling device discovery, polling, routing, health monitoring, and property access.  
This reference outlines all BACnet driver configuration fields, behavior, limits, and operational details.

---

## 🧭 1. Overview

The BACnet driver is responsible for:

- BACnet/IP communication  
- Sending **Who-Is** and receiving **I-Am**  
- Reading object lists & properties  
- Maintaining polling schedules  
- Managing router tables  
- Handling Foreign Device registration  
- Monitoring device health  
- Providing raw data to the data transformation layer  

---

## 🌐 2. BACnet/IP Network Configuration Fields

Located at:

```
BACnet → Configuration → BACnet IP Network
```

![BACnet IP Network](../img/BACnet - Configuration - BACnet IP Network.PNG)

### **Network Settings**

| Field | Description |
|-------|-------------|
| **UDP Port** | Default: `47808` (0xBAC0). Must match field devices. |
| **Interface / IP Address** | Network interface DataLynX binds to. |
| **Network Number** | BACnet network number. Must be unique. |
| **Device Type** | Standard, Foreign Device, or BBMD participant. |
| **Foreign Device Options** | BBMD IP and registration TTL. |
| **Enabled** | Enables the network segment. |

---

## 🔄 3. Device Discovery

### **Who-Is / I-Am**

- Broadcasting **Who-Is** discovers all reachable devices.  
- **Ranged Who-Is** allows targeted discovery for large systems.  

![Discovery](../img/BACnet - discovery.PNG)

### Discovery populates:

- Device ID  
- Device name  
- Vendor  
- Firmware data  
- MAC address  
- Network number  

---

## 📋 4. Device List & Properties

All discovered devices appear in:

```
BACnet → Devices
```

![Devices](../img/BACnet - devices.PNG)

Selecting a device shows:

![Point Discovery](../img/BACnet - devices - point discovery.PNG)

You can view:

- Full object list
- Present values
- Units
- Reliability
- Status flags  

---

## 🔁 5. Polling Engine

Polling determines how often DataLynX reads analog, binary, and multistate values.

Located at:

```
BACnet → Configuration → Poll Service
```

![Poll Service](../img/BACnet - Configuration - BACnet IP Network - poll service.PNG)

### Polling Parameters

| Setting | Meaning |
|--------|---------|
| **Poll Interval** | How often values are read. |
| **Stale Time** | If exceeded, value becomes unreliable/offline. |
| **Timeout** | Max wait time for device response. |
| **Retries** | Attempts before marking offline. |

---

## 🚦 6. Health Monitoring

Located at:

```
BACnet → Configuration → Health Monitoring
```

![Health Monitoring](../img/BACnet - Configuration - health_monitoring.PNG)

Monitors:

- Device offline conditions  
- Poll failures  
- Router failures  
- High latency responses  

### Offline thresholds can be configured globally.

---

## 🔀 7. Router Table

DataLynX automatically learns routers and remote networks.

Located at:

```
BACnet → Configuration → Router Table
```

![Router Table](../img/BACnet - Configuration - router table.PNG)

Supports:

- Automatic router discovery  
- Manual entries  
- MSTP remote station definitions  
- Multiple network tree architectures  

---

## 🧩 8. Object Types Supported

The BACnet driver supports all major object types:

- Analog Input / Output / Value  
- Binary Input / Output / Value  
- Multistate Input / Output / Value  
- Device  
- Calendar  
- Schedule  
- Loops (read-only)  
- Trendlogs (future enhancement)  

Each object type exposes:

- Present value  
- Units  
- Reliability  
- Status flags  
---

## ✏️ 9. Read Behavior

DataLynX is a **read-only BACnet client**. It reads point values from BACnet devices but does not write to them.

### **Reads**

- Polling retrieves values at configured intervals.
- Ad-hoc reads occur when property views are opened.

---

## 🛡️ 10. Reliability & Status Handling

The driver tags each property with:

- **OK**  
- **Fault**  
- **Downstream Fault**  
- **Unreliable**  
- **Out of Range**  

Values that exceed stale time are marked **offline**.

These flags propagate through block links unless overridden.

---

## 🔐 11. Foreign Device Registration (FDR)

If the Agent is not on the same subnet as field devices, FDR allows connection to a BBMD.

Configuration:

- **BBMD IP**  
- **Registration TTL**  
- **Renew interval**  

Used primarily for:

- WAN-connected sites  
- Cellular gateways  
- Multi-building firewalled networks  

---

## 🚫 12. Common BACnet Errors

| Error | Cause | Fix |
|-------|-------|-----|
| **No Response** | Device offline or wrong port | Verify device, port, firewall |
| **Invalid APDU** | Vendor mismatch or segmentation | Reduce APDU size |
| **Router Unreachable** | Network segmentation | Add router entry or fix routing |
| **Units Missing** | Vendor does not provide units | Add unit conversion block |
| **Timeout** | Network latency | Increase timeout or polling interval |

---

## 📝 13. Summary

The BACnet Driver provides:

- Full BACnet/IP communication  
- Robust discovery  
- Polling + stale-time logic  
- Routing and BBMD support  
- High-visibility diagnostics  
- Clean normalized data for the engine  

DataLynX’s BACnet stack is optimized for fast integration and reliable operation.

---

## ➡️ Next Step

Proceed to:

👉 **FAQ** for common questions and troubleshooting tips.

