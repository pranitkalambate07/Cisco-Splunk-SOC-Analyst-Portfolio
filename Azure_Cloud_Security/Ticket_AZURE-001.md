# 🚨 Cloud Incident Report: #AZURE-001
**Status:** ✅ Closed | **Priority:** 🔴 High | **Assigned To:** Pranit Kalambate

---

## 📋 Ticket Overview
* **Alert Title:** Cloud Identity Threat - Azure AD Password Spraying
* **Target Environment:** Microsoft Azure Active Directory (Azure AD)
* **Telemetry Source:** `azure:monitor:aad` (Azure AD Sign-In Logs)
* **Target Error Code:** 50126 (Invalid Username or Password)

---

## 🌍 Real-World Scenario
A high-volume authentication anomaly was detected within the Azure Active Directory tenant. The threat actor attempted to compromise cloud identities by executing a "Password Spraying" attack. Instead of brute-forcing a single account and triggering conditional access lockout policies, the attacker tested a small subset of common passwords across a wide range of corporate email addresses to remain undetected.

---

## 🕵️‍♂️ SOC Analyst Investigation Logic
1. **Error Code Targeting:** In Azure AD telemetry, failed authentications strictly due to invalid credentials generate the specific error code `50126`. I used this exact condition to filter out regular telemetry noise.
2. **Aggregation & Attribution:** I aggregated the failed sign-in logs by the Source IP (`ipAddress`) and Target User (`userPrincipalName`). Observing a single external IP address generating the `50126` error code across multiple distinct internal user accounts is the definitive behavioral signature of an automated Password Spraying attack.

---

## 💻 Technical Evidence
### SPL Query Executed
```splunk
index="botsv3" sourcetype="ms:aad:signin" "50126" 
| stats count by ipAddress, userPrincipalName 
| sort - count
