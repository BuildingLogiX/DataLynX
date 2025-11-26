# 🧰 Toolbox Block Reference

The **Toolbox** is the block library available in DataLynX Flow View. It contains all the building blocks you can use to create data transformation logic, perform calculations, connect to BACnet points, and build virtual points for BasiX mapping.

The Toolbox appears on the right side of the screen when you're working in Flow View.

![Toolbox](../img/DataLynX - Toolbox.PNG)

---

## 📚 Toolbox Categories

Blocks are organized into the following categories:

```
Toolbox
├── Aggregate Functions
├── BACnet
├── BasiX Profiles
├── BDX
├── Comparisons
├── Control Points
├── Folder
├── Logic
├── Math
├── Priority Arrays
├── Status
└── Switches and Selects
```

---

## 🔢 Math

Mathematical operations for calculations and transformations.

### **Common Math Blocks:**

| Block | Description | Inputs | Output | Use Case |
|-------|-------------|--------|--------|----------|
| **Add** | Sum two values | in_a, in_b | out | Total energy consumption, combined airflow |
| **Subtract** | Difference between values | in_a, in_b | out | Delta temperature (ΔT), pressure drop |
| **Multiply** | Product of two values | in_a, in_b | out | Power calculations, scaling factors |
| **Divide** | Quotient of two values | in_a, in_b | out | Efficiency (kW/ton), per-square-foot metrics |
| **Mean** | Average of multiple inputs | in_0, in_1, ... in_N | out | Zone temperature averaging, load balancing |
| **Max** | Maximum of multiple inputs | in_0, in_1, ... in_N | out | Peak demand, highest zone temp |
| **Min** | Minimum of multiple inputs | in_0, in_1, ... in_N | out | Lowest zone temp, minimum setpoint |
| **Abs** | Absolute value | in | out | Removing negative signs, distance calculations |
| **Median** | Median of multiple inputs | in_0, in_1, ... in_N | out | Statistical analysis, outlier removal |

### **Example: Computing ΔT**

```
Supply Air Temp (72°F) ──┐
                         │
                      [Subtract] ──> ΔT (12°F)
                         │
Return Air Temp (84°F) ──┘
```

### **Example: Total Building Load**

```
AHU-1 kW ──┐
AHU-2 kW ──┤
AHU-3 kW ──┼── [Add] ──> Total HVAC Load (kW)
Chiller kW ─┤
Boiler kW ──┘
```

---

## 🧠 Logic

Boolean logic and decision-making blocks.

### **Common Logic Blocks:**

| Block | Description | Inputs | Output | Use Case |
|-------|-------------|--------|--------|----------|
| **AND** | All inputs must be true | in_a, in_b | out | Multiple conditions (fan on AND valve open) |
| **OR** | At least one input must be true | in_a, in_b | out | Alarm conditions, any zone over setpoint |
| **NOT** | Invert boolean value | in | out | Opposite of a condition (NOT occupied) |
| **XOR** | Exclusive OR | in_a, in_b | out | Either/or logic (heating XOR cooling) |

### **Example: Fault Detection**

```
Fan Status = OFF ────┐
                     │
                  [AND] ──> Fault Alarm
                     │
Supply Temp > 100°F ─┘
```

---

## ⚖️ Comparisons

Compare values and generate boolean outputs.

### **Common Comparison Blocks:**

| Block | Description | Inputs | Output | Use Case |
|-------|-------------|--------|--------|----------|
| **Greater Than (>)** | Check if A > B | in_a, in_b | out (bool) | Temp above setpoint, power exceeds threshold |
| **Less Than (<)** | Check if A < B | in_a, in_b | out (bool) | Temp below setpoint, low pressure alarm |
| **Equal (==)** | Check if A == B | in_a, in_b | out (bool) | Mode matching, status verification |
| **Greater or Equal (>=)** | Check if A >= B | in_a, in_b | out (bool) | At or above threshold |
| **Less or Equal (<=)** | Check if A <= B | in_a, in_b | out (bool) | At or below threshold |
| **Not Equal (!=)** | Check if A != B | in_a, in_b | out (bool) | Detecting changes |

### **Example: High Temperature Alarm**

```
Zone Temp ──┐
            │
         [>] ──> High Temp Alarm (true/false)
            │
Setpoint ───┘
```

---

## 🌐 BACnet

Blocks for reading and writing BACnet points.

### **Common BACnet Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **BACnet Point Reference** | Read a BACnet object's present value | Reading AI, AV, BI, BV values from devices |
| **BACnet Write** | Write to a BACnet writable property | Commanding AO, BO, or writable AV/BV points |
| **BACnet Priority** | Write with priority level (1-16) | Override control with specific priority |

### **How to Use:**

1. Drag **BACnet Point Reference** from Toolbox
2. Configure the block to point to a BACnet object:
   - Navigate to Connections → BACnet → devices → [Device] → [Point]
3. The block outputs the present value, units, status, and reliability
4. Link to other blocks for processing

### **Example: Reading Supply Air Temp**

```
[BACnet: /Connections/BACnet/devices/AHU-1/AI-SAT]
   │
   ├─> out (value: 55.2°F)
   ├─> units (°F)
   ├─> status (OK)
   └─> reliability (Reliable)
```

---

## 🧬 BasiX Profiles

Blocks representing BasiX device profile fields.

### **Common BasiX Profile Blocks:**

These blocks represent standardized points in BasiX profiles:
- **SupplyAirTemp** - AHU supply/discharge air temperature
- **ZoneAirTemp** - VAV zone temperature
- **DamperPosition** - VAV damper position
- **CoolingValveCommand** - Cooling coil valve position
- **FanSpeed** - Fan or VFD speed
- **Airflow** - Measured airflow (cfm)
- **Power** - Equipment power consumption (kW)
- **Status** - Equipment on/off or run status

### **How to Use:**

1. Drag a BasiX Profile block from the Toolbox
2. Link processed values to the block inputs
3. These blocks become the output points for BasiX mapping
4. They are published to BDX

### **Example: Mapping AHU Supply Temp**

```
[BACnet: AHU-1 Supply Temp] ──> [Divide by 10] ──> [BasiX: SupplyAirTemp]
                                                            │
                                                      (Sent to BDX)
```

---

## 🔄 Switches and Selects

Conditional routing and selection blocks.

### **Common Switch/Select Blocks:**

| Block | Description | Inputs | Output | Use Case |
|-------|-------------|--------|--------|----------|
| **Switch** | Route input based on condition | in, condition | out | If occupied, use schedule A; else use schedule B |
| **Select** | Choose between multiple inputs | in_0, in_1, selector | out | Select between sensors, choose control mode |
| **Multiplexer** | N-to-1 selection | Multiple inputs, index | out | Select from array of values |

### **Example: Occupancy-Based Setpoint**

```
Occupied Setpoint (72°F) ──┐
                           │
Occupancy Status ────> [Switch] ──> Active Setpoint
                           │
Unoccupied Setpoint (65°F) ┘
```

---

## 📊 Aggregate Functions

Advanced aggregation and statistical operations.

### **Common Aggregate Blocks:**

| Block | Description | Inputs | Output | Use Case |
|-------|-------------|--------|--------|----------|
| **Sum** | Sum all inputs | Multiple inputs | out | Total energy, combined load |
| **Average** | Mean of all inputs | Multiple inputs | out | Average zone temp across floor |
| **Weighted Average** | Weighted mean | Values, weights | out | Load-weighted efficiency |
| **Standard Deviation** | Statistical deviation | Multiple inputs | out | Variability analysis |
| **Count** | Count valid inputs | Multiple inputs | out | Number of active devices |

### **Example: Average Zone Temperature**

```
VAV-101 Zone Temp ──┐
VAV-102 Zone Temp ──┤
VAV-103 Zone Temp ──┼── [Average] ──> Floor Average Temp
VAV-104 Zone Temp ──┤
VAV-105 Zone Temp ──┘
```

---

## 🎛️ Control Points

Virtual setpoints and control variables.

### **Common Control Point Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Setpoint** | Configurable constant value | Fixed setpoints, thresholds |
| **Schedule** | Time-based value changes | Occupancy schedules, setback schedules |
| **Override** | Manual override capability | Testing, commissioning |

### **Example: Setpoint Block**

```
[Setpoint: 72.0°F] ──> Used in comparison or control logic
```

---

## 📡 Status

Status monitoring and fault detection blocks.

### **Common Status Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Status Monitor** | Check reliability and status flags | Detect offline points, faulted sensors |
| **Fault Detector** | Generate fault alarms based on conditions | High temp, low pressure, stuck valve |
| **Quality Check** | Validate data quality | Out-of-range detection, stale data |

### **Example: Sensor Fault Detection**

```
[BACnet: Temp Sensor] ──> [Status Monitor] ──> Sensor Fault Alarm
                              │
                          (Check reliability != Reliable)
```

---

## 🔢 Priority Arrays

BACnet priority array management.

### **Priority Array Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Priority Write** | Write to specific priority level (1-16) | Override control logic |
| **Priority Release** | Release a priority level | Return control to lower priority |
| **Priority Status** | Read current priority array | View active overrides |

### **Example: Manual Override**

```
Manual Command ──> [Priority Write: Level 8] ──> BACnet AO
```

**Important:** Priority 1 is highest, 16 is lowest. Manual operator overrides typically use priority 8.

---

## 📦 BDX

Blocks for sending data to BDX.

### **BDX Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **BDX Output** | Send value to BDX | Publishing custom metrics |
| **BDX Tag** | Add metadata tags | Equipment classification |

---

## 📁 Folder

Organizational container block.

### **Folder Block:**

Used to create organizational folders within the Hierarchy or other nodes.

**How to use:**
1. Drag **Folder** block from Toolbox
2. Name the folder
3. Drag other blocks into the folder

**Purpose:** Keep logic organized by equipment, system, or location.

---

## 🔧 Units and Conversions

Many blocks automatically handle units:

### **Unit Propagation:**

- When "Propagate Units" is enabled on a link, units flow from input to output
- Math blocks (Add, Subtract) check for compatible units
- **Subtract** two temperatures (°F) → outputs delta temperature (ΔF)
- **Add** temperature (°F) + delta (ΔF) → outputs temperature (°F)

### **Unit Conversion Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Convert Units** | Change units (°C → °F, Pa → inWC) | Normalizing vendor-specific units |
| **Scale** | Multiply by constant | Temperature scaling (value / 10) |
| **Assign Units** | Apply units to dimensionless value | Adding units to unitless BACnet points |

### **Example: Converting Celsius to Fahrenheit**

```
[BACnet: Temp in °C] ──> [Convert Units: °C → °F] ──> [BasiX: Temp in °F]
```

---

## 🎨 Using Blocks in Flow View

### **Step 1: Open Flow View**

Navigate to a Hierarchy folder or equipment node. The Flow View workspace opens in the center, and the Toolbox appears on the right.

---

### **Step 2: Drag Blocks from Toolbox**

1. Find the block you need in the Toolbox
2. Click and drag it onto the Flow View canvas
3. Drop it in the workspace

---

### **Step 3: Configure the Block**

1. Click on the block to select it
2. The **Edit Block** panel appears on the right
3. Set properties:
   - Block name
   - Input values
   - Configuration options
   - Units

---

### **Step 4: Connect Blocks with Links**

1. Click on an output pin of one block
2. Drag to the input pin of another block
3. The link connects the two blocks
4. Right-click the link to configure:
   - Unit propagation
   - Fault forwarding
   - Link properties

Use **Link View** to see all connections into and out of a block at a glance:

![Link View](../img/Link View.PNG)

---

### **Step 5: Test and Validate**

1. Check the block outputs in the Edit Block panel
2. Verify units are correct
3. Watch for fault status or NaN values
4. Use the **Watch** feature to monitor live values

---

## 💡 Common Block Patterns

### **Pattern 1: Reading and Scaling**

```
[BACnet Point] ──> [Divide by 10] ──> [Assign Units: °F] ──> [Output]
```

**Use case:** Controller sends temp as 723 instead of 72.3°F

---

### **Pattern 2: Fault Detection**

```
[BACnet: Fan Status] ──┐
                       │
                    [AND] ──> Fault Alarm
                       │
[BACnet: Temp > 100°F] ┘
```

**Use case:** Detect when fan is off but temp is high

---

### **Pattern 3: Virtual Point Creation**

```
[BACnet: kW Meter 1] ──┐
[BACnet: kW Meter 2] ──┼── [Add] ──> Total Building kW ──> [BasiX: Power]
[BACnet: kW Meter 3] ──┘
```

**Use case:** Create building-level total that doesn't exist as a physical point

---

### **Pattern 4: Unit Conversion**

```
[BACnet: Pressure in Pa] ──> [Convert: Pa → inWC] ──> [BasiX: Pressure]
```

**Use case:** Vendor uses Pascals, BasiX expects inches of water column

---

### **Pattern 5: Computed Efficiency**

```
[BACnet: Chiller Tons] ──┐
                         │
                      [Divide] ──> kW/Ton ──> [BasiX: Efficiency]
                         │
[BACnet: Chiller kW] ────┘
```

**Use case:** Calculate chiller efficiency metric

---

## ⚠️ Common Mistakes

### **1. Forgetting Units**

❌ **Problem:** Dimensionless BACnet point connected directly to BasiX field expecting units

✅ **Solution:** Use **Assign Units** block before mapping to BasiX

---

### **2. Unit Mismatches**

❌ **Problem:** Adding temperature (°F) + pressure (psi) = fault

✅ **Solution:** Use **Convert Units** to make them compatible, or disable unit propagation

---

### **3. Absolute vs Delta Temperature**

❌ **Problem:** Multiplying or dividing absolute temperature (°F)

✅ **Solution:** Convert to delta temperature (ΔF) first, or subtract 0°F

---

### **4. Circular References**

❌ **Problem:** Block A links to Block B, Block B links to Block A

✅ **Solution:** Redesign logic to avoid circular dependencies

---

### **5. Missing Status Checks**

❌ **Problem:** Using values from faulted or offline points

✅ **Solution:** Use **Status Monitor** blocks to check reliability before processing

---

## 📝 Toolbox Best Practices

1. **Start simple** - Build one piece of logic at a time
2. **Test frequently** - Verify each block works before connecting more
3. **Use clear names** - Rename blocks from defaults (`Add_1` → `Total_Building_Load`)
4. **Check units** - Always verify units are propagating correctly
5. **Monitor status** - Use status blocks to detect faulty inputs
6. **Organize with folders** - Group related logic together
7. **Document complex logic** - Add descriptions to blocks
8. **Follow patterns** - Use proven patterns for common tasks
9. **Validate outputs** - Check that final values make sense
10. **Plan for BasiX** - Build logic with BasiX mapping in mind

---

## ➡️ Related Documentation

- **[Data Transformation & Logic Flow](../concepts/data-transformation.md)** - Concepts behind block-based logic
- **[Hierarchy & Digital Twin](../user-guide/hierarchy-digital-twin.md)** - Where to organize your blocks
- **[Units & Formatting Best Practices](../concepts/units-formatting.md)** - Critical unit handling guidance
- **[BasiX Mapping Workflow](../user-guide/basix-mapping-workflow.md)** - How to map block outputs to BasiX
- **[Explorer Structure](../user-guide/explorer-structure.md)** - Understanding the UI navigation
