# 🚨 Incident Report: #SOC-2026-001
**Status:** 🔄 In Progress | **Priority:** 🔴 High | **Assigned To:** Pranit Kalambate

---

## 📋 Ticket Overview
* **Alert Title:** Anomalous Inbound Web Traffic & Potential Vulnerability Scanning Detected
* **Target Domain:** `imreallynotabat.com`
* **Telemetry Source:** `index=botsv1` | `sourcetype=access_combined`

---

## 🔍 Detailed Description
Our external-facing web infrastructure has triggered a threshold alert due to an unusual spike in HTTP status codes **404 (Not Found)** and **500 (Internal Server Error)** within a short timeframe. This behavior indicates automated web reconnaissance, directory brute-forcing, or scanners (e.g., Nikto/Dirbuster) attempting to locate exposed files.

---

## 🕵️‍♂️ Investigation & Findings
*To be filled out after running Splunk queries...*

### 📥 1. Malicious Source IP & Attack Volume
- **Attacker IP:** `40.80.148.42`
- **Total Request Count:** `3531`

### 🗺️ 2. Attacker Intent (Targeted URI Paths)
- *What specific directories or pages was the attacker scanning?*

---

## 💻 Technical Evidence (SPL Query)
```splunk
index=botsv1 sourcetype=access_combined (status=404 OR status=500) | top limit=10 clientip
