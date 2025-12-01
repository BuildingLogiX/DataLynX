# 📖 Terminology & Definitions

This page provides clear definitions of key terms and concepts used throughout the DataLynX documentation and user interface.

Understanding these terms will help you navigate the software, communicate with support, and follow documentation more effectively.

---

## 🏗️ Core Components

### **Agent**
The running DataLynX instance that executes the actual BACnet driver, data processing, and BuildingLogiX processes.

When the agent is running, it:

- Loads configuration files
- Runs the BACnet driver for device communication
- Executes data processing and transformation logic
- Manages BasiX profiles and mappings
- Sends normalized data to BDX

**Important**: The agent is controlled from the **Supervisor page** in the UI (not from Windows Services). The Windows Services provide the infrastructure, but the agent runs the actual BACnet and BuildingLogiX processes.

You can have the Windows Services running while the agent is stopped.

---

### **Supervisor**
The top-level administrator role and control interface for the Agent. The Supervisor provides full access to:

- **Start and stop the Agent** - Control interface appears immediately after login
- Manage user accounts and permissions
- Access system-level configuration
- View and modify all settings
- Perform backups and restores

**Important**: When you first log into DataLynX, you land on the Supervisor view. If the agent is not running, you must click the **Start** button to launch the agent and see the full DataLynX eXplorer tree.

The Supervisor role has the highest level of privileges in DataLynX.

---

## 🧭 User Interface Elements

### **eXplorer**
The navigation tree (typically on the left side of the UI) that allows users to browse and organize:

- **System Node**: BasiX mapping schemes and backup service
- **Connections Node**: BACnet and BDX configuration
- **Hierarchy Node**: Digital twin workspace for custom logic and virtual equipment structure
- **Physical Networks**: BACnet devices, networks, and discovered points (under Connections)
- **Virtual Structures**: BasiX devices, logic blocks, and data transformations (under Hierarchy)
- **System Settings**: Configuration, logs, backups, and diagnostics
- **User Management**: Accounts, roles, and permissions

The eXplorer is the primary navigation tool for accessing all DataLynX features.

**Top-Level Structure:**
```
DataLynX (root)
├── System
├── Connections
├── Hierarchy
├── Supervisor
├── Manage Users
└── Manage Licenses
```

See [eXplorer Structure Guide](../user-guide/explorer-structure.md) for complete details.

---

### **System Node**
The top-level eXplorer node containing DataLynX system configuration:

- **BasiX** - Contains mapping schemes for device types (VAV, AHU, Chiller, etc.)
- **backup_service** - Automated backup scheduling and snapshot management

**Navigation:** `System → BasiX → mapping_schemes → [Device Type]`

---

### **Connections Node**
The top-level eXplorer node containing communication driver configuration:

- **BDX** - Connection settings for BuildingLogiX Data eXchange
- **BACnet** - BACnet driver configuration, hosted device, networks, and discovered devices

**Navigation:** `Connections → BACnet → devices` or `Connections → BDX`

---

### **Hierarchy Node**
The digital twin workspace—a flexible area where users can mirror physical building infrastructure, organize custom logic and calculations, and build BasiX profiles:

- Can mirror physical layout (campus, building, floor)
- Can group by equipment type (AHUs, VAVs, Chillers, etc.)
- Can organize by function (BasiX profiles, custom calculations, virtual meters)
- Build data transformation flows in whatever structure makes sense for your site
- Where BasiX device profiles for BDX are created
- Separate logical organization from physical BACnet device tree

**Example Structure:**
```
Hierarchy
├── Campus_Main
│   ├── Building_A
│   │   ├── AHUs
│   │   └── VAVs
│   └── Building_B
└── Central_Plant
    ├── Chillers
    └── Boilers
```

The Hierarchy is where Flow View logic lives and where you build your site's digital twin.

See [Hierarchy & Digital Twin](../user-guide/hierarchy-digital-twin.md) for complete guide.

---

### **Toolbox**
The panel (typically on the right side in Flow View) containing available blocks that can be:

- Dragged and dropped into the workspace
- Used to build logic flows
- Connected together to create data transformations
- Organized by category (Math, Logic, BACnet, Comparisons, etc.)

**Toolbox Categories:**

- Aggregate Functions
- BACnet
- BasiX Profiles
- BDX
- Comparisons
- Control Points
- Folder
- Logic
- Math
- Priority Arrays
- Status
- Switches and Selects

The Toolbox provides quick access to all available block types for building custom logic.

See [Toolbox Block Reference](toolbox-blocks.md) for complete block library.

---

## 🧩 Logic & Data Processing

### **Block / Blox**
A functional component that can exist in the eXplorer or Toolbox. Blocks are used for:

- Data transformation (math, logic, conversions)
- Connecting to BACnet points
- Creating virtual points
- Implementing custom logic
- Mapping to BasiX profiles

Blocks have inputs, outputs, and properties that define their behavior.

---

## 🔄 View Modes (Active View)

DataLynX provides multiple view modes for working with blocks. Switch between views using the **Active View** dropdown in the **top-right corner** of the screen.

### **Active View Toggle**
The dropdown selector in the top-right corner of the UI that allows switching between different view modes for the selected block. Available views depend on the context (block type, location).

---

### **Flow View**
The default visual, block-based programming interface where users can:

- Drag and drop blocks from the Toolbox
- Connect blocks together with links
- Build data transformation logic
- Monitor live data flow
- See the overall structure of logic flows

**How to access:** Select any block or folder, then ensure "Flow View" is selected in the Active View dropdown.

Similar to programming interfaces in building automation systems like Niagara, Flow View provides an intuitive way to create complex logic without writing code.

---

### **Property View**
A full-page list view showing **all properties** of the selected block in detail:

- All inputs listed (In_1 through In_16 for aggregate blocks)
- Status column showing OK, NULL, or error states
- Checkbox to mark properties as "null" (unused)
- Output value shown at the bottom
- Detailed configuration for each property

**How to access:** Select a block, then choose "Property View" from the Active View dropdown.

**Use Property View when:** You need to see all inputs/outputs at once, debug NULL values, or configure multiple properties.

---

### **Link View**
A table view showing **all connections (links)** into and out of the selected block:

- Source block and property
- Target block and property
- Propagate Units checkbox
- Propagate Range checkbox
- Is Active status

**How to access:** Select a block, then choose "Link View" from the Active View dropdown.

**Use Link View when:** You need to debug data flow, check link properties, or manage multiple connections.

---

### **Mapping Editor**
A specialized view for configuring BasiX mapping schemes:

- Define paths to BACnet points using patterns
- Configure which property to read (typically `out`)
- Set up input properties for fields that need additional data
- Link Hierarchy blocks to BasiX profile fields

**How to access:** Navigate to System → BasiX → mapping_schemes → [Device Type] and select a mapping scheme.

---

### **Properties Pane (Edit Block Panel)**
The side panel that appears on the right when you select a block in **any view**, labeled **"Edit Block"** in the UI:

- Block name editing
- Input and output configuration
- Property values with live updates
- Status and reliability indicators
- Settings specific to the selected block type
- Submit and Close buttons

**Location:** Right side of the screen when a block is selected (in Flow View, Property View, or Link View)

**Key Difference from Property View:**

- **Properties Pane** = Quick access side panel, always available when a block is selected
- **Property View** = Full-page detailed view accessed via the Active View dropdown

The Properties Pane streamlines workflow by keeping block configuration visible while working in any view.

---

## 📊 Data & Configuration

### **Property**
A data element that contains:

- **Value**: The actual data (number, text, boolean, etc.)
- **Units**: Optional unit of measurement (°F, kW, %, etc.)
- **Status**: Current state (OK, Fault, Downstream Fault, etc.)
- **Reliability**: Quality indicator (Reliable, Unreliable, Out of Range, etc.)
- **Timestamp**: When the value was last updated

Properties flow through blocks and links, carrying data and metadata throughout the system.

---

### **Property Flags**
Configuration options that control how a property behaves within the system:

| Flag | Description |
|------|-------------|
| **Transient** | The property can be linked, but its value is not persisted to the Agent Configuration Database. Use for temporary or calculated values that don't need to survive an agent restart. |
| **Summary** | Always displays the property in the Flow View, even if it is not linked. Useful for key values you want visible at a glance. |
| **Readonly** | Prevents the property value from being modified. Use for values that should only be read from their source. |
| **Override Source Units** | Allows you to reinterpret the incoming units on a block without converting the value. |

See **[Property Configuration & Flags](../concepts/units-formatting.md#-5-property-configuration--flags)** for detailed usage.

---

### **Link**
A connection between blocks that:

- Propagates values from one block's output to another block's input
- Can propagate units and status
- Handles change detection
- Forwards faults when configured
- Maintains data integrity

Links are the "wires" that connect blocks together in Flow View.

---

## 🌐 BACnet Terms

### **Hosted Device**
The BACnet device identity that DataLynX presents on the network:

- Has a unique Device ID
- Represents the DataLynX Agent on BACnet/IP
- Can be discovered by other BACnet devices
- Used for network communication and identification

---

### **Discovery**
The process of finding BACnet devices on the network using:

- **Who-Is / I-Am** broadcasts
- Ranged discovery for large networks
- Manual device entry when needed

Discovery populates the device list in the eXplorer.

---

### **Polling**
The automated reading of point values from BACnet devices at configured intervals:

- Ensures data remains current
- Respects stale-time rules
- Manages network bandwidth
- Handles offline devices gracefully

---

## 🧬 BasiX Terms

### **BasiX Profile**
A standardized device template that defines:

- Device type (AHU, VAV, Chiller, etc.)
- Required and optional points
- Expected units and ranges
- Normalization rules

BasiX Profiles ensure consistent data structure for analytics.

---

### **Mapping**
The process of connecting raw BACnet points to BasiX profile fields using:

- **Exact Mapping**: One-to-one connection to a specific point
- **Pattern Mapping**: Wildcards (`*`, `?`) to match multiple devices
- **Regex Mapping**: Advanced patterns for complex naming schemes

---

## 🔒 System Terms

### **Snapshot**
A saved state of the logic processing configuration including:

- All block definitions
- All link connections
- All property values
- Configuration metadata

Snapshots enable quick restoration after changes or failures.

---

### **Data Pump**
The component responsible for:

- Packaging normalized BasiX data
- Sending data securely to BDX
- Handling retries and errors
- Managing communication health

---

## 📝 Quick Reference Table

| Term | Category | Description |
|------|----------|-------------|
| Agent | Core | Running DataLynX instance |
| Supervisor | User Role | Top-level administrator |
| eXplorer | UI | Navigation tree |
| Toolbox | UI | Block selection panel |
| Block/Blox | Logic | Functional component |
| Flow View | UI | Visual programming interface |
| Property Pane | UI | Quick property access panel |
| Link View | UI | Connection management view |
| Properties Pane | UI | Full property details view |
| Property | Data | Value with units, status, reliability |
| Property Flags | Data | Transient, Summary, Readonly, Override Source Units |
| Link | Logic | Connection between blocks |
| Hosted Device | BACnet | DataLynX's BACnet identity |
| BasiX Profile | Data | Standardized device template |
| Snapshot | System | Saved configuration state |

---

## ➡️ Related Documentation

For more details on specific topics:

- **Flow View & Logic**: See [Data Transformation & Logic Flow](../concepts/data-transformation.md)
- **BasiX**: See [BasiX Ontology & Profiles](../concepts/basix-ontology.md)
- **BACnet**: See [BACnet Driver Reference](bacnet-driver.md)
- **UI Navigation**: See [UI Tour](../user-guide/ui-tour.md)
