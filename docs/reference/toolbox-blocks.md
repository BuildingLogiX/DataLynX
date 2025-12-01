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

### **Math Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Absolute Value** | Returns the absolute value of a number | Removing negative signs, distance calculations |
| **Add** | Sum of two or more values | Total energy consumption, combined airflow |
| **Arccosine** | Inverse cosine function | Trigonometric calculations |
| **Arcsine** | Inverse sine function | Trigonometric calculations |
| **Arctangent** | Inverse tangent function | Angle calculations |
| **Cosine** | Cosine function | Trigonometric calculations |
| **Divide** | Quotient of two values | Efficiency (kW/ton), per-square-foot metrics |
| **Exponential** | Exponential function (e^x) | Growth calculations |
| **Factorial** | Factorial of a number | Statistical calculations |
| **Logarithm Base 10** | Base 10 logarithm | Decibel calculations, scaling |
| **Logarithm Base 2** | Base 2 logarithm | Binary scaling |
| **Modulus** | Remainder after division | Cyclic calculations, scheduling |
| **Multiply** | Product of two values | Power calculations, scaling factors |
| **Natural Logarithm** | Natural logarithm (ln) | Exponential decay calculations |
| **Negate** | Reverses the sign of a value | Sign inversion |
| **Power** | Raises a value to a power | Exponential calculations |
| **Sine** | Sine function | Trigonometric calculations |
| **Square Root** | Square root of a value | RMS calculations |
| **Subtract** | Difference between values | Delta temperature (ΔT), pressure drop |
| **Tangent** | Tangent function | Trigonometric calculations |
| **Value Reset** | Resets a value to a default | Initialization, fault recovery |

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

### **Logic Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **And** | All inputs must be true | Multiple conditions (fan on AND valve open) |
| **Not** | Invert boolean value | Opposite of a condition (NOT occupied) |
| **Or** | At least one input must be true | Alarm conditions, any zone over setpoint |
| **Xor** | Exclusive OR - one or the other but not both | Either/or logic (heating XOR cooling) |

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

### **Comparison Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Equal** | Check if A equals B | Mode matching, status verification |
| **Greater Than** | Check if A > B | Temp above setpoint, power exceeds threshold |
| **Greater Than or Equal** | Check if A >= B | At or above threshold |
| **Less Than** | Check if A < B | Temp below setpoint, low pressure alarm |
| **Less Than or Equal** | Check if A <= B | At or below threshold |
| **Not Equal** | Check if A does not equal B | Detecting changes |

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

Blocks for BACnet network configuration and point references.

### **BACnet Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **BACnet Device** | Represents a BACnet device on the network | Reference discovered or manually added devices |
| **BACnet Device Folder** | Container for organizing BACnet devices | Group devices by location or system |
| **BACnet IP Network** | BACnet/IP network configuration | Define network interface, port, and network number |
| **BACnet Point Folder** | Container for organizing points within a device | Group points by type or function |
| **BACnet Router Table Entry** | Router table configuration for remote networks | Access devices on routed networks (MSTP, etc.) |

### **How to Use:**

1. Configure **BACnet IP Network** with your network settings
2. Discover or manually add **BACnet Device** entries
3. Use **BACnet Point Folder** to organize points within devices
4. Reference points in Flow View for data transformation
5. The block outputs the present value, units, status, and reliability

### **Example: Reading Supply Air Temp**

```
[BACnet: /Connections/BACnet/devices/AHU-1/AI-SAT]
   │
   ├─> out (value: 55.2°F)
   ├─> units (°F)
   ├─> status (OK)
   └─> reliability (Reliable)
```

> **Note:** DataLynX is a read-only BACnet client. It reads point values from BACnet devices but does not write to them.

---

## 🧬 BasiX Profiles

Blocks representing BasiX device profile fields.

### **Common BasiX Profile Blocks:**

These blocks represent standardized points in BasiX profiles:

| Block | Description |
|-------|-------------|
| **supplyAirTemp** | AHU supply/discharge air temperature |
| **zoneAirTemp** | VAV zone temperature |
| **damperPosition** | VAV damper position |
| **coolOutput** | Cooling coil valve position |
| **supplyFanVFDSpeed** | Fan or VFD speed |
| **airflow** | Measured airflow (cfm) |
| **power** | Equipment power consumption (kW) |
| **runStatus** | Equipment on/off or run status |

### **How to Use:**

1. Drag a BasiX Profile block from the Toolbox
2. Map processed values to the block inputs
3. These blocks become the output points for BasiX mapping
4. They are published to BDX

### **Example: Mapping AHU Supply Temp**

```
[BACnet: AHU-1 Supply Temp] ──> [Divide by 10] ──> [BasiX: supplyAirTemp]
                                                            │
                                                      (Sent to BDX)
```

---

## 🔄 Switches and Selects

Conditional routing and selection blocks for different data types.

### **Switch/Select Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Boolean Select** | Choose between boolean inputs based on selector | Select active alarm source |
| **Boolean Switch** | Route boolean input based on condition | Toggle between on/off states |
| **Enum Select** | Choose between enumerated inputs based on selector | Select operating mode source |
| **Enum Switch** | Route enumerated input based on condition | Switch between mode options |
| **Numeric Select** | Choose between numeric inputs based on selector | Select between sensors |
| **Numeric Switch** | Route numeric input based on condition | Occupancy-based setpoints |
| **String Select** | Choose between string inputs based on selector | Select message source |
| **String Switch** | Route string input based on condition | Switch between text labels |

### **Example: Occupancy-Based Setpoint**

```
Occupied Setpoint (72°F) ──┐
                           │
Occupancy Status ────> [Numeric Switch] ──> Active Setpoint
                           │
Unoccupied Setpoint (65°F) ┘
```

---

## 📊 Aggregate Functions

Aggregation and statistical operations for multiple inputs.

### **Aggregate Function Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Add** | Sum of multiple inputs | Total energy, combined load |
| **Average** | Mean of all inputs | Average zone temp across floor |
| **Maximum** | Highest value among inputs | Peak demand, highest zone temp |
| **Median** | Middle value of inputs | Statistical analysis, outlier filtering |
| **Minimum** | Lowest value among inputs | Lowest zone temp, minimum setpoint |
| **Mode** | Most frequent value among inputs | Common operating state |
| **Standard Deviation** | Statistical deviation of inputs | Variability analysis, anomaly detection |

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

Virtual points for storing and managing values.

### **Control Point Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Boolean Point** | Stores a true/false value | On/off states, mode flags |
| **Boolean Priority Array** | Boolean with 16-level priority | Priority-based boolean control |
| **Enumerated Point** | Stores a value from a defined set of options | Operating modes, state machines |
| **Enumerated Priority Array** | Enumerated with 16-level priority | Priority-based mode control |
| **Numeric Point** | Stores a numeric value | Setpoints, thresholds, calculated values |
| **Numeric Priority Array** | Numeric with 16-level priority | Priority-based setpoint control |
| **String Point** | Stores a text value | Labels, descriptions, messages |
| **String Priority Array** | String with 16-level priority | Priority-based text values |

### **Example: Numeric Point (Setpoint)**

```
[Numeric Point: 72.0°F] ──> Used in comparison or control logic
```

---

## 📡 Status

Status monitoring and signal routing blocks.

### **Status Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Status Demultiplexer** | Separates combined status into individual signals | Parse composite status values, route status bits |

### **Example: Status Demultiplexer**

```
[Combined Status] ──> [Status Demultiplexer] ──> Individual status outputs
                              │
                          (Separates fault, alarm, mode flags)
```

---

## 🔢 Priority Arrays

Priority array blocks for managing values with BACnet-style 16-level priorities.

### **Priority Array Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **Boolean Priority Array** | Boolean value with 16-level priority | Priority-based on/off control |
| **Enumerated Priority Array** | Enumerated value with 16-level priority | Priority-based mode selection |
| **Numeric Priority Array** | Numeric value with 16-level priority | Priority-based setpoint control |
| **String Priority Array** | String value with 16-level priority | Priority-based text values |

Priority arrays allow multiple sources to command a value at different priority levels. The highest active priority (lowest number) wins.

---

## 📦 BDX

Blocks for BDX connectivity configuration.

### **BDX Blocks:**

| Block | Description | Use Case |
|-------|-------------|----------|
| **BDX Agent Configuration** | Configure connection to BDX | Set up BDX URL, credentials, and sync settings |

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
- **[eXplorer Structure](../user-guide/explorer-structure.md)** - Understanding the UI navigation
