================================================================================
TICKET ID: #SOC-2026-001                               PRIORITY: HIGH
STATUS: OPEN                                           ASSIGNED TO: Pranit Kalambate
ALERTER: Splunk ES Correlation Engine
================================================================================

ALERT TITLE: 
Anomalous Inbound Web Traffic & Potential Vulnerability Scanning Detected.

DETAILED DESCRIPTION:
Our external-facing web infrastructure (hosting the domain: imreallynotabat.com) 
has triggered a threshold alert. The SIEM has detected an unusual spike in 
HTTP status codes (404 Not Found and 500 Internal Server Error) within a short 
timeframe. 

This behavior is highly indicative of automated web reconnaissance, directory 
brute-forcing, or a malicious vulnerability scanner (like Nikto/Dirbuster) 
attempting to find exposed pages or code.

TELEMETRY DATA SOURCE AVAILABLE:
- sourcetype = access_combined 
- index = botsv1

OBJECTIVES FOR THE ANALYST:
1. Identify the malicious external Source IP address targeting our server.
2. Quantify the attack: Find the total number of connection attempts made by this IP.
3. Determine the attacker's intent: Which specific URI paths (web pages) was the attacker trying to scan or exploit?
4. Write the SPL query used to uncover the evidence.
