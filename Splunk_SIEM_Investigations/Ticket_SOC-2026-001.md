# 🚨 Incident Report: #SOC-2026-001
**Status:** ✅ Closed | **Priority:** 🔴 High | **Assigned To:** Pranit Kalambate

---

## 📋 Ticket Overview
* **Alert Title:** Anomalous Inbound Web Traffic & Potential Vulnerability Scanning Detected
* **Target Domain:** `imreallynotabat.com`
* **Telemetry Source:** `index=botsv1` | `sourcetype=iis`

---

## 🔍 Detailed Description
Our external-facing web infrastructure has triggered a threshold alert due to an unusual spike in HTTP status codes **404 (Not Found)** and **500 (Internal Server Error)** within a short timeframe. This behavior indicates automated web reconnaissance, directory brute-forcing, or scanners (e.g., Nikto/Dirbuster) attempting to locate exposed files.

---

## 🕵️‍♂️ Investigation & Findings

### 📥 1. Malicious Source IP & Attack Volume
- **Attacker IP:** `40.80.148.42`
- **Total Request Count:** `3531`
- **Visual Evidence:** ![Source IP](<Splunk_SIEM_Investigations/Ticket SOC-2026-001/image1.png>)

### 🗺️ 2. Attacker Intent (Targeted URI Paths)
- **Targeted Framework:** `Joomla CMS`
- **Primary Attack Vector:** `/joomla/index.php/component/search/` (Highly targeted for potential injection vulnerabilities).
- **Secondary Target:** `/joomla/administrator/index.php` (Attempted admin panel discovery/brute-forcing).
- **Visual Evidence:** ![Attacker Intent](<Splunk_SIEM_Investigations/Ticket SOC-2026-001/image2.png>)

---

## 💻 Technical Evidence (SPL Query)
```splunk
index=botsv1 sourcetype=iis sc_status="404" OR sc_status="500" | top limit=10 c_ip
index=botsv1 sourcetype=iis c_ip="40.80.148.42" | top limit=10 cs_uri_stem
