# ![DataLynX Logo](../assets/datalynx_logo.svg){ width="50" } Hierarchy & Digital Twin Workspace

The **Hierarchy** node in DataLynX is your digital twin workspace—a flexible area where you can mirror your physical building infrastructure, organize custom logic and calculations, and build BasiX profiles for BDX.

Unlike the physical BACnet device tree (found under Connections → BACnet → devices), the Hierarchy is **your logical workspace** organized however makes sense for your site—whether that's mirroring physical layout, grouping by equipment type, organizing by function, or a combination.

---

## What is the Hierarchy?

The Hierarchy is:

- **A blank canvas** for organizing your site's virtual structure
- **Site and building-specific** - Unique to each physical location
- **Your workspace** for custom logic blocks, calculations, and data transformations
- **Separate from BACnet devices** - Provides logical organization independent of physical network topology
- **The home of Flow View logic** - Where blocks are stored and organized
- **The place where BasiX profiles get mapped** - Virtual devices for BDX are built here

**Key Concept:**
Think of Hierarchy as the flexible workspace where you:

- Build custom calculations and logic (efficiency metrics, aggregations, fault detection)
- Create BasiX device profiles that map to physical equipment
- Organize data processing flows in whatever structure makes sense for your site
- Prepare normalized output for the BDX agent

**The flow:**

- **Physical BACnet devices** (raw data from controllers) → Connections/BACnet/devices
- **Custom logic & BasiX profiles** (processed, organized data) → Hierarchy
- **Normalized BasiX output** (sent to BDX) → Built from Hierarchy blocks

---

## Common Hierarchy Structures

The Hierarchy is flexible—organize it however makes sense for your use case. Here are common patterns:

### **Option 1: Organize by Physical Layout**

Mirror your campus and building structure (good for facility management and location-based analytics):

```
Hierarchy
├── Campus_Main
│   ├── Building_A
│   │   ├── Floor_1
│   │   │   ├── AHU-1A
│   │   │   ├── VAV-101 through VAV-115
│   │   │   └── Lighting_Zone_1A
│   │   ├── Floor_2
│   │   └── Floor_3
│   └── Building_B
│       ├── Mechanical_Room
│       │   ├── Chiller_1
│       │   ├── Chiller_2
│       │   └── Primary_Pumps
│       └── Occupied_Spaces
├── Campus_North
└── Remote_Sites
```

**When to use:**

- Multi-building campuses
- Clear physical separation between buildings or zones
- Facility management teams organized by building
- When you need zone-level aggregations

---

### **Option 2: Organize by Equipment Type**

Group by system or equipment category (good for equipment-focused analytics and standardized logic):

```
Hierarchy
├── AHUs
│   ├── AHU-1
│   ├── AHU-2
│   └── AHU-3
├── VAVs
│   ├── East_Wing
│   │   ├── VAV-101
│   │   └── VAV-102
│   └── West_Wing
├── Chillers
├── Boilers
├── Cooling_Towers
├── Pumps
│   ├── Chilled_Water_Pumps
│   └── Hot_Water_Pumps
└── Lighting_Systems
```

**When to use:**

- Single building or small campus
- Emphasis on equipment-type analytics
- When creating standardized logic that applies to all equipment of the same type
- Energy management focus

---

### **Option 3: By Function or Purpose**

Group by calculation type, function, or analytical purpose:

```
Hierarchy
├── BasiX_Profiles
│   ├── AHU_Mappings
│   ├── VAV_Mappings
│   └── Chiller_Mappings
├── Custom_Calculations
│   ├── Energy_Metrics
│   ├── Efficiency_Calcs
│   └── Fault_Detection
├── Aggregations
│   ├── Building_Totals
│   └── Floor_Averages
└── Virtual_Meters
    ├── Electric
    └── Thermal
```

**When to use:**

- Focus on BasiX profile creation for BDX
- Separating custom calculations from device mappings
- Organizing by analytical function rather than physical location
- Building reusable logic libraries

---

### **Option 4: Hybrid Approach**

Combine location, equipment type, and function:

```
Hierarchy
├── Main_Campus
│   ├── Central_Plant
│   │   ├── Chillers
│   │   ├── Boilers
│   │   └── Pumps
│   ├── Building_A
│   │   ├── HVAC
│   │   │   ├── AHUs
│   │   │   └── VAVs
│   │   └── Electrical
│   └── Building_B
├── Utility_Metering
│   ├── Electric_Meters
│   ├── Gas_Meters
│   └── Water_Meters
└── Weather_Station
```

**When to use:**

- Large, complex sites
- Multiple systems spanning multiple buildings
- Need for both location-based and system-based views
- Portfolio-level analytics

---

## What Lives in the Hierarchy?

### **1. Folders**

Organizational containers for grouping logic blocks.

**Example:**
```
Hierarchy → AHUs → (folder)
```

**How to create:**

- Right-click on Hierarchy or a subfolder → New Folder
- Or drag a **Folder** block from the Toolbox

---

### **2. Logic Blocks**

Custom data transformation blocks that:

- Read values from BACnet points
- Perform calculations (math, conversions, logic)
- Create virtual points
- Output processed values for BasiX mapping

**Example:**
```
Hierarchy → AHUs → AHU-1 → (contains logic blocks)
```

Inside `AHU-1`, you might have blocks like:

- BACnet point references (supply air temp, fan status, etc.)
- Math blocks (computing airflow, ΔT)
- Conversion blocks (scaling, unit changes)
- Comparison blocks (fault detection)

![Flow View](../img/Flow View.PNG)

---

### **3. Virtual Points**

Computed values that don't exist as physical BACnet points.

**Examples:**

- **Total Building Electric Demand** - Sum of multiple meter readings
- **AHU Efficiency** - Calculated from airflow, temperatures, and power
- **System Status** - Derived from multiple fault conditions
- **Occupancy-Adjusted Setpoint** - Computed from schedule and space temp

These virtual points are created using blocks in the Hierarchy and can be:

- Mapped to BasiX profiles
- Sent to BDX
- Used as inputs to other logic blocks

---

### **4. Equipment-Specific Logic Flows**

Each piece of equipment in your digital twin can have its own logic flow workspace.

**Example for a VAV:**
```
Hierarchy → Building_A → Floor_2 → VAV-205
```

Inside `VAV-205`, you might build logic to:

- Read zone temp from BACnet
- Read damper position from BACnet
- Calculate airflow (if not directly available)
- Compute heating/cooling load
- Detect faults (stuck damper, sensor failure)
- Output normalized points for BasiX

---

## Building Your Digital Twin: Step-by-Step

### **Step 1: Plan Your Structure**

Before creating folders, decide:

- How is your site organized physically? (campus, buildings, floors)
- What equipment types do you have? (AHUs, VAVs, chillers, etc.)
- How will you group similar equipment?
- What makes sense for your analytics and reporting needs?

---

### **Step 2: Create the Top-Level Structure**

1. Navigate to **Hierarchy** in the eXplorer
2. Right-click → **New Folder**
3. Name it (e.g., `Campus_Main` or `Building_A`)
4. Repeat for other top-level folders

---

### **Step 3: Build Out the Tree**

Create subfolders for:

- Buildings (if you have multiple)
- Equipment categories (AHUs, VAVs, Chillers, etc.)
- Zones or floors (if organizing by location)

**Example:**
```
Hierarchy
├── Building_A
│   ├── AHUs  ← Create this folder
│   ├── VAVs  ← Create this folder
│   └── Chillers  ← Create this folder
```

---

### **Step 4: Create Equipment Nodes**

For each piece of equipment:

1. Navigate to the appropriate folder (e.g., `Hierarchy → Building_A → AHUs`)
2. Right-click → **New Folder** (or drag Folder from Toolbox)
3. Name it after the equipment (e.g., `AHU-1`)
4. Open the folder in **Flow View**

---

### **Step 5: Build Logic Inside Equipment Nodes**

With the equipment folder open in Flow View:

1. **Drag BACnet point blocks** from Toolbox → BACnet
   - Reference physical points from Connections/BACnet/devices

2. **Add transformation blocks** from Toolbox categories:
   - Math (Add, Subtract, Multiply, Divide)
   - Logic (AND, OR, NOT, Comparisons)
   - Conversions (unit changes, scaling)
   - Status (fault detection, quality checks)

3. **Connect blocks with links**
   - Drag from output to input
   - Configure link properties (unit propagation, etc.)

4. **Create virtual output points**
   - These become available for BasiX mapping
   - Can be used in other logic blocks

![Flow View Example](../img/Flow View.PNG)

---

### **Step 6: Organize and Document**

Best practices:

- **Use clear naming conventions** - `AHU-1_supplyAirTemp` instead of `SAT1`
- **Group related blocks** - Keep blocks for the same equipment together
- **Use folders to separate systems** - Don't mix chillers and AHUs in the same folder
- **Add comments** - Use description fields to document complex logic

---

## Hierarchy vs Connections: Key Differences

| Aspect | Connections/BACnet/devices | Hierarchy |
|--------|---------------------------|-----------|
| **Purpose** | Physical BACnet devices as discovered on the network | Logical organization and custom logic |
| **Organization** | Organized by BACnet network topology | Organized by your site structure |
| **Contents** | Raw BACnet points (AI, AO, AV, BI, BO, BV, etc.) | Custom logic blocks and virtual points |
| **Modification** | Read-only (reflects actual BACnet devices) | Fully editable (you create the structure) |
| **Where it comes from** | Discovery process finds devices automatically | You manually create folders and logic |
| **Use case** | Reading raw BACnet data | Processing, transforming, and organizing data |
| **Flow View** | Shows device points and properties | Shows custom logic flows you build |

**Important:**
You'll often **reference** points from Connections/BACnet in your Hierarchy logic blocks, but they remain separate trees.

---

## Using Flow View in the Hierarchy

When you click on any folder in the Hierarchy, it opens in **Flow View** - the visual workspace where you build logic.

### **Flow View Layout**

```
┌─────────────────────────────────────────────────────────────┐
│  Breadcrumb: /Hierarchy/Building_A/AHUs/AHU-1              │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────┐  ┌──────────────────────┐ │
│  │                              │  │   Edit Block         │ │
│  │    Flow View Canvas          │  │   (Properties)       │ │
│  │                              │  │                      │ │
│  │  [Blocks and Links]          │  │   - Block Name       │ │
│  │                              │  │   - Settings         │ │
│  │                              │  │   - Input/Output     │ │
│  │                              │  └──────────────────────┘ │
│  │                              │  ┌──────────────────────┐ │
│  │                              │  │   Toolbox            │ │
│  │                              │  │                      │ │
│  └──────────────────────────────┘  │  - Math              │ │
│                                     │  - Logic             │ │
│                                     │  - BACnet            │ │
│                                     │  - Conversions       │ │
│                                     └──────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

---

## Common Hierarchy Use Cases

### **Use Case 1: Building-Level Energy Aggregation**

```
Hierarchy → Building_A → Energy_Metering
```

**Logic blocks:**

- Read electric meters for all floors
- Sum total building demand
- Compute peak demand
- Calculate energy per square foot
- Output virtual "Building_Total_kW" point

---

### **Use Case 2: AHU Efficiency Calculation**

```
Hierarchy → HVAC_Equipment → AHUs → AHU-1
```

**Logic blocks:**

- Read supply air temp, return air temp, outdoor air temp from BACnet
- Read fan power (kW) from BACnet
- Calculate airflow (cfm)
- Compute ΔT (supply - return)
- Compute efficiency (tons cooling per kW)
- Output virtual "AHU1_Efficiency" point

---

### **Use Case 3: Chilled Water Plant Optimization**

```
Hierarchy → Central_Plant → Chilled_Water_System
```

**Logic blocks:**

- Read chiller kW from all chillers
- Read supply/return CHW temps
- Compute plant ΔT
- Compute total tons
- Calculate plant kW/ton
- Detect low ΔT conditions
- Output virtual "Plant_Efficiency" and "Low_Delta_T_Alarm" points

---

### **Use Case 4: Multi-Zone Temperature Averaging**

```
Hierarchy → Building_A → Floor_2 → Zone_Averages
```

**Logic blocks:**

- Read zone temps from 15 VAVs
- Calculate average zone temp
- Calculate min/max zone temps
- Compute zone temp variance
- Output virtual "Floor2_AvgTemp" point for display

---

## Common Mistakes to Avoid

### **1. Mixing Physical and Logical**

❌ **Don't do this:**
```
Hierarchy
├── Connections (duplicate structure)
└── BACnet (duplicate structure)
```

✅ **Do this:**
```
Hierarchy
├── Building_A (logical organization)
└── Equipment (logical grouping)
```

**Reason:** Connections already shows the physical structure. Hierarchy is for logical organization.

---

### **2. Creating Overly Deep Nesting**

❌ **Don't do this:**
```
Hierarchy → Campus → Region → Building → Wing → Floor → Zone → Equipment_Type → Equipment_01
```

✅ **Do this:**
```
Hierarchy → Building_A → Floor_2 → AHU-2A
```

**Reason:** Deep nesting makes navigation difficult. Keep it practical.

---

### **3. Forgetting to Use Folders**

❌ **Don't do this:**
```
Hierarchy
├── AHU-1 (block at root level)
├── AHU-2 (block at root level)
├── VAV-101 (block at root level)
└── 200 other blocks...
```

✅ **Do this:**
```
Hierarchy
├── AHUs (folder)
│   ├── AHU-1
│   └── AHU-2
└── VAVs (folder)
    └── VAV-101
```

**Reason:** Folders keep things organized and manageable.

---

### **4. Not Planning Ahead**

❌ **Don't start building logic without a plan**

✅ **Sketch out your structure first:**

- What buildings do you have?
- What equipment types?
- How will you group similar equipment?
- What makes sense for your analytics?

**Reason:** Reorganizing later is time-consuming.

---

## Best Practices Summary

1. **Plan before you build** - Sketch your structure on paper or whiteboard first
2. **Use clear naming** - `Building_A_AHU_1` is better than `BA-A1`
3. **Mirror physical layout** - Or mirror equipment types, but be consistent
4. **Keep folders shallow** - 2-4 levels deep is usually sufficient
5. **Group similar equipment** - All AHUs together, all VAVs together
6. **Use virtual points** - Create computed values for analytics
7. **Reference, don't duplicate** - Reference BACnet points from Connections, don't recreate them
8. **Document your logic** - Use block descriptions to explain complex flows
9. **Test as you build** - Verify logic works before building out 100 copies
10. **Think about BasiX** - Organize in a way that makes BasiX mapping straightforward

---

## Hierarchy Workflow Summary

```
1. Plan Structure
   ↓
2. Create Folders (Buildings, Equipment Types, Zones)
   ↓
3. Create Equipment Nodes (Individual AHUs, VAVs, etc.)
   ↓
4. Open Equipment Node in Flow View
   ↓
5. Drag BACnet Blocks (Reference physical points)
   ↓
6. Add Logic Blocks (Math, conversions, fault detection)
   ↓
7. Connect Blocks with Links
   ↓
8. Create Virtual Output Points
   ↓
9. Map Virtual Points to BasiX
   ↓
10. Publish to BDX
```

---

## ➡️ Next Steps

Now that you understand the Hierarchy and digital twin concept:

- **Learn about blocks** - See [Toolbox Reference](../reference/toolbox-blocks.md)
- **Build logic flows** - See [Data Transformation & Logic Flow](../concepts/data-transformation.md)
- **Understand units** - See [Units & Formatting Best Practices](../concepts/units-formatting.md)
- **Map to BasiX** - See [BasiX Mapping Workflow](basix-mapping-workflow.md)
- **Explore the full UI** - See [eXplorer Structure](explorer-structure.md)
