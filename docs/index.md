# ![DataLynX Logo](assets/datalynx_logo.svg){ width="50" } **DataLynX Documentation**

**DataLynX** is a modern, high-performance BACnet agent and integration framework designed for the BuildingLogiX ecosystem.
Built on Python, it provides fast, reliable BACnet/IP communication, powerful data transformation and logic capabilities, and seamless integration with the BuildingLogiX Data eXchange (BDX).

---

## What is DataLynX?

DataLynX acts as the **bridge between on-premises building automation systems (BAS)** and the **BDX (BuildingLogiX Data eXchange)** analytics platform.

Built entirely in **Python**, DataLynX leverages modern libraries and frameworks to deliver enterprise-grade performance, extensibility, and reliability.

![DataLynX Flow View](img/Flow View.PNG)

<div style="margin-top: 1em;">
  <a href="https://www.bacnetinternational.org/" target="_blank" rel="noopener" title="BACnet International">
    <img src="https://upload.wikimedia.org/wikipedia/commons/9/9a/BACnet-Logo-New.gif" alt="BACnet Logo" style="height: 50px;">
  </a>
</div>

It provides:

- **BACnet/IP communication**
  Full client support, router/FDT handling, polling, discovery, and device health monitoring.

- **Data Transformation & Logic Flow**
  Visual wiresheet interface (Flow View) for transforming BAS values, scaling units, creating virtual points, and performing custom logic.

- **BasiX Ontology & Profiles**
  Standardized device and point modeling to normalize diverse BAS data into a common structure.

- **User Interface**  
  A clean web UI for configuring networks, discovering devices, managing BasiX mappings, monitoring live values, and navigating logic flows.

- **Secure communication to BDX**
  Agent registration, data pumping, and remote commands/status handling.

- **Windows Services**  
  The DataLynX installation includes system services for Agent execution, background operations, and UI hosting.

---

## Documentation Overview

This site contains the complete documentation for DataLynX, including:

### **➡ Getting Started**

- Installation & required Windows services  
- Creating your first BACnet network  
- Connecting the Agent to BDX  
- Verifying communication & first-run checks

### **➡ Concepts & Architecture**

- Agent architecture
- Data transformation and logic flow
- BasiX ontology and profiles
- Mapping strategies (Exact, Pattern, Regex)
- **Units, formatting, and best practices** ⚠️ (Critical for BasiX mapping)

### **➡ User Guide**

- Full UI tour with screenshots  
- BACnet discovery, device views, and point properties  
- BasiX mapping workflow  
- Using the Flow/Link view for logic visualization

### **➡ Administration**

- Managing users and permissions  
- Understanding Windows services  
- Logs and diagnostics  
- Backups & restoring  
- Resetting the Agent configuration safely

### **➡ Reference**

- BACnet driver configuration details
- Polling policies & device health rules
- Data transformation reference
- BasiX device/point modeling guidelines

### **➡ FAQ**
Common issues, troubleshooting steps, and quick answers.

---

## System Requirements

- Windows 10, Windows Server 2016+  
- DataLynX installer (.msi)  
- Administrative privileges for service installation  
- Network visibility to BACnet/IP devices  
- Network access to the web UI port (80, 8050, or 8020)  
- Optional: Connectivity to a BDX instance

---

## How DataLynX Fits into the BDX Ecosystem

DataLynX is designed to work seamlessly with:

- **[BDX (BuildingLogiX Data eXchange)](https://buildinglogix.github.io/BDX/)**
  Analytics platform that receives normalized data from Agents.

- **Supervisor UI**
  Remote visibility into Agents, status, and device mappings.

- **BDXpy**
  Python SDK for interacting programmatically with BDX data.

DataLynX handles on-prem BAS communication, computation, and normalization so BDX can focus on long-term storage, analytics, and visualization.

---

## Getting Started

➡ Start with: **Installation & Services**  
After installing DataLynX, you will:

1. Open the Web UI  
2. Configure the hosted BACnet device  
3. Add your first BACnet network  
4. Discover devices  
5. Map points using BasiX  
6. Connect the Agent to BDX

Use the navigation on the left to follow the recommended sequence.

---

## Licensing, Support, & Contact

**DataLynX** is proprietary software developed and owned by **BuildingLogiX**.

For questions, licensing, technical support, or feedback:

📧 **Email:** [technical.support@buildinglogix.net](mailto:technical.support@buildinglogix.net)

---

## License & Copyright

- **Software:** DataLynX © 2025 BuildingLogiX. All rights reserved.
- **Documentation:** © 2025 BuildingLogiX. All rights reserved.

DataLynX is licensed software. Unauthorized copying, distribution, modification, or use of this software without a valid license from BuildingLogiX is strictly prohibited.


