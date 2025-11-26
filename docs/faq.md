# ❓ Frequently Asked Questions (FAQ)

This FAQ covers the most common questions and troubleshooting steps for working with DataLynX, including BACnet configuration, BasiX mapping, services, backups, and UI usage.

---

# 🔧 1. General System Questions

### **Q: What's the difference between the Windows Services and the DataLynX Agent?**
**A:**

**This is a critical distinction:**

**Windows Services** (DataLynX Supervisor Service & DataLynX Web Service):
- Provide the infrastructure for DataLynX to run
- Host the web UI
- Enable remote management capabilities
- Must be running for UI access
- Managed through Windows Services (services.msc)
- Run as processes: `supervisor.exe` and `agent.exe`

**The DataLynX Agent** (controlled from the UI):
- Runs the actual BACnet driver
- Executes data processing and transformation logic
- Manages BuildingLogiX processes
- Controlled from the **Supervisor page** in the UI (NOT from Windows Services)

**Key Point**: You can have the Windows Services running while the DataLynX Agent is stopped. This is why you can log into the UI but not see any BACnet or BasiX sections until you start the DataLynX agent from the Supervisor page.

---

### **Q: The UI won't load. What should I check?**
**A:**
1. Make sure **DataLynX Web Service** is running in Windows Services
2. **Check Windows Firewall** - Ensure an inbound rule exists for the web port (default: 80, or 8050/8020)
3. Verify no other application is using the same port (especially IIS, Apache, or other web servers on port 80)
4. Try accessing from localhost first: `http://localhost`
5. If using a non-standard port, include it in the URL: `http://localhost:8050`
6. Check Task Manager → Details to verify `agent.exe` process is running
7. Check the log file at:
   ```
   C:\ProgramData\DataLynX\agents\default\logs\http.log
   ```

---

### **Q: How do I restart the DataLynX Agent?**
**A:**

**IMPORTANT: The Windows Services and the DataLynX Agent are different things.**

**To restart the DataLynX Agent** (recommended):
1. Log into the DataLynX UI
2. Go to the **Supervisor** page
3. Click **Stop** (wait for DataLynX agent to stop)
4. Click **Start** (wait for DataLynX agent to start)

This restarts the BACnet driver, data processing, and BuildingLogiX processes without restarting the Windows Services.

**To restart the Windows Services** (only if needed for troubleshooting):
1. Open **services.msc**
2. Restart **DataLynX Supervisor Service**
3. Restart **DataLynX Web Service**

**Note**: Restarting the Windows Services will also restart the DataLynX agent if it was running.

---

### **Q: How do I configure the firewall to allow browser access to DataLynX?**
**A:**

DataLynX requires a Windows Firewall rule for the web port.

**Using PowerShell (Administrator):**
```powershell
# For port 80 (default)
New-NetFirewallRule -DisplayName "DataLynX Web UI" -Direction Inbound -Protocol TCP -LocalPort 80 -Action Allow

# OR for alternative ports
New-NetFirewallRule -DisplayName "DataLynX Web UI" -Direction Inbound -Protocol TCP -LocalPort 8050 -Action Allow
```

**Using Windows Firewall GUI:**
1. Open `wf.msc`
2. Inbound Rules → New Rule
3. Port → TCP → Enter your port (80, 8050, or 8020)
4. Allow the connection
5. Apply to all profiles
6. Name: "DataLynX Web UI"

See **Getting Started → Installation** for detailed steps.

---

### **Q: What port does DataLynX use and how do I change it?**
**A:**

DataLynX uses port **80** by default for the Web UI.

**If port 80 is unavailable** (used by IIS, Apache, etc.):
- Recommended alternatives: **8050** or **8020**
- Configure during installation, or manually edit the port in `datalynx.cfg`
- **Important:** Update your firewall rules to match the new port
- Update BDX agent URLs if connecting remotely

---

### **Q: Can I access DataLynX from another computer on the network?**
**A:**

Yes, if:
1. Windows Firewall allows inbound connections on the web port
2. Network firewall/router allows traffic between machines
3. You use the server's IP address: `http://<server-ip>` (or `http://<server-ip>:8050` for non-standard ports)
4. DataLynX Web Service is running

---

### **Q: What are the default login credentials?**
**A:**

DataLynX ships with default login credentials provided by BuildingLogiX during installation/setup.

After first login:
1. You will land on the **Supervisor** view
2. If the agent is not running, click **Start**
3. Once started, the full DataLynX Explorer tree will appear

**IMPORTANT**: Change the default password immediately after first login for security.

See **[Installation & Services](getting-started/installation.md#-4-accessing-the-web-ui)** for details.

---

### **Q: I logged in but don't see the BACnet or BasiX sections. What's wrong?**
**A:**

The agent is probably not running. After login, you land on the **Supervisor** page.

**Remember**: The Windows Services can be running, but the agent can be stopped. They are different things.

**To start the agent:**
1. Check the status on the Supervisor page - it should say "Stopped" or "Not Running"
2. Look for the **Start** button on the Supervisor page
3. Click **Start**
4. Wait for the agent to initialize (BACnet driver, data processing, BuildingLogiX processes)
5. The full DataLynX Explorer tree will appear with all sections:
   - BACnet networks and devices
   - BasiX profiles
   - System configuration
   - User management

---

# 🌐 2. BACnet Questions

### **Q: Devices are not showing up in discovery. What should I check?**
**A:**
- Ensure BACnet driver is enabled  
- Check UDP port (default 47808)  
- Ensure firewall allows UDP broadcasts  
- Verify correct network interface is selected  
- Confirm network number matches your system  
- Validate router table entries if using routing  

---

### **Q: Why do some points show NaN or Fault in the UI?**
**A:**
- Device may be offline (check Health Monitoring)  
- Units may be incompatible (if unit propagation enabled)  
- Link or mapping may be incorrect  
- Value may be stale—check poll interval & stale timeout  
- Block in Flow View may be producing invalid output  

---

### **Q: What does “stale” mean in the BACnet context?**
A point becomes *stale* when it has not been updated within the configured **Stale Time** interval.  
Stale points are marked unreliable.

---

### **Q: Why does the router table look empty?**
If using routers, verify:

- Router devices are online  
- Router broadcasts are allowed  
- Remote networks respond to discovery  
- Foreign Device registration is not required  

---

# 🧩 3. Data Transformation & Logic Questions

### **Q: My math block is producing NaN. Why?**
**Possible reasons:**
- Input units are incompatible (°F vs inWC, etc.)  
- Divide-by-zero error  
- Unsupported or missing values from upstream  
- Block misconfiguration  

Use **Watch** to inspect inputs and outputs.

---

### **Q: How do I debug a logic block?**
Use the **Watch** feature:
1. Right-click any block output or property  
2. Select **Add to Watch**  
3. Watch window will display live values, status, and units  

---

### **Q: Why is my value not converting units?**
Check that:
- The correct block type is used (e.g., Convert block)  
- Units are defined on the source value  
- “Propagate Units” is enabled in the link  

---

# 🧬 4. BasiX Questions

### **Q: Why can't I publish my BasiX device to BDX?**
Because required fields are missing or have incorrect/missing units.
Open the BasiX device and check:

- Required fields (marked with ⚠️ if unmapped)
- **Correct units on all mapped points**
- Valid mappings

**Critical**: All points mapped to BasiX profiles must have proper units. Missing or incorrect units will prevent device publishing.  

---

### **Q: How do I map multiple similar devices efficiently?**
Use **Pattern Mapping** or **Regex Mapping** to match repeated VAVs, HPUs, or FCUs.

---

### **Q: My mappings look correct but the wrong points map. What gives?**
Most likely your pattern or regex needs refinement.
Use "Mapping Preview" to validate which points match the rule.

---

### **Q: What are dimensionless units and how do I handle them?**
Dimensionless (or "unitless") values are valid in BACnet. They're commonly used for:
- Ratios (0–1 fractions)
- Counts (number of starts)
- Vendor-specific scales

**For DataLynX**: Dimensionless values must have units assigned when mapping to BasiX profile fields that expect physical measurements (like temperature, airflow, or pressure).

Use the **Override Source Units** flag or conversion blocks to assign proper units.

See **[Units & Formatting](../concepts/units-formatting.md)** for details.

---

### **Q: Why is my temperature math producing wrong results?**
You may be mixing absolute temperature (°F) with delta temperature (ΔF).

**Key rules**:
- Use **delta temperature (ΔF)** for temperature differences
- Do NOT multiply or divide absolute temperatures
- Subtract/Mean blocks work with absolute temps
- Add blocks need: absolute temp + delta temp

See **[Units & Formatting → Temperature Units](../concepts/units-formatting.md#-4-temperature-units-and-delta-temperature)** for detailed examples.

---

### **Q: How do I fix a BACnet point that has wrong units?**
Use the **Override Source Units** flag:
1. Select the BACnet point block
2. Check **Override Source Units**
3. Select the correct units

**Important**: This reinterprets the value with new units—it does NOT convert the value.

For actual conversion (e.g., Pa → inWC), use a Convert block.

---

# ☁️ 5. BDX Communication Questions

### **Q: Agent shows “offline” in BDX. Why?**
Check:
- BDX URL configured correctly  
- `/agent/bdxAgent` endpoint is used  
- Password is correct  
- Web Service is running  
- Outbound traffic allowed on required ports  

---

### **Q: Data isn’t showing in BDX. What should I check?**
- Ensure BasiX devices are published  
- Check BDX logs:  
  ```
  C:\ProgramData\DataLynX\agents\default\logs\bdx.log
  ```
- Verify data pump is enabled  
- Confirm values are updating in the data processing layer  

---

# 🔐 6. Security & User Questions

### **Q: I forgot my password. How do I reset it?**
Log in as a **Supervisor**, then:

```
System → Users → Select User → Change Password
```

If all Supervisor credentials are lost, reset the agent config (see **Reset Config**).

---

### **Q: What happens if I delete a user?**
The user is permanently removed.  
For temporary access removal, disable the account instead.

---

# 💾 7. Backup & Reset Questions

### **Q: Does deleting datalynx.cfg reset the system?**
Yes, **but only if the Supervisor Service is stopped first**.  
Otherwise, the agent will recreate the file from its in-memory state.

---

### **Q: How often does DataLynX back up automatically?**
Backups occur when:
- Configuration changes
- Logic processing snapshot is created
- A restart triggers state persistence  

---

### **Q: Can I restore a config from another machine?**
Yes, as long as DataLynX versions match.  
Copy:

```
datalynx.cfg
snapshots/
backups/
```

onto the new machine’s agent directory.

---

# 🛠️ 8. Advanced Questions

### **Q: Does DataLynX support BACnet MSTP?**
Yes, **via routed networks** (Remote Stations).  
Requires a BACnet router.

---

### **Q: Does DataLynX support BBMD?**
Yes.  
Use “Foreign Device” mode to register with a remote BBMD.

---

### **Q: Can I import/export mappings?**
Yes — DataLynX supports saving and loading mapping structures through configuration files, though UI tools for import/export vary by version.

---

# 📝 Summary

This FAQ covers:

- General usage  
- BACnet setup  
- Data transformation logic  
- BasiX mappings  
- BDX communication  
- Backups & resets  
- User and security concerns  

If your question isn’t answered here, consider checking:

- Log files (`C:\ProgramData\DataLynX\agents\default\logs\`)  
- The User Guide pages  
- The Reference documentation  

Or reach out to BuildingLogiX support.

