# ![DataLynX Logo](../assets/datalynx_logo.svg){ width="50" } DataLynX Agent Architecture

This page provides an overview of **DataLynX**, the BuildingLogiX on-premises agent responsible for BACnet communication, data transformation, BasiX modeling, and secure data exchange with BDX.

---

## 🏗️ How DataLynX Works

DataLynX is designed with clear, modular layers:

```
        +-----------------------+
        |          BDX          |
        +-----------+-----------+
                    ↑
               BDX Data Pump
                    ↑
        +-----------+------------+
        |      BasiX Profiles    |
        +-----------+------------+
                    ↑
           Data Processing Layer
                    ↑
            Property Containers
                    ↑
              BACnet Driver
                    ↑
             Driver Framework
                    ↑
             Physical Network
```


## 🧩 Core Components at a Glance

| Component | What It Does |
|-----------|--------------|
| **BACnet Driver** | Discovers devices, reads points, manages polling |
| **Data Processing Layer** | Transforms data, handles units, creates virtual points |
| **BasiX Profiles** | Standardizes devices into consistent schemas for analytics |
| **Data Pump** | Sends processed data to BDX securely |
| **Web Service** | Hosts the UI and API endpoints |
| **Supervisor Interface** | Enables remote management from BDX |

---

## 🌐 BACnet Driver

The BACnet driver handles all communication with your building automation system.

**Key capabilities:**

- BACnet/IP communication
- Device discovery (Who-Is / I-Am)
- Reading object lists and properties
- Configurable polling intervals
- Router table management
- Support for Standard Device, Foreign Device, and BBMD modes
- Device health monitoring and stale-time detection

**Routing:** DataLynX automatically learns BACnet networks from router responses. Manual router table entries are also supported.

**Polling:** The driver polls points at configured intervals and tracks when values become stale or devices become unreachable.

---

## 🔢 Data Processing & Transformation Layer

The data processing layer is where raw BACnet data becomes analytics-ready.

**What it provides:**

- Visual logic blocks (Flow View)
- Arithmetic and logical operations
- Unit conversion and propagation
- Virtual point generation
- Status and reliability tracking
- Watch values for debugging

**Why it matters:** Raw BACnet data is often incomplete, improperly scaled, or missing units. The data processing layer lets you:

- Convert values to standard units
- Compute derived values (ΔT, efficiency, totals)
- Combine multiple points into single outputs
- Apply fallback logic for missing data

All values sent to BDX flow through this layer.

---

## 🧱 Property Containers & Data Flow

DataLynX uses **Property Containers** to track each value through the system:

- Value
- Status
- Units
- Timestamps
- Reliability

---

## 📚 BasiX Ontology & Device Profiles

BasiX provides a **standardized schema** for HVAC and building equipment.

A BasiX Profile:

- Defines the device type (AHU, VAV, Chiller, Pump, etc.)
- Maps points to standard names
- Specifies expected units and ranges
- Enables consistent analytics across different vendors

DataLynX converts raw BACnet points into structured BasiX entities using mappings and transformations.

---

## ☁️ Data Pump (BDX Communication)

The Data Pump sends normalized data to BDX:

- Packages BasiX data for transmission
- Handles retries and communication errors
- Receives remote commands from BDX
- Reports agent and device health

**Communication modes:**

- Agent-initiated
- Server-initiated
- Bi-directional

---

## 🖥️ Web Service

The Web Service provides:

- Local UI access
- REST API endpoints
- Authentication
- Live point values
- Configuration management

This service must be running to access the DataLynX UI.

---

## 🔒 Supervisor Interface

Used by BDX for remote management:

- Agent health checks
- Driver restarts
- Configuration updates
- Log collection
- Snapshot management

This interface is accessed through secure channels by BDX tooling.

---

## 📝 Summary

This architecture ensures:

- **High reliability** — Modular components isolate failures
- **Clean separation** — Device data, logic, and BDX integration are independent layers
- **Flexibility** — Transform any BACnet system into standardized BasiX data
- **Visibility** — Debug at any layer using Watch values and logs

---

## ➡️ Next Step

Proceed to **[Data Transformation & Logic Flow](data-transformation.md)** to learn about the logic processing capabilities in detail.
