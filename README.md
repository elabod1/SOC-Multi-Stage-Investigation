#  SOC Investigation: Multi-Stage Attack Analysis

##  Overview
This repository contains a comprehensive SOC investigation report based on analyzing 2,255 simulated logs across Windows, Linux, Network, Web, and Cloud environments. 

The investigation uncovered a **Multi-Stage Attack** targeting the internal web server `LINUX-WEB-01`, including a successful SSH Brute Force compromise, a blocked SQL Injection attempt, and a blocked internal Port Scan.

**Environment:** Elastic Security / Kibana  
**Language:** KQL (Kibana Query Language)  
**Frameworks:** MITRE ATT&CK  

>  **Disclaimer:** All data used in this investigation is simulated training data. No real production environments or sensitive data are included.

---

##  Skills Demonstrated
- **Threat Hunting:** Analyzing vast datasets to identify malicious patterns.
- **Incident Correlation:** Linking events across time, IPs, and hosts to reconstruct the attack timeline.
- **Log Analysis:** Deep-diving into Web, Network, Authentication, and Process logs.
- **Detection Engineering:** Creating custom rules to alert on malicious IP activity.
- **MITRE ATT&CK Mapping:** (T1110, T1190, T1046).

---

##  Incident Summary

| Incident | Type | Outcome | MITRE ATT&CK |
| :--- | :--- | :--- | :--- |
| **1. SSH Brute Force** | External Attack |  **Successful Compromise** | T1110.001 |
| **2. SQL Injection** | External Attack |  **Blocked by WAF** | T1190 |
| **3. Port Scan** | Internal Recon |  **Blocked by Firewall** | T1046 |

---

##  Full Report
- [Read the full Investigation Report here](./report.md)
## Detection Rule
- [View Rule Explanation (Markdown)](./detection_rule.md)
- [Download the actual exported Rule (JSON)](./Malicious IP Activity.ndjson)
---

##  Author
**Abdulrezzak Juma'a AL-Ahmad**
- LinkedIn: www.linkedin.com/in/abdulrezekalahmad
- GitHub: elabod1
