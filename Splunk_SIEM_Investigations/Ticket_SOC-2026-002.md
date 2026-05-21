
# 🚨 Incident Report: #SOC-2026-002
**Status:** ✅ Closed | **Priority:** 🔴 High | **Assigned To:** Pranit Kalambate

---

## 📋 Ticket Overview
* **Alert Title:** Potential Data Exfiltration: Large Payload Transfer Detected
* **Target Server:** Web Server (IIS)
* **Telemetry Source:** `index=botsv1` | `sourcetype=iis`

---

## 🔍 Detailed Description
The SOC has received an alert regarding an abnormal spike in outbound data transfer from our web infrastructure. This behavior suggests potential Data Exfiltration, where an attacker might be downloading large sensitive files or database dumps. The raw data size is logged in bytes, requiring mathematical conversion for accurate impact assessment.

---

## 🕵️‍♂️ Investigation & Findings

### 📥 1. Top Exfiltrator Details
- **Suspect IP:** `40.80.148.42`
- **Total Data Stolen (MB):** `18.94 MB`

---

## 💻 Technical Evidence (SPL Query)
```splunk
index="botsv1" sourcetype="stream:http" | stats sum(bytes_out) as TotalBytes by src_ip | eval Total_MB = round((TotalBytes/1024/1024), 2) | sort - Total_MB | head 10
