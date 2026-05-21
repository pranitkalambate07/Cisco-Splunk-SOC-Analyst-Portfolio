# 🚨 Incident Report: #SOC-2026-003
**Status:** ✅ Closed | **Priority:** 🟠 Medium | **Assigned To:** Pranit Kalambate

---

## 📋 Ticket Overview
* **Alert Title:** Prolonged Malicious Web Session (Campaign Duration)
* **Target Server:** Web Server (IIS)
* **Telemetry Source:** `index=botsv1` | `sourcetype=iis`

---

## 🔍 Detailed Description
Following the identification of the primary threat actor (`40.80.148.42`) involved in vulnerability scanning and data exfiltration, the Incident Response (IR) team requires a timeline analysis. We need to group the attacker's disparate HTTP requests into a single continuous session to determine the exact duration of the attack campaign.

---

## 🕵️‍♂️ Investigation & Findings

### ⏱️ Attack Timeline Analysis
- **Attacker IP:** `40.80.148.42`
- **Total Attack Duration (Seconds):** `2742`
- **Total Attack Duration (Minutes):** `45.70`

---

## 💻 Technical Evidence (SPL Query)
```splunk
index="botsv1" sourcetype="iis" c_ip="40.80.148.42" | transaction c_ip maxevents=50000 | eval duration_minutes=round((duration/60),2) | table c_ip, duration, duration_minutes, eventcount
