# Detection Rule Explanation

## 1. Objective
To create a custom detection rule in Elastic Security that instantly alerts the SOC team when a specific malicious or suspicious IP address (`198.51.100.23`) appears in any system log, whether as a source or a destination.

## 2. Rule Configuration
- **Rule Name:** `SOC-Detection: Known Malicious IP Activity`
- **Rule Type:** Custom Query
- **Severity:** High
- **Risk Score:** 73
- **Index Pattern:** `multi_attack` (or `logs-*`)

**KQL Query Used:**
```kql
source.ip: "198.51.100.23" or destination.ip: "198.51.100.23"
```
3. How It Works
The rule is scheduled to run every 5 minutes. During each run, it searches the specified index for any document containing the targeted IP address.

If the IP is found as a source.ip, it means the attacker is initiating traffic.

If the IP is found as a destination.ip, it means internal systems are communicating with the attacker (e.g., C2 beaconing or data exfiltration).

If a match is found, an Alert is immediately generated in the Elastic Security dashboard, regardless of how many times the IP appears.

4. Expected Outcome & Value
This rule provides early warning from the very first appearance of the IP. Unlike a Threshold rule (which waits for multiple failed attempts), this Custom Query rule ensures that a known threat actor cannot operate silently. It was tested against the multi_attack dataset, confirming it successfully flags the SSH compromise traffic and subsequent attacks.
