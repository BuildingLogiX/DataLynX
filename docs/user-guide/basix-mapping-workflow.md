# ![DataLynX Logo](../assets/datalynx_logo.svg){ width="50" } BasiX Mapping Workflow

This guide walks you through the full workflow for converting raw BACnet points into clean, normalized **BasiX device profiles** using DataLynX.
This is the final step before sending standardized data to BDX.

---

## 1. Overview

The BasiX Mapping Workflow covers:

1. Identifying BACnet points for each device
2. Selecting or creating a BasiX profile (AHU, VAV, Pump, etc.)
3. Mapping raw points to BasiX fields
4. **Ensuring proper units on all mapped points** ⚠️
5. Applying conversions or logic if needed
6. Validating required fields
7. Reviewing device health and mapping completeness
8. Preparing devices for BDX upload

**Critical**: All points mapped to BasiX profiles must have proper units. Missing or incorrect units will prevent device publishing to BDX.  

---

## 2. Where Mapping Happens

In the eXplorer tree, BasiX mapping schemes are located at:

```
System → BasiX → mapping_schemes → [Device Type]
```

Here you will see:

- Profile templates for different device types (VAV, AHU, Chiller, etc.)
- Mapping scheme configurations for each profile
- Field definitions for BasiX standard points

![BasiX Mapping Scheme Editor](../img/System - BasiX - VAV Mapping Scheme.PNG)

The mapping scheme editor allows you to:

- Define paths to BACnet points using patterns
- Specify which property to read (typically `out`)
- Configure input properties for fields that need additional data
- Link Hierarchy blocks to BasiX profile fields

---

## 3. Saving & Reusing Mapping Schemes

One of the most powerful features of DataLynX is the ability to **save mapping configurations as reusable schemes** that can be applied to multiple devices. This dramatically reduces setup time when integrating sites with many similar devices.

### How Mapping Schemes Work

When you create a BasiX profile and configure its point mappings, you can save that configuration as a **mapping scheme**. This scheme becomes a template that can be applied to other devices of the same type.

**Key Benefits:**

- **Time Savings**: Configure once, apply to dozens or hundreds of similar devices
- **Consistency**: All devices using the same scheme have identical mapping rules
- **Maintainability**: Update the scheme once to affect all linked devices

### Creating a Reusable Mapping Scheme

1. Create a BasiX device and configure all point mappings
2. Use pattern matching (wildcards like `*`) for device-specific identifiers
3. Save the profile as a mapping scheme
4. The scheme appears under `System → BasiX → mapping_schemes → [Device Type]`

**Example:**
For 50 VAVs with naming convention `VAV-01` through `VAV-50`:

```
BACnet Pattern: /VAV-*/AI-RM-T → zoneAirTemp
BACnet Pattern: /VAV-*/AO-DMP → damperPosition
```

This single scheme maps all 50 VAVs automatically.

### Linked vs Private (Rogue) Schemes

**Linked Scheme (Default):**

- Multiple devices share the same mapping scheme
- **Editing the scheme updates ALL linked devices**
- Ideal for standardized equipment with consistent naming

**Private Scheme (Rogue):**

- A device uses its own independent mapping configuration
- Changes affect only that specific device
- Useful for one-off devices or equipment with unique naming conventions

**To make a scheme private:**

1. Select the device using the shared scheme
2. Choose to "disconnect" or "go rogue" from the linked scheme
3. The device now has its own private mapping configuration
4. Future edits to the original scheme will not affect this device

!!! warning "Consider Before Editing Shared Schemes"
    When you edit a mapping scheme that is linked to multiple devices, **all linked devices will be affected**. Always verify which devices are using a scheme before making changes. If you need to customize mapping for a specific device without affecting others, make that device's scheme private first.

---

## 4. Creating a BasiX Device

### Automatic Creation  
If your BACnet naming conventions are consistent (e.g., `VAV-01` through `VAV-100`), DataLynX may automatically detect logical groups.

### Manual Creation  
You can create a BasiX device manually:

1. Navigate to:

   ```
   BasiX → Devices → New Device
   ```

2. Choose a **BasiX Profile** (device type), such as:

- AHU  
- VAV  
- Pump  
- Chiller  
- Cooling Tower  
- Exhaust Fan  
- Heat Pump  
- Fan Coil Unit  

3. Provide a device name and optional metadata.

---

## 5. Mapping Modes You Can Use

When mapping BACnet points to BasiX fields, you may choose:

- **Exact mapping**  
- **Pattern mapping** (`*` and `?`)  
- **Regex mapping** (advanced)

Each profile contains required and optional fields.

---

## 6. Mapping Example (VAV)

Suppose you create a VAV BasiX device for `VAV-12`.

You might map:

| BACnet Point | BasiX Point |
|--------------|-------------|
| `/VAV-12/AI-RM-T` | `zoneAirTemp` |
| `/VAV-12/AI-SAT` | `dischargeAirTemp` |
| `/VAV-12/AO-DMP` | `damperPosition` |
| `/VAV-12/AV-FLOW` | `airflow` |
| `/VAV-12/BO-HCMD` | `heatCommand` |

If dozens of VAVs follow the same naming pattern, you can create a **pattern mapping**:

```
BACnet Pattern: /VAV-*/AI-RM-T
BasiX Point: zoneAirTemp
```

---

## 7. Using Flow View for Value Conditioning

Before mapping to BasiX, you may need to apply logic such as:

- **Assigning units to dimensionless values** ⚠️
- Converting units (°C to °F, Pa to inWC)
- Dividing by scaling factors (e.g., value / 10)
- Replacing null values
- Computing ΔT (delta temperature)
- Cleaning unreliable values
- Re-mapping enumerations

Example:

```
RawTemp / 10 → Assign Units °F → Map to BasiX supplyAirTemp
```

**Important**: Many BACnet points have dimensionless or missing units. You must assign proper units before mapping to BasiX profiles.

Use **Flow View** to build logic with blocks.

See **[Units & Formatting Best Practices](../concepts/units-formatting.md)** for detailed guidance.

![Flow View](../img/Flow View.PNG)

---

## 8. Required vs Optional Points

Each BasiX Profile defines:

- **Required points** (must be mapped before sending to BDX)
- **Optional points** (increase analytics completeness)

Missing required points will show warnings like:

> ⚠️ Required field "supplyAirTemp" is unmapped

You can still save the device but cannot publish to BDX until the profile is complete.

---

## 9. Device Completeness Indicators

After mapping a BasiX device, DataLynX displays:

- ✔ **Complete** – All required fields mapped  
- ⚠️ **Partial** – Some required fields unmapped  
- ❌ **Invalid** – No valid mappings or major errors  

You can filter all mapped devices by status.

---

## 10. Publishing to BDX

Once mappings are valid:

1. Ensure the BDX connection is enabled  
2. Navigate to:

   ```
   BasiX → Devices
   ```

3. Select a device  
4. Click:

   ```
   Publish to BDX
   ```

BDX will receive a normalized BasiX structure, enabling analytics, dashboards, and alarms.

---

## 11. Troubleshooting Common Mapping Issues

### ❌ No points appear in the mapping search

- The BACnet network may not be configured
- The device may not have completed discovery
- Polling may be disabled

### ❌ Cannot publish device to BDX
**Most common cause**: Missing or incorrect units on mapped points

- Check all mapped points have proper units
- Look for dimensionless values that need units assigned
- Use **Override Source Units** flag for points with wrong units
- See **[Units & Formatting](../concepts/units-formatting.md)** for guidance

### ❌ Units mismatch

- Enable "Propagate Units" for debugging
- Add a conversion block before mapping
- Check for absolute vs delta temperature issues
- Verify dimensionless values have units assigned

### ❌ Required fields unmapped

- Review the BasiX profile to see mandatory fields
- Ensure pattern or regex mappings match correctly
- Check that all required fields have proper units

### ❌ Values look wrong in BDX

- Check Flow View for scaling
- Use Watch to inspect raw values
- Confirm mapping order and priority
- Verify unit conversions are correct  

---

## Summary

The BasiX Workflow:

1. Identify devices  
2. Create BasiX profiles  
3. Map BACnet points  
4. Normalize units and logic  
5. Validate required points  
6. Publish to BDX  

This workflow transforms raw field data into clean, standardized analytics-ready information.

---

## ➡️ Next Step

Proceed to the **Admin Guide** beginning with:

👉 **[Windows Services & Logs](../admin/windows-services-logs.md)**

