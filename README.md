# 🕵️‍♂️ End-to-End Splunk SOC Investigation: Web Server Attack Analysis

**Author:** Pranit Kalambate  
**Role:** Cybersecurity / SOC Analyst  
**Environment:** Splunk Enterprise, BOTS v1 Dataset (IIS & Network Stream Logs)

---

## 🎯 Project Objective
The goal of this project is to simulate a real-world Security Operations Center (SOC) investigation. By analyzing raw web server and network logs using Splunk (SPL), this project traces a complete attack lifecycle—from initial reconnaissance and vulnerability scanning to data exfiltration.

## 🛠️ Investigation Lifecycle & Key Findings

This investigation was structured into 5 distinct phases (ticket-based workflow). Click on the tickets below to view the detailed investigation reports and visual evidence:

* 📄 **[Phase 1: Web Reconnaissance & Intent Analysis](Splunk_SIEM_Investigations/Ticket_SOC-2026-001.md)**
  * Identified the attacker IP (`40.80.148.42`) generating 3,531 malicious requests.
  * Discovered the primary target: Joomla CMS (`/joomla/index.php/component/search/`).
* 📄 **[Phase 2: Data Exfiltration & Network Pivoting](Splunk_SIEM_Investigations/Ticket_SOC-2026-002.md)**
  * Pivoted from application logs to network wire data (`stream:http`) to bypass missing IIS byte logs.
  * Confirmed the successful exfiltration of **18.94 MB** of data.
* 📄 **[Phase 3: Attack Timeline & Duration Analysis](Splunk_SIEM_Investigations/Ticket_SOC-2026-003.md)**
  * Grouped 20,967 individual requests into a single session by bypassing memory limits.
  * Calculated the exact campaign duration: **45.70 minutes**.
* 📄 **[Phase 4: Threat Intelligence & Geolocation](Splunk_SIEM_Investigations/Ticket_SOC-2026-004.md)**
  * Enriched the raw IP data to trace the attacker's origin to **Washington, Virginia, US**.
* 📄 **[Phase 5: Attack Visualization for Executive Reporting](Splunk_SIEM_Investigations/Ticket_SOC-2026-005.md)**
  * Plotted a time-series area chart revealing the peak attack velocity at **03:07 AM** (928 hits/minute).

---

## 🧠 Key Analyst Takeaways
1. **Never blindly trust application logs:** As demonstrated in Phase 2, when IIS logs failed to provide data size metrics, pivoting to Network Stream logs was essential to uncover the exfiltration.
2. **Attention to Syntax is Critical:** A single typo in an HTTP status code (`400` vs `404`) can drastically alter the visual timeline and misguide the incident response team.
3. **Translating Tech to Business:** Used `eval` functions to convert raw bytes into readable Megabytes (MB) and utilized `timechart` to convert raw event counts into executive-friendly visual graphs.

---
*Note: This project is part of a hands-on SIEM training exercise to demonstrate proficiency in log analysis, threat hunting, and incident reporting.*
