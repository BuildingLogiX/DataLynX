# Mapping Modes in DataLynX

The Mapping Layer in DataLynX is responsible for converting raw BACnet points into structured BasiX points.  
Because building automation systems vary widely in naming, structure, and point availability, DataLynX supports **three powerful mapping modes**:

1. **Exact Mapping**  
2. **Pattern (Glob) Mapping**  
3. **Regex (Regular Expression) Mapping**

This page explains each mode, when to use it, and how they fit into the BasiX normalization process.

---

## 1. Why Mapping Matters

Raw BACnet metadata is rarely standardized.  
Examples you may encounter:

- `AI-1`, `AI-Temp1`, `T_SF`, `SA-T`, `DischargeTemp`
- `BO-CV`, `CCV-CMD`, `CoolingValveCmd`, `CV_OUT`
- `VAV-01`, `VAV-002`, `VAV-A`, `VAV_NorthWing-12`

To create reliable, analytics-ready data:

- Every device must map to a **BasiX Profile**
- Every BasiX point must have **a source**
- Mapping rules must survive **naming inconsistencies**
- Users must be able to maintain large systems with **repeatable patterns**

Mapping modes provide the flexibility needed for these realities.

---

## 2. Exact Mapping

Exact Mapping creates a strict one-to-one relationship:

```
Specific BACnet Point → Specific BasiX Point
```

### ✔ Use Exact Mapping When:

- You have a small system  
- Names are clean and consistent  
- You want explicit links for clarity  
- You are troubleshooting or verifying values  

### Example:

```
BACnet Path: /AHU-1/AI-SAT
BasiX Point: supplyAirTemp
```

If the name does not match exactly, the mapping will not apply.

---

## 3. Pattern Mapping (Glob)

Pattern mapping uses simple wildcards:

- `*` — any number of characters  
- `?` — exactly one character  

This is helpful when working with repeated devices, such as:

- VAVs  
- Fan coils  
- Heat pumps  
- Room controllers  
- Lighting zones  

### ✔ Example: VAV Temperature

```
BACnet Pattern: /VAV-*/AI-RM-T
BasiX Point: zoneAirTemp
```

This will match:

- `/VAV-1/AI-RM-T`
- `/VAV-27/AI-RM-T`
- `/VAV-EAST-WING/AI-RM-T`

### ✔ Example: Numbered Devices

```
/HP-??/AO-CMD
```

Matches:

- `HP-01`  
- `HP-12`  
- Not `HP-123`  

---

## 4. Regex Mapping (Advanced)

Regex mapping uses **full regular expressions**, allowing the most powerful matching method.

Use this mode when:

- Naming conventions are inconsistent  
- There are optional prefixes/suffixes  
- Strings include unpredictable separators  
- Multiple segments must be parsed  
- Vendor naming is inconsistent across floors or controllers  

### ✔ Example: Match all VAV room temp sensors with odd conventions

```
Regex: ^/VAV[-_ ]?(\d{1,3})/((AI|AV|AI-TEMP|RM-TEMP).*)
Maps to: zoneAirTemp
```

This rule would match:

- `/VAV-1/AI1`
- `/VAV_05/RM-TEMP`
- `/VAV 12/AI-TEMP`
- `/VAV-003/AV-TEMP`

### ✔ Example: Match all “Cooling Valve Commands” regardless of spelling

```
Regex: (CCV|COOLING.*VALVE.*CMD|CV_CMD|CVALVE)
Maps to: coolingValveCommand
```

Regex mapping is extremely powerful and enables cross-vendor normalization.

---

## 5. Best Practices

### ✔ Use Exact Mapping for:

- AHUs  
- Chillers  
- Boilers  
- Pumps  
- One-off equipment  

### ✔ Use Pattern Mapping for:

- Any repeated device count > 10  
- VAVs  
- Heat pumps  
- FCUs  
- VRF zones  

### ✔ Use Regex Mapping for:

- Messy systems  
- Mixed-vendor retrofits  
- Custom naming  
- Large campuses  
- Any naming where patterns are unpredictable  

---

## ➡️ Next Step

Proceed to **[Units & Formatting](units-formatting.md)**
to learn how DataLynX handles engineering units and best formatting practices.

