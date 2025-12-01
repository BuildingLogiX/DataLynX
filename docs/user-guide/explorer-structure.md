# ![DataLynX Logo](../assets/datalynx_logo.svg){ width="50" } eXplorer Structure & Navigation

The **eXplorer** is the primary navigation tree in DataLynX, located on the left side of the UI. It provides hierarchical access to all configuration, devices, logic blocks, and system tools.

Understanding the eXplorer structure is essential for efficient navigation and workflow management in DataLynX.

---

## 📊 eXplorer Tree Overview

When you log into DataLynX with the agent running, you'll see the following top-level structure:

```
DataLynX (root)
├── System
├── Connections
├── Hierarchy
├── Supervisor
├── Manage Users
└── Manage Licenses
```

![eXplorer Top Node](../img/DataLynX - eXplorer Top Node.PNG)

Each of these nodes serves a specific purpose in the DataLynX ecosystem.

---

## 🏗️ Top-Level Nodes Explained

### **DataLynX (Root Node)**

The root node represents the DataLynX agent instance. Clicking on it provides:

- System information (OS, host ID, version)
- Agent information (name, data directory, state)
- Agent control (Start/Stop buttons if accessed via Supervisor)

---

### **System**

The **System** node contains configuration and management tools for DataLynX itself.

**Structure:**
```
System
├── BasiX
│   └── mapping_schemes
│       ├── VAV
│       ├── AHU
│       ├── FanCoil
│       ├── Chiller
│       ├── CoolingTower
│       ├── ResourceMeter
│       └── [Other device types...]
└── backup_service
```

**What's inside:**

- **BasiX/mapping_schemes** - Contains BasiX profile templates and mapping configurations for different device types
- **backup_service** - Configuration for automated backup scheduling and snapshot management

**When to use:**

- Configuring BasiX device mapping schemes (VAV, AHU, Chiller, etc.)
- Setting up automated backup schedules
- Managing system-level BasiX configuration

**Related documentation:**

- [BasiX Ontology & Profiles](../concepts/basix-ontology.md)
- [BasiX Mapping Workflow](basix-mapping-workflow.md)

---

### **Connections**

The **Connections** node contains all communication driver configuration for external systems.

**Structure:**
```
Connections
├── BDX
│   └── [BDX connection settings]
└── BACnet
    ├── configuration
    │   ├── hosted_device
    │   ├── router_table
    │   └── bacnet_networks
    │       └── BACnetIPNetwork
    │           ├── configuration
    │           ├── polling_service
    │           └── health_monitoring_configuration
    └── devices
        ├── [Discovered device: Honeywell]
        ├── [Discovered device: ALC]
        └── [Other vendors/devices...]
```

**What's inside:**

- **BDX** - Connection settings for sending data to the BuildingLogiX Data eXchange platform
- **BACnet** - BACnet driver configuration and discovered devices
  - **configuration** - Hosted device, router table, network settings
  - **bacnet_networks** - Individual BACnet/IP network configurations
  - **devices** - All discovered BACnet devices organized by vendor or name

**When to use:**

- Setting up your first BACnet network
- Discovering and managing BACnet devices
- Configuring the hosted device identity
- Connecting to BDX
- Viewing and managing device points

**Related documentation:**

- [First BACnet Network](../getting-started/first-bacnet-network.md)
- [Connect to BDX](../getting-started/connect-to-bdx.md)
- [BACnet Driver Reference](../reference/bacnet-driver.md)

---

### **Hierarchy**

The **Hierarchy** node is your digital twin workspace—a flexible area where you can mirror physical building infrastructure, organize custom logic and calculations, and build BasiX profiles for BDX.

**Purpose:**

- Build BasiX device profiles that get sent to the BDX agent
- Create custom calculations and logic (efficiency metrics, aggregations, fault detection)
- Organize blocks by physical layout (campuses, buildings, zones) OR by function (calculations, profiles, virtual meters)
- Build data transformation flows in whatever structure makes sense for your site
- Maintain separation between physical BACnet devices (under Connections) and logical processing (under Hierarchy)

**Example Structure:**
```
Hierarchy
├── Campus_Main
│   ├── Building_A
│   │   ├── AHUs
│   │   │   ├── AHU-1 [logic blocks]
│   │   │   └── AHU-2 [logic blocks]
│   │   ├── VAVs
│   │   └── Lighting
│   └── Building_B
│       ├── Chiller_Plant
│       └── Boiler_Plant
├── Water_System_Devices
│   ├── Pumps
│   └── Cooling_Towers
└── Central_Plant
    ├── Chillers
    └── Boilers
```

**When to use:**

- Creating BasiX device profiles to send to BDX
- Building custom calculations (efficiency, aggregations, virtual meters)
- Organizing logic by physical layout, equipment type, or analytical function
- Developing virtual points that combine multiple physical points
- Creating fault detection and monitoring logic
- Preparing all data processing that feeds the BDX agent

**Key Concept:**
The Hierarchy is where **all your custom work lives**:

- BasiX profiles for BDX are built here
- Custom calculations and logic are organized here
- Flow View workspace operates within Hierarchy folders
- This is your canvas—structure it however makes sense for your needs

**Related documentation:**

- [Hierarchy & Digital Twin](hierarchy-digital-twin.md) (detailed guide)
- [Data Transformation & Logic Flow](../concepts/data-transformation.md)

---

### **Supervisor**

The **Supervisor** node provides administrative control and monitoring of the DataLynX agent.

**What's inside:**

- Agent status display (Running/Stopped)
- Start/Stop agent controls
- System information
- Agent configuration overview

**When to use:**

- Starting or stopping the DataLynX agent
- Checking agent status
- Viewing system information
- Administrative oversight

**Important:**

- This is the **landing page** when you first log in
- If the agent is not running, you must click **Start** here to see the full eXplorer tree
- The agent can be stopped while Windows Services continue running

**Related documentation:**

- [Installation & Services](../getting-started/installation.md)
- [Terminology: Supervisor](../reference/terminology.md#supervisor)

---

### **Manage Users**

User account management interface.

**What's inside:**

- User list with roles and permissions
- User creation and deletion
- Password management
- Role assignment (Supervisor, Operator, Viewer, etc.)

**When to use:**

- Creating new user accounts
- Assigning roles and permissions
- Resetting passwords
- Managing authentication settings

**Related documentation:**

- [Security & User Management](../admin/security-users.md)

---

### **Manage Licenses**

License and activation management.

**What's inside:**

- Current license status
- License key entry
- Feature entitlements
- Expiration information

**When to use:**

- Activating DataLynX with a license key
- Checking license status
- Viewing enabled features

---

## 🔍 Navigation Patterns

### **Breadcrumb Navigation**

When you click on an eXplorer node, the top of the screen shows a breadcrumb path:

```
/Hierarchy/Campus_Main/Building_A/AHUs
```

or

```
/Connections/BACnet/devices/Honeywell_Controller
```

This shows your current location in the tree and helps you understand context when working in Flow View or configuration pages.

---

### **Flow View Context**

When you select a node in the eXplorer:

- The **center workspace** shows the Flow View for that node (if it contains blocks)
- The **right panel** shows the Toolbox (with block categories)
- The **Edit Block** panel appears when you select a block

**Example:**

- Click on `Hierarchy → AHUs → AHU-1`
- Flow View opens showing all logic blocks for AHU-1
- Toolbox appears on the right with available block types
- You can drag blocks, create links, and build logic flows

---

### **Switching Between Views**

The top-right corner shows the **Active View** toggle:

- **Flow View** - Visual block-based logic workspace (most common)
- **Mapping Editor** - Used when working with BasiX mapping schemes
- **Other views** - Context-dependent based on the selected node

---

## 📁 Organizing Your Workspace

### **Best Practices for Hierarchy Organization**

1. **Mirror Physical Layout:**
   ```
   Hierarchy
   ├── Campus_North
   ├── Campus_South
   └── Remote_Sites
   ```

2. **Or Organize by System Type:**
   ```
   Hierarchy
   ├── HVAC_Equipment
   ├── Lighting_Systems
   ├── Electrical_Monitoring
   └── Water_Systems
   ```

3. **Or Combine Both:**
   ```
   Hierarchy
   ├── Building_1
   │   ├── HVAC
   │   └── Lighting
   └── Building_2
       ├── HVAC
       └── Lighting
   ```

### **Creating Folders**

To create a new folder in Hierarchy:
1. Right-click on Hierarchy (or a subfolder)
2. Select **New Folder** (or use Toolbox → Folder)
3. Name the folder
4. Drag blocks into the folder or create new logic inside it

---

## 🗺️ Quick Reference: Where to Find Things

| Task | Navigate To |
|------|-------------|
| Configure BACnet network | Connections → BACnet → configuration → bacnet_networks |
| Discover BACnet devices | Connections → BACnet → devices |
| View device points | Connections → BACnet → devices → [Device Name] |
| Configure hosted device | Connections → BACnet → configuration → hosted_device |
| Connect to BDX | Connections → BDX |
| Create custom logic blocks | Hierarchy → [Your folders] |
| View existing BasiX mappings | System → BasiX → mapping_schemes |
| Set up backups | System → backup_service |
| Start/stop agent | Supervisor |
| Manage users | Manage Users |
| Check license | Manage Licenses |

---

## 💡 Key Concepts Summary

1. **Physical vs Logical Separation:**
   - **Connections/BACnet/devices** = Physical BACnet devices from the network
   - **Hierarchy** = Logical organization and custom data processing

2. **Flow View is Everywhere:**
   - Most eXplorer nodes can be opened in Flow View
   - This is where you visualize and edit logic blocks

3. **Breadcrumbs Show Context:**
   - Always check the breadcrumb path to know where you are
   - Helps when navigating complex nested structures

4. **Hierarchy is Your Canvas:**
   - This is where you build the digital twin
   - Organize it to match how you think about your building systems

---

## ➡️ Next Steps

Now that you understand the eXplorer structure:

- **Create your digital twin** - See [Hierarchy & Digital Twin](hierarchy-digital-twin.md)
- **Learn about blocks** - See [Toolbox Reference](../reference/toolbox-blocks.md)
- **Build logic flows** - See [Data Transformation & Logic Flow](../concepts/data-transformation.md)
- **Map to BasiX** - See [BasiX Mapping Workflow](basix-mapping-workflow.md)
