# SOC Investigation Report: Multi-Stage Attack Analysis

**Analyst:** Abdulrezzak Juma'a AL-Ahmad  
**Index:** `multi_attack`  
**Date:** October 18, 2026

## 1. Executive Summary
Analysis of the `multi_attack` dataset revealed three distinct security incidents targeting the internal web server `LINUX-WEB-01` (`10.20.10.21`). The incidents include a successful SSH Brute Force compromise, a blocked SQL Injection attempt, and a blocked internal Port Scan. While perimeter defenses (WAF and Firewall) successfully blocked the external web attack and internal reconnaissance, the SSH compromise resulted in active unauthorized access to the server.

## 2. Incident 1: SSH Brute Force Compromise (External) 🚨
**Summary:** An automated SSH Brute Force attack was detected targeting `LINUX-WEB-01`. After multiple failed attempts, the attacker successfully compromised the `deploy` account and began executing reconnaissance commands within the system.

**Evidence:**
- **Attacker IP:** `198.51.100.23` (External).
- **Target:** `LINUX-WEB-01` (`10.20.10.21`), Port 22.
- **Targeted Users:** `deploy` (Compromised), `root` (Failed).
- **Time Range:** `01:04:00` to `01:23:00`.
- **Result:** Successful login at `01:19:00`, followed by command execution (`whoami`, `id`, `hostname`, `pwd`).

**Queries Used:**
```kql
source.ip: "198.51.100.23" and event.action: "ssh_login"
source.ip: "198.51.100.23" and event.outcome: "success"
```
Conclusion:  True Positive (Successful Compromise) - The server is fully compromised.

3. Incident 2: SQL Injection (External)
Summary: An automated SQL Injection attack was launched from an external IP between 04:18 and 04:24.

Evidence:

Attacker: 198.51.100.78 | Target: LINUX-WEB-01 (Port 443).

Result: All requests returned 403 Forbidden.

Queries Used:

```kql
event.kind: "alert" and rule.name: "Training - SQL Injection Pattern"
source.ip: "198.51.100.78" and event.category: "web" and event.outcome: "success"
```
Conclusion:  True Positive (Blocked) - Malicious activity failed.

4. Incident 3: Internal Port Scan
Summary: An aggressive port scan occurred between 05:09 and 05:17 originating from an internal host.

Evidence:

Attacker: 10.20.30.55 (Internal) | Target: LINUX-WEB-01.

Result: All attempts logged as connection_denied.

Queries Used:
```
kql
source.ip: "10.20.30.55" or destination.ip: "10.20.30.55"
source.ip: "10.20.30.55" and event.category: "network" and event.action: "connection_allowed"
```
Conclusion:  True Positive (Blocked) - Reconnaissance failed, but the source host is highly suspicious.

5. Final Recommendations
Immediate Action: Isolate LINUX-WEB-01 (compromised) and 10.20.30.55 (suspicious).

Immediate Action: Block external IPs 198.51.100.23 and 198.51.100.78.

Detection Engineering: Create a detection rule for repeated connection_denied events.
