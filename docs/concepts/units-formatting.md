# 📏 Units & Formatting Best Practices

DataLynX includes powerful unit handling, formatting rules, and data validation features to ensure that values coming from BACnet devices are presented correctly, normalized for analytics, and safely propagated through the Data Processing Layer.

**Proper unit handling is critical** because different BAS vendors use inconsistent or non-standard engineering units, scaling factors, and precision. Incorrect or missing units will prevent successful mapping to BasiX profiles and can cause data quality issues in BDX.

This guide explains how DataLynX manages units, handling of dimensionless values, temperature calculations, and provides comprehensive formatting recommendations for all common HVAC measurements.

---

## 🎯 1. Why Units Matter

BACnet points may come in with:

- Incorrect units  
- No units at all  
- Unexpected scaling (e.g., temperature x10)  
- Vendor-specific conventions  
- Values represented as integers when they should be floats  

To prevent bad data from reaching analytics, DataLynX:

- Propagates units through the Data Processing Layer
- Converts units where possible
- Flags incompatible or missing units
- Applies default formatting rules
- Generates normalized output for BasiX

**Critical**: BasiX profiles require proper units on all mapped points. Missing or incorrect units will prevent device publishing to BDX.  

---

## 🧩 2. Unit Propagation

Each value in the Data Processing Layer contains:

- Value
- Units
- Status
- Reliability
- Timestamp

Links between blocks may propagate units depending on configuration.

### ✔ If "Propagate Units" is enabled:

- Linked properties must have *compatible units*
- Mismatches may trigger **fault status**, often resulting in **NaN outputs**

### ✔ If disabled:

- Units are not required to match
- Useful for advanced or custom logic
- Use with caution

---

## 🔧 3. Dimensionless Units in BACnet

BACnet points can legally have **dimensionless** or **no units**—this is fully supported in the BACnet standard.

## How BACnet Handles "No Units"

BACnet uses an enumeration called `EngineeringUnits`. Among the many unit codes, there is a specific one for dimensionless values:

- **UNITLESS** (sometimes called dimensionless, no units, or unitless)
- This means the value has meaning but is not tied to a physical quantity like temperature, pressure, flow, etc.

## Where You Commonly See Dimensionless Units

BACnet objects frequently use dimensionless units when the value is:

- **A percentage represented as 0–1** instead of 0–100
  - Example: valve position, VFD speed

- **A raw count**
  - Example: number of starts, cycle counts

- **A ratio**
  - Example: mixing percentages, humidity ratios

- **A custom engineering value** that the vendor defines without mapping to a physical unit
- **Boolean-like analog values**
  - Example: 0 = off, 1 = on, but stored in an Analog Input

## Dimensionless Units in Practice

| Object | Example Value | Why Dimensionless? |
|--------|--------------|-------------------|
| Mixed air ratio | 0.65 | Pure ratio |
| Filter status score | 13 | Vendor-specific scale |
| VFD speed fraction | 0.82 | Not a physical "speed" unit |
| Occupancy "level" | 1 | Mode indicator stored as analog |

## Important Notes

- Many devices incorrectly use **Percent** (0–100%) for values that are actually 0–1 fractions, so "dimensionless" is often a cleaner choice
- Trend and analytics systems prefer knowing when a value is unitless so they don't try to convert it or compare it incorrectly
- **DataLynX behavior with dimensionless units**:
  - DataLynX will read or inherit BACnet units
  - Users can override units on points/blocks
  - Users can override units on linked blocks where units pass through links
  - Dimensionless units can have units added in DataLynX
  - DataLynX will auto-convert values when units are changed on non-dimensionless units
  - **For dimensionless units, it's up to the user to define the correct units when mapping to BasiX profile fields that expect units** (e.g., airflow, temperature, pressure)

---

## 🌡️ 4. Temperature Units and Delta Temperature

## Critical Difference: Absolute vs Delta Temperature

**Delta degree Fahrenheit (ΔF)** is different than **degrees Fahrenheit (°F)** when calculating temperature differences.

### When to Use Delta Temperature

For calculations involving temperature differences:

- Space temp vs space temp setpoint
- Supply air temp vs supply air temp setpoint
- Return temp vs supply temp

**You cannot use absolute temperature units to represent the difference between two temperatures.**

### Math Operations with Temperature

#### ✔ Addition (requires temp + delta):
For an **Add** block, you need:

- One input: absolute temperature (°F)
- Other input: delta temperature (ΔF)
- Output: absolute temperature (°F)

#### ✔ Subtraction and Mean (works correctly):
**Subtract** or **Mean** blocks work correctly with absolute temperatures:

- Input: Two absolute temperatures (°F, °F)
- Output: Delta temperature (ΔF)

### Why This Matters

Degrees Celsius and Fahrenheit are **offset units**, measured using a basis. Multiplying or dividing them requires conversion to base units (Kelvin).

**Example of the problem**:

- Dividing 70°C by 2 translates to -101°C when forcefully converted back to Celsius
- You probably expected 35°C
- This happens because absolute temperatures aren't additive/multiplicative

### Best Practices

To avoid these issues:
1. Convert degrees to delta degrees by:
   - Using the **Override Source Units** flag, OR
   - Subtracting 0°F from the source value
2. Then perform multiplication/division operations
3. **Bottom line**: Do not multiply/divide absolute temperatures unless you expect absolute values in Kelvin as the result
4. **Only deltas can be multiplied or divided**

---

## 🔄 5. Property Configuration & Flags

When configuring properties in DataLynX, you have access to several flags that control how the property behaves within the system. These are accessed through the **Configure Property** dialog.

![Configure Property Dialog](../img/DataLynX - Configure Units.PNG)

### Property Flags

| Flag | Description |
|------|-------------|
| **Transient** | The property can be linked, but its value is not persisted to the Agent Configuration Database. Use this for temporary or calculated values that don't need to survive an agent restart. This flag cannot be used with MetadataProperty or PersistedValue declarations. |
| **Summary** | Always displays the property in the Flow View, even if it is not linked. Useful for key values you want visible at a glance without opening the block details. |
| **Readonly** | Prevents the property value from being modified. Use this for values that should only be read from their source and not overwritten. |
| **Override Source Units** | Allows you to override the incoming or native units on a block and change them to something else. See details below. |

### Override Source Units

**Override Source Units** is the flag that allows DataLynX users to override the incoming or native units on a block and change it to something else.

**When to use this feature:**

- A controller has wrong units selected in device configuration
- A manufacturer used incorrect units in their integration
- The value needs to be maintained in DataLynX with just the unit changed

**How it works:**

1. Check the **Override Source Units** checkbox
2. Select the correct units from the **Unit** dropdown
3. DataLynX will treat the value as having the new units
4. **Important**: This does NOT convert the value—it reinterprets the existing value with different units

### Unit and Value Format Options

The Configure Property dialog also provides:

- **Unit** — Select the engineering units for this property
- **Value Format** — Define how the value is displayed (decimal places, formatting)

---

## 🛠️ 6. HVAC Measurement Formatting Reference

The following table provides recommended formatting for all common HVAC and energy management measurements. These recommendations ensure consistent, readable displays throughout DataLynX and BDX.

## Complete Formatting Reference Table

| Measurement Type | Example Units (IP) | Recommended Format | Example Output |
|-----------------|-------------------|-------------------|----------------|
| **Temperature** | °F | `.1f` | 72.5 °F |
| **Delta Temperature (CHW ΔT)** | ΔF | `.2f` | 12.47 °F |
| **Relative Humidity** | % | `.0f` | 45 % |
| **CO₂ Concentration** | ppm | `,.0f` | 1,024 ppm |
| **Airflow** | cfm | `,.0f` | 1,250 cfm |
| **Pressure** | inH₂O | `.2f` | 1.25 inH₂O |
| **Differential Pressure (Filter/Coil)** | inH₂O | `.3f` | 0.245 inH₂O |
| **Pressure (Fluid)** | psi | `.1f` | 35.4 psi |
| **Power** | kW | `.1f` | 18.7 kW |
| **Energy** | kWh | `,.0f` | 3,542 kWh |
| **Thermal Energy** | ton·hr | `,.0f` | 1,280 ton·hr |
| **Flow Rate (Water)** | gpm | `.1f` | 245.6 gpm |
| **Setpoints** | °F, psi, % | `.1f` | 68.0 °F |
| **Valve or Damper Position** | % | `.0f` | 75 % |
| **Fan / Pump Speed** | % | `.0f` | 60 % |
| **Runtime / Hours** | hr | `,.1f` | 1,240.5 hr |
| **Power Factor** | (unitless) | `.3f` | 0.982 |
| **Frequency** | Hz | `.2f` | 59.97 Hz |
| **Voltage** | V | `.1f` | 480.2 V |
| **Current** | A | `.2f` | 12.45 A |

## Format Code Reference

| Code | Meaning | Example |
|------|---------|---------|
| `.0f` | No decimals (rounded) | 72 |
| `.1f` | One decimal | 72.5 |
| `.2f` | Two decimals | 12.47 |
| `.3f` | Three decimals | 0.245 |
| `,.0f` | Thousands separator, no decimals | 1,024 |
| `,.1f` | Thousands separator, one decimal | 1,240.5 |

## General Formatting Tips

- Use `,.0f` for large cumulative totals (kWh, ton·hr, runtime hours)
- Use `.2f` for precision control metrics (ΔT, pressure, energy rate)
- Avoid more than two decimals unless troubleshooting sensors or doing diagnostics
- Always pair unit formatting with unit symbols so displays automatically adapt if engineering units change
- For **Binary / Enum States**: Always show the **label**, not the numeric value
  - Example: `Off`, `Auto`, `Occupied`, `Unoccupied`  

---

## 📘 7. Physical Unit Conversions

The Data Processing Layer supports automatic unit conversions such as:

- °C ↔ °F
- Pa ↔ inWC
- kW ↔ W
- Tons ↔ kBTU/hr

Conversions occur *only* when units are defined and compatible.

If a BACnet point has no unit:

- A default may be assigned during mapping
- Or the user may manually set the expected unit
- **Remember**: BasiX profile fields that expect physical measurements (temperature, airflow, pressure, etc.) require proper units, so dimensionless values must have units assigned before mapping to these fields  

---

## ❗ 8. Handling Incorrect or Missing Units

If a BACnet point:

- Has wrong units
- Has no units
- Has inconsistent metadata

You can correct this by:

1. Using the **Override Source Units** flag (see Section 5)
2. Applying a conversion block in Flow View
3. Explicitly assigning units in a mapping block
4. Using fallback or override logic
5. Normalizing the value before feeding it to BasiX

**Best Practice**: Devices with chronically incorrect units should be fixed upstream in the BAS, but DataLynX allows on-the-fly correction.

**Critical for BasiX**: All points mapped to BasiX profiles must have correct units. Missing or incorrect units will prevent device publishing to BDX.

---

## 🔍 9. Typical Examples

### ✔ Example 1: Temperature Scaling
Some devices send temperatures like `734` to represent `73.4°F`.

**Solution** in Flow View:
```
Value / 10 → Assign Unit °F
```

### ✔ Example 2: Pressure in Pascals
If a vendor supplies filter differential pressure in **Pa**, but BasiX expects **inWC**:

**Solution**:
```
Pa → ConvertTo(inWC)
```

### ✔ Example 3: Dimensionless VFD Speed
A VFD reports speed as a 0-1 fraction with dimensionless units, but BasiX expects %.

**Solution**:
1. Add Convert block
2. Multiply by 100
3. Assign unit: %

### ✔ Example 4: Wrong Units from Controller
A temperature sensor is configured as **psi** instead of **°F** in the BACnet controller.

**Solution**:
1. Select the BACnet point block
2. Check **Override Source Units**
3. Select **°F**
4. Value remains the same, only unit interpretation changes

### ✔ Example 5: Enum Mapping
Some devices send binary states as numbers:

```
0 = Off
1 = On
```

**Solution** - Map to BasiX-friendly labels:
```
0 → Off
1 → On
```

---

## 📝 10. Summary

Proper unit and formatting handling is **critical for DataLynX success**:

- **BasiX Requirement**: All mapped points must have correct units to publish devices to BDX
- **Data Quality**: Clean, consistent values ensure accurate analytics
- **Automation**: Reliable downstream automation depends on proper units
- **Debugging**: Correct units make troubleshooting easier
- **Standardization**: Campus-wide consistency across all equipment

## Key Takeaways

1. **Dimensionless units are valid** in BACnet but must be assigned proper units when mapping to BasiX fields that expect physical measurements (temperature, airflow, pressure, etc.)
2. **Delta temperatures (ΔF)** are different from absolute temperatures (°F)—use the right one for calculations
3. **Override Source Units** flag allows you to reinterpret (not convert) units on BACnet points
4. **Use the formatting reference table** (Section 6) for consistent, readable displays
5. **Missing or wrong units on fields expecting physical measurements will prevent BasiX device publishing**—always validate units before mapping

By following these best practices, your BasiX data becomes robust, readable, and ready for professional-grade analytics in BDX.

---

## ➡️ Related Documentation

- **[Data Transformation & Logic Flow](data-transformation.md)** - Learn about Flow View and logic blocks
- **[BasiX Ontology & Profiles](basix-ontology.md)** - Understand BasiX device templates and requirements
- **[BasiX Mapping Workflow](../user-guide/basix-mapping-workflow.md)** - Step-by-step mapping process
- **[Terminology & Definitions](../reference/terminology.md)** - Key DataLynX terms explained

