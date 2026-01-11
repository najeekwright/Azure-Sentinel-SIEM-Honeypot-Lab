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

## Steps
drag & drop screenshots here or use imgur and reference them using imgsrc

# [cite_start]PROJECT REPORT: Cloud-Native SIEM Implementation & Global Threat Intelligence [cite: 1, 2, 3]

## 1. Project Objective
[cite_start]I initiated this project to bridge the gap between theoretical security concepts and practical "hands-on" engineering[cite: 4, 5]. [cite_start]My goal was to build a functional Security Operations Center (SOC) environment from scratch, specifically focusing on cloud-native logging, data enrichment through external APIs, and real-time visualization of malicious activity[cite: 6].

---

## 2. Phase 0: Environment Provisioning (The Lab Setup)
[cite_start]Before any security analysis could occur, I had to architect the underlying infrastructure[cite: 7, 8]. [cite_start]I utilized Microsoft Azure to deploy a distributed system consisting of a target endpoint and a centralized log management system[cite: 9].

* [cite_start]**Virtual Machine**: I provisioned a Windows 10 Pro instance (Standard B2s) to serve as a high-visibility target[cite: 10].
* [cite_start]**Log Analytics Workspace (LAW)**: I created a dedicated LAW to act as the "sink" for all telemetry, ensuring that logs were stored even if the endpoint was compromised[cite: 13].

![Azure VNet and Resource Group Setup](YOUR_IMAGE_LINK_HERE)
[cite_start]*Figure 1: Virtual network setup in Resource Group RG-SOC[cite: 28, 32].*

---

## 3. Phase 1: Attack Surface Orchestration
[cite_start]To ensure I collected enough data for a meaningful analysis, I intentionally compromised the security posture of the VM[cite: 38, 39].

* [cite_start]**Network Security Group (NSG) Configuration**: I authored a custom inbound rule set to allow all traffic (TCP/Any/Any)[cite: 42].
* [cite_start]**Analyst Rational**: While strictly against enterprise best practices, this "open-door" policy was necessary to attract automated botnets and scanners immediately upon deployment[cite: 43, 44].

![NSG Inbound Security Rules](YOUR_IMAGE_LINK_HERE)
[cite_start]*Figure 2: Configuring the 'AllowAllInbound' rule with Priority 5500[cite: 47, 76].*

---

## 4. Phase 2: Telemetry Engineering & Log Enrichment (ETL)
[cite_start]The most technically intensive part of this project was converting "flat" Windows Event Logs into "context-aware" security data[cite: 93, 94]. [cite_start]Standard Event ID 4625 (Failed Logon) logs provide an IP address but no location data[cite: 95].

### My Data Pipeline:
1. [cite_start]**Extraction**: I utilized a PowerShell script to continuously poll the Security event log[cite: 96, 97].
2. [cite_start]**Transformation**: I integrated the **ipgeolocation.io** API to enrich the raw logs with Latitude, Longitude, and Country metadata[cite: 98].
3. [cite_start]**Loading**: I directed this enriched data into a custom log file located at `C:\ProgramData\failed_rdp.log`[cite: 99, 100].

![Log Analytics Query Results](YOUR_IMAGE_LINK_HERE)
[cite_start]*Figure 3: Inspecting Windows Security Auditing logs for failed logons[cite: 103, 116, 135].*

---

## 5. Phase 3: SIEM Integration & KQL Analytics
[cite_start]After configuring a Data Collection Rule (DCR) to pull my custom logs into Microsoft Sentinel, I had to develop the logic to make the data useful[cite: 137, 138]. [cite_start]I wrote a custom Kusto Query Language (KQL) script to parse the raw strings and aggregate them by geographic location[cite: 139].

### My Detection Logic:
```kql
FAILED_RDP_WITH_GEO_CL 
| summarize EventCount = count() by latitude_CF, longitude_CF, country_CF, label_CF





Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*
