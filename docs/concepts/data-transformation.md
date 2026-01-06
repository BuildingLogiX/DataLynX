# Data Transformation & Logic Flow

DataLynX provides powerful **data transformation and logic capabilities** that allow you to clean, scale, convert, and manipulate building automation data before sending it to BDX.

This layer sits between your BACnet devices and the final BasiX profiles, ensuring that raw field data becomes accurate, normalized, and analytics-ready information.

You'll interact with these capabilities primarily through the **Flow View** in the DataLynX UI—a visual wiresheet interface where you can build logic using drag-and-drop blocks.

---

## 1. What Data Transformation Does

DataLynX's data transformation layer:

- Receives raw values from BACnet devices
- Wraps values in standardized objects including:
  - Value
  - Units
  - Status
  - Reliability
  - Timestamp

- Propagates data through links between logic blocks
- Performs math, conversions, logical operations, and conditionals
- Produces clean, normalized values for BasiX profiles
- Supports live monitoring through Watch features
- Supports snapshots and online backups

Everything that goes to BDX flows through this transformation layer.

---

## 2. Core Concepts

### **Logic Blocks**
Logic blocks are the functional units you'll work with in the Flow View.
Each block has:

- **Inputs** (properties)
- **Outputs** (properties)
- Optional **configuration parameters**

Common block categories include:

- Math (Add, Subtract, Multiply, Divide)
- Logic (And, Or, Not)
- Comparisons (GreaterThan, Equal)
- Unit conversions
- Status propagation
- Value merging or fallback blocks
- Custom BasiX mapping blocks

---

### **Properties**
Properties represent the data at inputs and outputs of blocks. They contain:

- A value
- A unit (optional)
- A status (OK, Fault, Downstream Fault, etc.)
- A reliability indicator
- A timestamp
- Optional error or fallback information

Properties are the fundamental data elements that flow through your logic.

---

### **Links**
Links connect block outputs to block inputs in the Flow View.

A link handles:

- Value propagation
- Unit propagation
- Status propagation
- Change detection
- Fault-forwarding

There is a global configuration option for **"propagate units"**:

- When enabled, mismatched units between blocks may trigger faults
- When disabled, blocks may accept values without unit constraints

---

## 3. Data Flow Through DataLynX

Below is the typical flow of a single value from a BACnet device:

```
BACnet Driver → Raw Value
Raw Value → Processing Layer
Processing Layer → Logic Block
Logic Block → Transformation
Transformation → Linked Block
...
Final Value → BasiX Profile
BasiX Profile → Data Pump → BDX
```

This pipeline ensures that raw, noisy field data is normalized, scaled, and reliable.

---

## 4. Example: Building a Simple Logic Flow

Consider measuring an AHU Cooling Coil Delta-T:

```
MAT (°F) → Subtract Block ┐
                           ├→ Result = ΔT (°F)
SAT (°F) → Subtract Block ┘
```

Configuration:

- Block: **Subtract**
- Input A: MAT (Mixed Air Temperature)
- Input B: SAT (Supply Air Temperature)
- Output: deltaT

The block automatically:

- Ensures units are compatible
- Tracks status (fault, ok, unreliable)
- Produces an updated output whenever either input changes
- Passes the ΔT result to downstream logic or BasiX mapping

---

## 5. Status & Reliability Handling

Every property maintains:

- **Status**
    - OK
    - Faulted
    - Downstream Fault
    - Configuration Error

- **Reliability**
    - No Fault
    - Questionable
    - Unreliable
    - Out of Range

DataLynX enforces data quality through:

- Block-level rules
- Link behavior
- Input validation
- Unit consistency checking
- Device stale-time handling

This ensures BDX receives quality data.

---

## 6. Flow View & Live Monitoring

The **Flow View** in the UI provides a visual wiresheet interface where you can:

- Drag and drop logic blocks
- Connect blocks with links
- Configure block parameters
- View live data flowing through your logic
- Use the **Watch** feature to monitor any point in real-time

### Using the Watch Feature

The Watch feature lets you:

- Select any property in the logic flow
- View its value, status, and units live
- See change events in real time
- Debug mappings and logic
- Confirm BACnet reads are working

Watches are essential for troubleshooting mappings or device issues.

---

## 7. Snapshots & Backups

DataLynX automatically maintains:

- **Logic Snapshots**
  Save all block states and property values instantly.

- **Online Backups**
  Rotating backups of the entire agent configuration and logic state.

This is critical for restoring operation after failures, upgrades, or corrupted configs.

---

## 8. Configuration Files

DataLynX stores your logic configuration in standard formats:

- **Configuration Files** (`.json`)
  Full block definitions, links, and properties.

Configuration and state files live under:

```
C:\ProgramData\DataLynX\agents\default\
```

---


## ➡️ Next Step

Proceed to **[BasiX Ontology & Profiles](basix-ontology.md)**
to understand how DataLynX models equipment in a standardized form before sending it to BDX.
