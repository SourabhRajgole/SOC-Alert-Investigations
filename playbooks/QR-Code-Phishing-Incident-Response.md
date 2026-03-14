# QR Code Phishing (Quishing) Incident Response Playbook

Playbook ID: PB-SOC-002  
Category: Email Security / Phishing  
Framework Alignment: NIST SP 800-61 Incident Response Lifecycle  
MITRE ATT&CK Reference: T1566 Phishing  

---

# 1. Purpose

This playbook provides a standardized procedure for Security Operations Center (SOC) analysts to investigate and respond to **QR Code Phishing (Quishing)** incidents.

Quishing is a phishing technique where attackers embed malicious URLs inside QR codes to bypass email security filters and trick users into visiting credential harvesting sites or downloading malware.

The objectives of this playbook are to:

• Detect QR-code based phishing attempts  
• Prevent credential compromise or malware infection  
• Contain affected accounts or endpoints  
• Remove malicious emails from the environment  

---

# 2. Scope

This playbook applies to incidents involving:

• Suspicious QR codes embedded in email messages  
• Security alerts detecting QR-code phishing  
• User reports of suspicious QR codes in emails  
• Detection rules such as:

SOC251 – Quishing Detected  
ZeroFont Phishing Detection  
Email Gateway Suspicious Attachment Alert

---

# 3. Roles and Responsibilities

### Tier 1 SOC Analyst

• Validate the alert  
• Collect email metadata  
• Identify suspicious indicators  
• Escalate if malicious indicators are confirmed  

### Tier 2 SOC Analyst

• Perform QR code decoding and analysis  
• Conduct threat intelligence enrichment  
• Investigate endpoint or authentication logs  

### Incident Response Team

• Contain compromised accounts or endpoints  
• Coordinate remediation actions  
• Conduct post-incident analysis  

---

# 4. Detection and Analysis

## 4.1 Alert Validation

Review alert details and extract the following information:

Sender Email Address  
Recipient Email Address  
SMTP Source IP Address  
Email Subject  
Timestamp of Email Delivery  
Presence of QR Code in the message  

Example indicators:

Email containing a QR code asking the user to verify MFA  
Email requesting scanning a code to log into a service  
QR codes embedded inside PDF or image attachments  

---

## 4.2 Email Content Analysis

Analyze the email message body for phishing indicators.

Common characteristics include:

• Urgency or fear-based messaging  
• Security update or MFA verification requests  
• Requests to scan QR code for login verification  
• Impersonation of trusted services (Microsoft, Okta, etc.)

Example message patterns:

"Security update required for your account"

"Scan the QR code to complete your authentication"

"Failure to update security settings will result in account suspension"

---

## 4.3 QR Code Analysis

Extract and decode the QR code.

Recommended tools:

CyberChef  
QR Code Decoder Tools  
Mobile forensic tools  
Email security platform sandbox  

Steps:

1. Extract QR image from email  
2. Decode QR code to reveal embedded URL  
3. Analyze the decoded URL  

---

## 4.4 URL Analysis

Investigate the decoded URL using threat intelligence tools.

Recommended tools:

VirusTotal  
Any.run  
URLScan  
Hybrid Analysis  

Check for the following:

Credential harvesting pages  
Malicious redirects  
Known phishing infrastructure  
Malware downloads  

Example malicious behavior:

Fake Microsoft login page  
Credential harvesting forms  
POST requests sending credentials to attacker servers

---

## 4.5 Threat Intelligence Enrichment

Investigate the following indicators:

Sender domain reputation  
SMTP IP reputation  
Decoded URL reputation  

Sources:

VirusTotal  
AbuseIPDB  
Threat Intelligence feeds  
Internal IOC databases  

Determine if the infrastructure has been associated with phishing campaigns.

---

## 4.6 User Interaction Investigation

Determine whether the recipient interacted with the phishing email.

Investigate:

QR code scans  
Authentication logs  
Browser history  
Proxy logs  

Possible scenarios:

User scanned QR code using mobile device  
User visited phishing page  
User entered credentials  

Note:

Mobile device scans may bypass endpoint monitoring systems.

---

# 5. Containment

If the email is confirmed malicious:

• Remove the phishing email from all user mailboxes  
• Block malicious sender domain and SMTP IP  
• Block decoded malicious URLs at web gateway  
• Disable affected user accounts if credentials were entered  
• Isolate infected endpoints if malware execution occurred  

---

# 6. Eradication

Remove attacker access and malicious artifacts.

Actions include:

Reset compromised credentials  
Revoke active authentication sessions  
Remove malicious persistence mechanisms  
Update phishing detection rules  

---

# 7. Recovery

Restore normal operations.

Actions include:

Re-enable user access after credential reset  
Enable Multi-Factor Authentication (MFA)  
Verify system integrity  
Monitor authentication logs for suspicious activity  

---

# 8. Post-Incident Activities

After containment and recovery:

Conduct incident review  
Document the attack timeline  
Update detection rules  
Improve email filtering policies  

User awareness training should also be conducted to educate employees about QR code phishing threats.

---

# 9. MITRE ATT&CK Mapping

| Tactic | Technique |
|------|------|
| Reconnaissance | T1589 – Gather Victim Identity Information |
| Initial Access | T1566 – Phishing |
| Credential Access | T1556 – Credential Harvesting |
| Execution | T1204 – User Execution |
| Defense Evasion | T1036 – Masquerading |

---

# 10. Indicators of Compromise (Examples)

| Indicator Type | Example |
|---------------|--------|
| Malicious Domain | ipfs.io |
| Malicious Domain | nsggroup.it |
| Malicious URL | credential harvesting page |
| SMTP IP | malicious email infrastructure |
| Email Subject | Security Update Required |

---

# 11. References

NIST SP 800-61 — Computer Security Incident Handling Guide  
MITRE ATT&CK Framework  
SANS Incident Handler Handbook  
OWASP Phishing Prevention Guidelines
