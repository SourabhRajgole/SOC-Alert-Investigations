
# SOC Investigation 07 — RDP Brute Force Detected

## Alert Information

| Field | Value |
|------|------|
| Event ID | 234 |
| Rule Name | SOC176 – RDP Brute Force Detected |
| Severity | Medium |
| Event Type | Brute Force |
| Detection Platform | LetsDefend SIEM |
| Hostname | Matthew |
| Host IP | 172.16.17.148 |
| Attacker IP | 218.92.0[.]56 |
| Classification | True Positive |
| Status | Contained |

---

## Alert Description

The SIEM generated an alert triggered by the rule **SOC176 – RDP Brute Force Detected**.  
The alert indicates repeated login failures targeting a host named **Matthew (172.16.17.148)** over the **Remote Desktop Protocol (RDP)** service.

RDP brute force attacks occur when attackers repeatedly attempt different username and password combinations in order to gain unauthorized access to a system.

Firewall logs indicate that traffic from the external source IP **218.92.0[.]56** was allowed to reach the destination host. Multiple authentication failures triggered the detection rule.

---

## Detection

### Enrichment & Context

The first step of the investigation was determining whether the attacker IP address was internal or external.

| Source IP | Destination Host |
|-----------|-----------------|
| 218.92.0[.]56 | Matthew (172.16.17.148) |

The source IP address is external and therefore suspicious. External login attempts targeting RDP services are common indicators of brute-force activity.

---

### IP Reputation Control

Threat intelligence checks were performed using:

- VirusTotal  
- AbuseIPDB  
- LetsDefend Threat Intelligence Feed  

Results showed that the IP address **218.92.0[.]56** has been reported multiple times for malicious activity.

VirusTotal flagged the IP as malicious by **11 security engines**, confirming that the source is associated with suspicious activity.

---

## Analysis

### Traffic Analysis

Firewall logs showed **15 connection attempts** from the attacker IP **218.92.0[.]56** targeting:

- Host: Matthew  
- IP: 172.16.17.148  
- Port: 3389 (RDP)

The repeated connection attempts indicate a brute-force attack attempting to access the RDP service.

Further log analysis confirmed that the attacker targeted **only one host**, indicating a focused attack rather than broad scanning.

---

### Endpoint Analysis

Endpoint logs were reviewed to determine whether the attack was successful.

Windows security logs revealed the following:

**Failed Login Attempts**
- Event ID: 4625 (Failed login)
- Usernames attempted:
  - sysadmin
  - admin
  - guest
- Error Code: 0xC000006D (bad username or password)

These failed attempts indicate automated credential guessing.

**Successful Login Event**
- Event ID: 4624 (Successful login)
- Username: Matthew
- Time: March 7, 2024 — 11:44:57 AM

After the successful login, the process **winlogon.exe** was observed on the endpoint, confirming remote authentication.

---

### Post‑Compromise Activity

Shortly after the successful login, the attacker executed several commands on the compromised host:

```
C:\Windows\system32\cmd.exe
whoami
net user letsdefend
net localgroup administrators
netstat -ano
```

These commands indicate the attacker was attempting to gather information about:

- current privileges
- system users
- administrative groups
- active network connections

Such activity is typical during the **discovery phase** of an attack after initial access.

---

## Indicators of Compromise (IOCs)

| Type | Indicator |
|-----|-----------|
| IPv4 | 218.92.0[.]56 |
| Username | admin |
| Username | guest |
| Username | sysadmin |
| Username | Matthew |

---

## MITRE ATT&CK Mapping

| Tactic | Technique |
|------|------|
| Initial Access | T1078 – Valid Accounts |
| Credential Access | T1110 – Brute Force |
| Execution | T1059 – Command and Scripting Interpreter |
| Discovery | T1087 – Account Discovery |

---

## Containment

Based on the investigation results, the system was confirmed to be compromised.

The affected host was immediately isolated from the network using endpoint security controls to prevent lateral movement.

| Hostname | IP |
|----------|----|
| Matthew | 172.16.17.148 |

After containment, the investigation case was closed.

---

## Summary

The alert detected suspicious login attempts targeting the host **Matthew (172.16.17.148)** through the RDP service.

Investigation confirmed:

- Multiple failed login attempts from external IP **218.92.0[.]56**
- A successful login using the username **Matthew**
- Post‑compromise commands executed by the attacker

The attack demonstrates a successful brute-force compromise of an RDP service.

---

## Lessons Learned

- Monitoring authentication logs is critical for detecting brute-force attacks.
- External RDP exposure significantly increases the risk of unauthorized access.
- Rapid investigation and containment reduce the impact of successful compromises.
- Security awareness and monitoring should be combined with threat intelligence for better detection.

---

## Remediation Actions

Recommended remediation actions include:

- Enforce strong password policies
- Implement Multi‑Factor Authentication (MFA) for remote access
- Restrict external RDP access to internal networks or VPN only
- Deploy account lockout policies to prevent brute-force attempts
- Continuously monitor authentication logs for suspicious activity

