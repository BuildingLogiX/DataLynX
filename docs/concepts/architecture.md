# 🏛️ DataLynX Agent Architecture

This page provides a high‑level overview of the internal architecture of **DataLynX**, the BuildingLogiX on‑premises agent responsible for BACnet communication, local computation, BasiX modeling, and secure data exchange with BDX.

Understanding the architecture will help you configure networks, troubleshoot issues, and design effective BasiX mappings.

---

## 🧩 1. Architectural Overview

DataLynX is composed of several core subsystems that work together:

- **Data Processing Layer**
  Performs data transformation, logic, units handling, and virtual point generation.

- **Driver Framework**
  Manages all protocol-level communication. Currently includes the BACnet driver, with future expansion possible.

- **BACnet Driver**  
  Handles BACnet/IP communication, discovery, routing, polling, and point reads.

- **HTTP/Web Service**  
  Hosts the UI, API endpoints, and configuration interface.

- **Supervisor Interface**  
  Allows remote control and monitoring via BDX Supervisor tools.

- **BasiX Profiles & Ontology**
  Normalizes diverse device types into a consistent, analytics-ready structure.

- **Data Pump**  
  Sends transformed/normalized data to BDX reliably and securely.

These components communicate through internal message buses and property containers to provide a fast, scalable integration layer.

---

## 🔌 2. Driver Framework

The **Driver Framework** is responsible for modular protocol support. Each driver:

- Implements discovery  
- Reads/writes point values  
- Manages health status
- Registers networks and devices
- Produces normalized values for the data processing layer

### Benefits of this design:
- Drivers remain isolated from each other
- Data processing behavior is consistent regardless of protocol
- New protocols can be added without UI redesign
- Clear layering between physical devices and logical models  

---

## 🌐 3. BACnet Driver

The BACnet driver is the cornerstone of DataLynX integrations.

### Key responsibilities:

- BACnet/IP communication  
- Handling **Who‑Is / I‑Am** discovery  
- Reading object lists and properties  
- Managing polling intervals  
- Maintaining router tables  
- Supporting Standard Device, Foreign Device, and BBMD modes  
- Device health monitoring and stale‑time logic
- Exposure of BACnet objects to the data processing layer  

### Routing

DataLynX automatically learns BACnet networks via responses from routers and can maintain a router table. Manual overrides are also supported.

### Polling

The driver uses a single default polling policy (unless overridden in future releases).  
Polling determines how often DataLynX reads:

- Analog Inputs/Outputs/Values  
- Binary Inputs/Outputs/Values  
- Multi-state objects  
- Device properties  

Polling is integrated with stale-time handling and device reliability reporting.

---

## 🔢 4. Data Processing & Transformation Layer

The **data processing layer** is the computation heart of DataLynX.

It provides:

- Wiresheet-style logic blocks (accessed via Flow View)
- Links between block inputs and outputs
- Arithmetic and logical operations
- Unit propagation and conversion
- Status and reliability tracking
- Ability to generate virtual points
- Snapshots and online backups
- Debugging through "watch" values

### Why it matters

The raw BACnet data coming from devices is often incomplete, noisy, or improperly scaled.
The data processing layer lets you:

- Convert temperatures, flows, and kW into standard units
- Compute derived values like ΔT, % load, energy usage, or airflow
- Normalize naming and behavior
- Combine multiple raw points into single BasiX points
- Apply safety logic or fallback rules

All values pushed to BDX go through this layer.

---

## 🧱 5. Property Containers & Data Flow

Internally, DataLynX uses **Property Containers** to track:

- Value  
- Status  
- Units  
- Source timestamps  
- Reliability  
- Change events  

Data flows through the system in this sequence:

```
BACnet Driver → Property Container → Data Processing Layer → BasiX Profile → Data Pump → BDX
```

Each layer annotates and transforms the data appropriately.

---

## 📚 6. BasiX Ontology & Device Profiles

BasiX provides a **standardized schema** to represent HVAC and building systems.

A BasiX Profile:

- Defines the type of device (AHU, VAV, Chiller, Pump, etc.)
- Maps required and optional points  
- Defines units and expected ranges  
- Allows analytics to treat similar systems consistently

DataLynX converts raw BACnet points into structured BasiX entities using mappings and data transformations.

---

## ☁️ 7. Data Pump (BDX Communication Layer)

The Data Pump is responsible for:

- Packaging normalized BasiX data  
- Sending it securely to BDX  
- Handling retries and communication errors  
- Receiving remote commands from BDX  
- Reporting device and Agent health  

Communication supports:

- Agent-initiated mode  
- Server-initiated mode  
- Bi-directional mode  

(Depending on BDX configuration.)

---

## 🖥️ 8. HTTP/Web Service

The Web Service provides:

- Local UI  
- JSON REST API endpoints  
- Authentication (username/password, token generation)  
- Live point values and watch updates  
- Config file persistence  

This service must be running to access the UI.

---

## 🔒 9. Supervisor Interface

Used by BDX-side BuildingLogiX tooling for:

- Remote agent health checks  
- Restarting drivers  
- Updating config files  
- Collecting logs  
- Managing snapshots  

This interface is exposed through secure channels and is generally not accessed manually.

---

## 📝 10. Summary

DataLynX is designed with clear, modular layers:

```
           +-----------------------+
           |          BDX          |
           +-----------+-----------+
                       ↑
                 Data Pump
                       ↑
          +------------+------------+
          |      BasiX Profiles     |
          +------------+------------+
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

This architecture ensures:

- High reliability
- Modular expansion
- Clean separation between device data, logic, and BDX integration
- Rapid debugging and visibility  

---

## ➡️ Next Step

Proceed to **Concepts → Data Transformation & Logic Flow** to understand the logic processing portion in more detail.

