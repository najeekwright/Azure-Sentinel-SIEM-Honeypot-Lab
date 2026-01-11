# Azure Sentinel SIEM Honeypot 

## Objective


The objective of this project was to engineer a high-interaction Azure Sentinel SIEM Honeypot to monitor and analyze real-time brute-force attacks. By deploying a deliberately exposed Windows endpoint, the goal was to observe adversarial behavior, track global attack patterns, and gain hands-on experience in log ingestion, data enrichment, and security visualization within a cloud-native SIEM environment.

### Skills Learned


- Microsoft Sentinel SIEM configuration and log monitoring.
- KQL query development for security data analysis.
- PowerShell scripting for log extraction and API enrichment.
- Azure Network Security Group and firewall management.
- Attacker behavior analysis including brute-force and credential stuffing.

### Tools Used


- Microsoft Sentinel SIEM for centralized security monitoring and threat visualization.
- Microsoft Azure for cloud infrastructure provisioning and virtual machine hosting.
- PowerShell with Geolocation API integration for custom log enrichment and automation.
- Kusto Query Language KQL for data parsing and building analytical workbooks


# PROJECT REPORT: Cloud-Native SIEM Implementation & Global Threat Intelligence 

## 1. Project Objective
I initiated this project to bridge the gap between theoretical security concepts and practical "hands-on" engineering. My goal was to build a functional Security Operations Center (SOC) environment from scratch, specifically focusing on cloud-native logging, data enrichment through external APIs, and real-time visualization of malicious activity.

---

## 2. Phase 0: Environment Provisioning (The Lab Setup)
Before any security analysis could occur, I had to architect the underlying infrastructure. I utilized Microsoft Azure to deploy a distributed system consisting of a target endpoint and a centralized log management system[cite: 9].

**Virtual Machine**: I provisioned a Windows 10 Pro instance (Standard B2s) to serve as a high-visibility target.
**Log Analytics Workspace (LAW)**: I created a dedicated LAW to act as the "sink" for all telemetry, ensuring that logs were stored even if the endpoint was compromised.

(Azure VNet and Resource Group Setup)
Figure 1: Virtual network setup in Resource Group RG <img width="1381" height="911" alt="MS Virtual Network Creation" src="https://github.com/user-attachments/assets/13503a96-b147-4db2-9a5c-20dcaf511ed9" />


---

## 3. Phase 1: Attack Surface Orchestration
To ensure I collected enough data for a meaningful analysis, I intentionally compromised the security posture of the VM.

* **Network Security Group (NSG) Configuration**: I authored a custom inbound rule set to allow all traffic (TCP/Any/Any).
* **Analyst Rational**: While strictly against enterprise best practices, this "open-door" policy was necessary to attract automated botnets and scanners immediately upon deployment.

(NSG Inbound Security Rules) Figure 2: Configuration of Inbound Rules <img width="1600" height="896" alt="Network Security Groups" src="https://github.com/user-attachments/assets/1bea2172-7ff0-4939-8daf-3d1c4235a373" />
I Configured the 'AllowAllInbound' rule with Priority 5500.*

---

## 4. Phase 2: Telemetry Engineering & Log Enrichment (ETL)
The most technically intensive part of this project was converting "flat" Windows Event Logs into "context-aware" security data. Standard Event ID 4625 (Failed Logon) logs provide an IP address but no location data.

### My Data Pipeline:
1. **Extraction**: I utilized a PowerShell script to continuously poll the Security event log.
2. **Transformation**: I integrated the **ipgeolocation.io** API to enrich the raw logs with Latitude, Longitude, and Country metadata.
3. **Loading**: I directed this enriched data into a custom log file located at `C:\ProgramData\failed_rdp.log`.

*Figure 3: Inspecting Windows Security Auditing logs for failed logons after querey configuration <img width="1600" height="873" alt="KQL Querey" src="https://github.com/user-attachments/assets/dc7b974c-57a5-48c5-8cc3-94c378d22263" />
Note

---

## 5. Phase 3: SIEM Integration & KQL Analytics
After configuring a Data Collection Rule (DCR) to pull my custom logs into Microsoft Sentinel, I had to develop the logic to make the data useful. I wrote a custom Kusto Query Language (KQL) script to parse the raw strings and aggregate them by geographic location.

*Figure 3: Inspecting Windows Security Auditing logs for failed logons after querey configuration <img width="1600" height="873" alt="KQL Querey" src="https://github.com/user-attachments/assets/dc7b974c-57a5-48c5-8cc3-94c378d22263" />


## 6. Final Observations & Live Threat Map
The project culminated in a live, interactive heatmap that visualized the "background noise" of the internet. This dashboard transformed raw, enriched logs into a spatial representation of global threat activity. [cite: 142-143]

(Figure 4: Attack Map) <img width="1600" height="834" alt="heat map" src="https://github.com/user-attachments/assets/9757bf05-c994-490f-a505-ac29098c7297" />

*Figure 3: Global heatmap of live brute-force attacks visualized in Microsoft Sentinel.*

### My Operational Findings:
* **Initial Discovery**: My VM was identified and probed by automated scanners within minutes of the firewall change, confirming the high speed at which attackers scan public cloud IP ranges.
* **Geographic Density**: According to my SIEM dashboard, the highest volume of attacks originated from **Jordanow, Poland** with **26.6K** events recorded, followed by significant activity from **Tilburg, Netherlands** and **Ranchos, Argentina**.
* [cite_start]**Threat Actor Behavior**: I observed a recurring pattern of automated credential stuffing targeting the **Administrator** account, proving that default account names remain the primary targets for brute-force exploits in cloud environments.

