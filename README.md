# 🕵️‍♂️ End-to-End Splunk SOC Investigation: Web Server Attack Analysis

**Author:** Pranit Kalambate  
**Role:** Cybersecurity / SOC Analyst  
**Environment:** Splunk Enterprise, BOTS v1 Dataset (IIS & Network Stream Logs)

---

## 🎯 Project Objective
The goal of this project is to simulate a real-world Security Operations Center (SOC) investigation. By analyzing raw web server and network logs using Splunk (SPL), this project traces a complete attack lifecycle—from initial reconnaissance and vulnerability scanning to data exfiltration.

## 🛠️ Investigation Lifecycle & Key Findings

This investigation was structured into 5 distinct phases (ticket-based workflow):

### Phase 1: Web Reconnaissance & Intent Analysis (#SOC-2026-001)
* **Objective:** Identify the most active malicious IP and determine the attacker's intent.
* **Findings:** Detected massive automated scanning (3,531 hits generating 404/500 errors). The attacker (`40.80.148.42`) was specifically targeting `Joomla CMS` paths, primarily `/joomla/index.php/component/search/`, indicating an attempt to exploit SQL injection or search-based vulnerabilities.
* **SPL Used:** `stats`, `top`, `search` filtering.

### Phase 2: Data Exfiltration & Network Pivoting (#SOC-2026-002)
* **Objective:** Determine if the attacker successfully stole data and calculate the payload size.
* **The Challenge:** Application-level IIS logs did not have byte logging enabled (a common real-world misconfiguration).
* **The Pivot:** Pivoted from application logs (`sourcetype="iis"`) to network wire data (`sourcetype="stream:http"`).
* **Findings:** The attacker successfully exfiltrated **18.94 MB** of data. 
* **SPL Used:** `stats sum()`, `eval` (Bytes to MB conversion), `sort`.

### Phase 3: Attack Timeline & Duration Analysis (#SOC-2026-003)
* **Objective:** Group fragmented logs to determine the exact duration of the attack campaign.
* **Findings:** Bypassed Splunk's default memory limits (`maxevents=50000`) to group 20,967 individual requests into a single session. The attack campaign lasted exactly **45.70 minutes** (2742 seconds).
* **SPL Used:** `transaction`, `eval`.

### Phase 4: Threat Intelligence & Geolocation (#SOC-2026-004)
* **Objective:** Enrich raw IP data with geographical context for threat profiling.
* **Findings:** The attacking IP `40.80.148.42` was traced back to **Washington, Virginia, United States**. Profiled as a Known Scanner/Brute-forcer.
* **SPL Used:** `iplocation`, `head`, `table`.

### Phase 5: Attack Visualization for Executive Reporting (#SOC-2026-005)
* **Objective:** Translate raw SPL data into a time-series visual format for management and incident response teams.
* **Findings:** Generated an area chart tracking HTTP error rates per minute. Identified the absolute peak of the attack at **03:07 AM** (928 malicious hits within a single minute).
* **SPL Used:** `timechart span=1m count`.

---

## 🧠 Key Analyst Takeaways
1. **Never blindly trust application logs:** As demonstrated in Phase 2, when IIS logs failed to provide data size metrics, pivoting to Network Stream logs was essential to uncover the exfiltration.
2. **Attention to Syntax is Critical:** A single typo in an HTTP status code (`400` vs `404`) can drastically alter the visual timeline and misguide the incident response team.
3. **Translating Tech to Business:** Used `eval` functions to convert raw bytes into readable Megabytes (MB) and utilized `timechart` to convert raw event counts into executive-friendly visual graphs.

---
*Note: This project is part of a hands-on SIEM training exercise to demonstrate proficiency in log analysis, threat hunting, and incident reporting.*
