# DataLynX Documentation

[![Documentation](https://img.shields.io/badge/docs-live-blue)](https://buildinglogix.github.io/DataLynX/)

DataLynX is a high-performance BACnet integration agent designed for the BuildingLogiX ecosystem. It provides reliable BACnet/IP communication, powerful data transformation logic, BasiX ontology mapping, and secure data exchange with the BuildingLogiX Data eXchange (BDX).

**[View Live Documentation →](https://buildinglogix.github.io/DataLynX/)**

---

## 📘 What's Inside

### **Getting Started**
- Quick Start Guide (10-minute setup)
- Installation & Windows Services
- First BACnet Network
- Connecting to BDX

### **Concepts**
- Agent Architecture
- Data Transformation & Logic Flow
- BasiX Ontology & Profiles
- Mapping Modes (Exact, Pattern, Regex)
- Units & Formatting Best Practices

### **User Guide**
- UI Tour
- Explorer Structure
- Hierarchy & Digital Twin
- BACnet Integration Workflow
- BasiX Mapping Workflow

### **Administration**
- Windows Services & Logs
- Backup & Restore
- Resetting the Agent Config
- Security & User Management

### **Reference**
- Terminology & Definitions
- Toolbox Block Reference
- BACnet Driver Reference

### **FAQ**
Common questions, troubleshooting steps, and workflow tips.

---

## 🚀 Quick Links

| Resource | Description |
|----------|-------------|
| [Quick Start](https://buildinglogix.github.io/DataLynX/getting-started/quick-start/) | Get running in 10 minutes |
| [Installation](https://buildinglogix.github.io/DataLynX/getting-started/installation/) | Full setup instructions |
| [FAQ](https://buildinglogix.github.io/DataLynX/faq/) | Troubleshooting & common questions |

---

## 📂 Repository Structure

```
/
├─ docs/
│  ├─ getting-started/    # Installation, Quick Start, BACnet, BDX
│  ├─ concepts/           # Architecture, Data Flow, BasiX, Units
│  ├─ user-guide/         # UI Tour, Explorer, Workflows
│  ├─ admin/              # Services, Backup, Security
│  ├─ reference/          # Terminology, Toolbox, BACnet Driver
│  ├─ img/                # Screenshots and diagrams
│  ├─ assets/             # Logos and styling
│  └─ faq.md
├─ overrides/             # MkDocs theme customizations
├─ mkdocs.yml             # Site configuration
└─ README.md
```

---

## 🛠️ Local Development

To build and preview the documentation locally:

```bash
# Install MkDocs with Material theme
pip install mkdocs-material

# Serve locally with live reload
mkdocs serve

# Build static site
mkdocs build
```

---

## 📄 License

Documentation © BuildingLogiX. All rights reserved.
