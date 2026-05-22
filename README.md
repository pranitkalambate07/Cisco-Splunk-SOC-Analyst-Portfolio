# 🛡️ Comprehensive Splunk SOC Analyst Portfolio: Multi-Domain Threat Hunting

**Author:** Pranit Kalambate  
**Role:** Cybersecurity / SOC Analyst  
**Environment:** Splunk Enterprise  
**Datasets Analyzed:** BOTS v1 (Web/Network), BOTS v2 (Endpoint/Sysmon), BOTS v3 (Azure Cloud)  
**Training Alignment:** Cisco Networking Academy - *Data and Tools for Defense Analysts*

---

## 🎯 Project Objective
The goal of this portfolio is to document the practical, hands-on lab exercises completed as part of the **Cisco Networking Academy "Data and Tools for Defense Analysts"** course (Cybersecurity Defense Analyst Career Path). By analyzing raw telemetry across different enterprise layers (Web, Endpoint, and Cloud) using Splunk (SPL), this project simulates a real-world Security Operations Center (SOC) investigation and traces complex attack lifecycles.

---

## 📂 Investigation Lifecycle & Key Findings

This portfolio is divided into 3 distinct domains. Click on the tickets below to view the detailed investigation reports, SPL queries, and visual evidence:

### 🌐 Domain 1: Web Server Attack Analysis (BOTS v1)
*Folder: `Splunk_SIEM_Investigations`*

* 📄 **[Ticket 1: Web Reconnaissance & Intent Analysis](Splunk_SIEM_Investigations/Ticket_SOC-2026-001.md)**
  * Identified the attacker IP (`40.80.148.42`) generating 3,531 malicious requests.
  * Discovered the primary target: Joomla CMS (`/joomla/index.php/component/search/`).
* 📄 **[Ticket 2: Data Exfiltration & Network Pivoting](Splunk_SIEM_Investigations/Ticket_SOC-2026-002.md)**
  * Pivoted from application logs to network wire data (`stream:http`) to bypass missing IIS byte logs.
  * Confirmed the successful exfiltration of **18.94 MB** of data.
* 📄 **[Ticket 3: Attack Timeline & Duration Analysis](Splunk_SIEM_Investigations/Ticket_SOC-2026-003.md)**
  * Grouped 20,967 individual requests into a single session by bypassing memory limits.
  * Calculated the exact campaign duration: **45.70 minutes**.
* 📄 **[Ticket 4: Threat Intelligence & Geolocation](Splunk_SIEM_Investigations/Ticket_SOC-2026-004.md)**
  * Enriched the raw IP data to trace the attacker's origin to **Washington, Virginia, US**.
* 📄 **[Ticket 5: Attack Visualization for Executive Reporting](Splunk_SIEM_Investigations/Ticket_SOC-2026-005.md)**
  * Plotted a time-series area chart revealing the peak attack velocity at **03:07 AM** (928 hits/minute).

---

### 💻 Domain 2: Advanced Endpoint Threat Hunting (BOTS v2)
*Folder: `Advanced_Threat_Hunting`*

* 📄 **[Ticket 1: Ransomware Activity Detected (Shadow Copy Deletion)](Advanced_Threat_Hunting/Ticket_SOC-ADV-001.md)**
  * Detected living-off-the-land (LotL) binary abuse via execution of the `vssadmin.exe delete shadows` command.
  * Confirmed ransomware pre-encryption tactics on the compromised host `we8105desk.waynecorp.inc` (Bob Smith).
* 📄 **[Ticket 2: Suspicious Encoded PowerShell Execution](Advanced_Threat_Hunting/Ticket_SOC-ADV-002.md)**
  * Detected obfuscated PowerShell execution (`-WindowStyle Hidden`, `-enc`) designed to bypass legacy AV signatures.
  * Traced the Base64 payload execution back to a compromised service account (`FROTHLY\service3`) on host `venus.frothly.local`.
* 📄 **[Ticket 3: LSASS Credential Dumping & Lateral Movement](Advanced_Threat_Hunting/Ticket_SOC-ADV-003.md)**
  * Identified unauthorized memory access requests to the Local Security Authority Subsystem Service (`lsass.exe`) for credential theft.
  * Utilized raw log hunting to uncover lateral movement via Remote Desktop Protocol (EventCode 4624 / Logon Type 10).

---

### ☁️ Domain 3: Cloud Identity Threat Detection (BOTS v3)
*Folder: `Azure_Cloud_Security`*

* 📄 **[Ticket 1: Azure AD Password Spraying Detection](Azure_Cloud_Security/Ticket_AZURE-001.md)**
  * Analyzed Azure Active Directory sign-in telemetry (`ms:aad:signin`) to detect horizontal brute-force attacks.
  * Identified a specific adversary IP (`51.18.191.61`) generating high-volume `50126` (Invalid Credentials) error codes across multiple internal `userPrincipalName` accounts.

---

## 🧠 Key Analyst Takeaways
1. **Never blindly trust single log sources:** As demonstrated in Domain 1, when IIS logs failed to provide data size metrics, pivoting to Network Stream logs was essential to uncover the exact exfiltration size.
2. **Bypassing Field Parsing Failures:** When strict `sourcetype` parsing fails or datasets are heavily structured (as seen in BOTS v2 and v3), executing raw text searches (e.g., `index="botsv3" "50126"`) is a critical pivot technique to uncover hidden telemetry.
3. **Cross-Domain Threat Tracking:** Modern adversaries do not stay in one place. Tracking a threat actor from a web-facing server down to an endpoint memory process (LSASS) and up to the cloud (Azure AD) requires a unified SIEM mindset and an understanding of diverse log architectures.
4. **Translating Tech to Business:** Used `eval` functions to convert raw bytes into readable Megabytes (MB) and utilized `timechart` to convert raw event counts into executive-friendly visual graphs.

---
*Note: This portfolio is a continuous documentation of hands-on SIEM training aligned with Cisco Networking Academy standards, demonstrating proficiency in multi-domain log analysis, proactive threat hunting, and professional incident reporting.*
