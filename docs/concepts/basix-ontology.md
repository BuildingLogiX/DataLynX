# BasiX Ontology & Device Profiles

The **BasiX Ontology** is the standardized data model that DataLynX uses to transform diverse BACnet devices and point structures into a common, analytics-ready format for BDX.  
It ensures that AHUs, VAVs, pumps, chillers, heat pumps, fan coils, and other HVAC systems all follow consistent schemas—regardless of vendor differences.

---

## 1. What Is BasiX?

**BasiX** defines:

- Standard **device types** (e.g., AHU, VAV, Chiller, Pump)
- Standard **point names** (e.g., supplyAirTemp, supplyFanVFDSpeed, coolOutput)
- Units, data types, and expected ranges
- Normalized behavior and meaning of each value
- Metadata needed for analytics and equipment classification

BasiX serves as the "lingua franca" between on-prem building systems and BDX.

---

## 2. Why BasiX Exists

BACnet systems differ dramatically across:

- Manufacturer naming conventions  
- Object types  
- Units  
- Enumerations  
- Scaling  
- Device structures  
- Quality / reliability fields  

BasiX solves this by:

- Normalizing all HVAC equipment into predictable structures  
- Defining consistent point names  
- Ensuring uniform units and interpretation  
- Eliminating vendor-specific quirks  
- Enabling common analytics across campuses and portfolios  

---

## 3. BasiX Device Profiles

A **BasiX Profile** is a template that defines the standard structure of a device.

Each profile includes:

- **Device type** (e.g., AHU, VAV, ExhaustFan, Tower, Boiler)
- **Point schema**
  - Required points
  - Optional points
  - Units
  - Expected ranges
  - Setpoint behavior

- **Tags & metadata**
- **Virtual or computed fields**
- **Normalization rules**

### Example: AHU BasiX Profile

| BasiX Point | Description | Units |
|-------------|-------------|--------|
| supplyAirTemp | Discharge air temperature | °F |
| mixedAirTemp | Mixed air temperature | °F |
| coolOutput | Cooling coil valve position | % |
| supplyFanVFDSpeed | Supply fan speed or VFD output | % |
| ductStaticPressure | Duct static pressure | inWC |

The same logic applies to VAVs, Chillers, Pumps, etc.

---

## 4. Mapping BACnet → BasiX

DataLynX uses two layers to convert field data into BasiX:

1. **Data Transformation**
   - Performs scaling, unit conversion, reliability logic, derived values.
   - Access these features through the Flow View interface.

2. **Mapping Layer**
   - Connects BACnet objects to BasiX point definitions.
   - Supports:
     - **Exact mapping**
     - **Pattern mapping** (`*`, `?`)
     - **Regex mapping**

### Example Mapping

```
BACnet AI "SF-T"  →  BasiX supplyAirTemp
BACnet AV "RM-T"  →  BasiX zoneAirTemp
BACnet AO "CCV"   →  BasiX coolOutput
```

If a device has 100 VAVs named `VAV-1` through `VAV-100`, pattern matching might be used:

```
BACnet path: /VAV-*/AI-RM-T
BasiX point: ZoneAirTemp
```

---

## 5. Multi-Device Scenarios

Some equipment configurations require special handling:

### **1 BACnet Device → Many BasiX**
Example: A plant controller exposes multiple towers and pumps under one device.

### **Many BACnet Devices → One BasiX**
Example: Multiple BACnet controllers represent a single AHU's logic and sensors.

### **Many BACnet Devices → Many BasiX**
Often used in distributed systems with many repeated devices (VAVs, FCUs).

The mapping layer handles all these scenarios.

---

## 6. Validation & Health Checking

The BasiX layer validates:

- **Invalid or missing units** ⚠️
- Inconsistent enumerations

**Critical**: All points mapped to BasiX profiles must have proper units. Missing or incorrect units will prevent device publishing to BDX.

This ensures only high-quality data is sent to BDX.

See **[Units & Formatting Best Practices](units-formatting.md)** for detailed guidance on proper unit handling.

---

## 7. Output to BDX

Once a device is mapped and validated:

- A **fully normalized BasiX object** is created
- It is transmitted to BDX through the Data Pump
- BDX treats all devices of the same type uniformly

This allows:

- Portfolio-wide analytics  
- Fault detection  
- Load modeling  
- Equipment comparisons  
- Energy performance baselines  

---


## ➡️ Next Step

Continue to **[Mapping Modes](mapping-modes.md)** to learn how DataLynX maps BACnet points to BasiX using exact, pattern, and regex approaches.

