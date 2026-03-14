# Brute Force Attack Incident Response Playbook

Playbook ID: PB-SOC-003  
Category: Authentication / Account Compromise  
Framework Alignment: NIST SP 800-61 Incident Response Lifecycle  
MITRE ATT&CK Reference: T1110 – Brute Force  

---

# 1. Purpose

This playbook provides a standardized investigation and response procedure for **brute force authentication attacks**.

A brute force attack occurs when an attacker repeatedly attempts to guess a user's password by submitting multiple login attempts until the correct credentials are discovered.

These attacks commonly target:

• Remote Desktop Protocol (RDP)  
• SSH servers  
• VPN portals  
• Web authentication portals  
• Cloud identity providers  

The objective of this playbook is to enable SOC analysts to:

• Detect brute force login attempts  
• Determine if an attack was successful  
• Identify compromised accounts or systems  
• Contain the threat and prevent unauthorized access  

---

# 2. Scope

This playbook applies to alerts triggered by authentication anomaly detection such as:

Multiple failed login attempts  
Rapid login attempts from a single source  
Authentication attempts against multiple accounts  

Example alerts:

SOC176 – RDP Brute Force Detected  
Excessive Login Failures  
Suspicious Authentication Attempts  

---

# 3. Roles and Responsibilities

### Tier 1 SOC Analyst

• Validate brute force alert  
• Identify source and destination of login attempts  
• Collect authentication logs  
• Escalate confirmed attacks  

### Tier 2 SOC Analyst

• Perform log analysis  
• Determine if login attempts succeeded  
• Investigate lateral movement  

### Incident Response Team

• Contain compromised accounts  
• Perform remediation  
• Conduct root cause analysis  

---

# 4. Detection and Analysis

## 4.1 Alert Validation

Review alert metadata and collect the following information:

Source IP address  
Destination system or service  
Target username(s)  
Number of failed login attempts  
Authentication protocol (RDP, SSH, VPN, Web Login)

Example alert indicators:

Large number of failed login attempts  
Repeated authentication failures from the same IP  
Attempts targeting default usernames  

---

## 4.2 Identify Source and Destination

Determine whether the attack originates from an internal or external IP address.

Key investigation points:

Source IP Address  
Destination Hostname  
Destination IP Address  
Authentication Service  

Example:

Source IP: 218.92.0[.]56  
Destination Host: Matthew  
Destination IP: 172.16.17.148  
Protocol: RDP (TCP 3389)

If the source IP is external, the attack may be an internet-based brute force attempt.

---

## 4.3 Threat Intelligence Enrichment

Investigate the reputation of the source IP address.

Use threat intelligence sources such as:

VirusTotal  
AbuseIPDB  
GreyNoise  
Threat intelligence feeds  

Indicators to check:

Previous malicious activity  
Known scanning or brute force infrastructure  
Geographic location of the attacker  

If the IP is associated with malicious activity, it strengthens the likelihood of an active attack.

---

## 4.4 Log Analysis

Search the SIEM or log management platform for authentication activity from the attacker IP.

Analyze:

Firewall logs  
Authentication logs  
VPN logs  
Endpoint security logs  

Example log indicators:

Repeated connection attempts to port 3389  
Multiple failed authentication attempts  
Authentication attempts against multiple usernames

---

## 4.5 Authentication Log Investigation

Analyze endpoint authentication logs to determine whether the attack succeeded.

### Windows Authentication Events

Event ID 4625 — Failed login attempt  
Event ID 4624 — Successful login  

Indicators of compromise:

Successful login after multiple failed attempts  
Login from unusual geographic location  
Login during unusual hours  

---

## 4.6 Endpoint Activity Analysis

If a successful login occurred, investigate post-authentication activity.

Check for:

New processes executed  
Privilege escalation attempts  
Network connections initiated by attacker  

Example commands attackers often execute:

whoami  
net user  
net localgroup administrators  
netstat -ano  

These commands help attackers gather system information.

---

# 5. Containment

If the brute force attack is confirmed:

Block attacker IP address at the firewall  
Disable compromised user accounts  
Isolate compromised endpoints if necessary  

If login success occurred:

Terminate active sessions  
Reset credentials immediately  
Investigate persistence mechanisms  

---

# 6. Eradication

Remove attacker access and eliminate vulnerabilities.

Actions include:

Password resets for compromised accounts  
Removal of malicious processes  
Patching exposed services  
Reviewing access controls  

---

# 7. Recovery

Restore affected systems to normal operation.

Recommended actions:

Enable Multi-Factor Authentication (MFA)  
Implement account lockout policies  
Monitor authentication logs for continued activity  

Verify that the attacker no longer has access to the system.

---

# 8. Post-Incident Activities

After the incident is resolved:

Conduct incident review  
Improve authentication monitoring rules  
Update brute force detection thresholds  

Security awareness training may also be recommended.

---

# 9. MITRE ATT&CK Mapping

| Tactic | Technique |
|------|------|
| Credential Access | T1110 – Brute Force |
| Initial Access | T1078 – Valid Accounts |
| Discovery | T1087 – Account Discovery |
| Execution | T1059 – Command Execution |

---

# 10. Indicators of Compromise (Examples)

| Indicator Type | Value |
|---------------|------|
| IPv4 Address | 218.92.0[.]56 |
| Username | admin |
| Username | guest |
| Username | sysadmin |
| Username | Matthew |

---

# 11. References

NIST SP 800-61 — Computer Security Incident Handling Guide  
MITRE ATT&CK Framework  
SANS Incident Handler Handbook  
Microsoft Security Event Documentation
