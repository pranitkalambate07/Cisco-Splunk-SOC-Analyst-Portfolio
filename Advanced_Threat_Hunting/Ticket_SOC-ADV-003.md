# 🚨 Incident Report: #SOC-ADV-003
**Status:** ✅ Closed | **Priority:** 🔴 Critical | **Assigned To:** Pranit Kalambate

---

## 📋 Ticket Overview
* **Alert Title:** Suspicious LSASS Authentication Activity (Lateral Movement via RDP)
* **Target Environment:** Domain Endpoints (`mercury`, `wrk-ghoppy`)
* **Telemetry Source:** Windows Security Event Logs
* **Event Code:** EventCode 4624 (Logon Success) / Logon Type 10

---

## 🌍 Real-World Scenario
Following a suspected credential dumping incident, the SOC initiated a threat hunt focusing on the Local Security Authority Subsystem Service (`lsass.exe`). The objective was to determine if the compromised credentials were being actively utilized to traverse the network (Lateral Movement) and access critical infrastructure.

---

## 🕵️‍♂️ SOC Analyst Investigation Logic
1. **Raw Telemetry Pivot:** Due to parsing anomalies in the SIEM dataset, I bypassed strict field constraints (`sourcetype`) and executed a raw text keyword search targeting `lsass.exe` and the string `10` to capture both Sysmon memory access attempts and RDP logon types.
2. **Behavioral Analysis:** The search returned over 1,441 events. Analysis of the raw logs revealed multiple successful RDP authentications (Logon Type 10) verified by the LSASS process across multiple hosts (`mercury`, `wrk-ghoppy`). This strongly indicates the adversary is 'living off the land' using stolen credentials to pivot via Remote Desktop Protocol.

---
**Visual Evidence:** ![PowerShell Execution Evidence](Ticket_SOC-ADV-003/evidence.png)
## 💻 Technical Evidence
### SPL Query Executed
```splunk
index=botsv2 "lsass.exe" 10
| table _time, host, _raw
