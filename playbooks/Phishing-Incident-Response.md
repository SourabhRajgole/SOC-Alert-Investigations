# Phishing Incident Response Playbook

Framework Alignment: NIST SP 800-61  
Category: Email Security  
Severity: Low / Medium / High  

---

# 1. Purpose

This playbook provides standardized procedures for Security Operations Center (SOC) analysts to investigate and respond to phishing-related security alerts.

The objective is to:

• Detect phishing attempts  
• Prevent credential theft or malware infection  
• Contain compromised accounts  
• Remove malicious emails from the environment  

---

# 2. Scope

This playbook applies to all phishing-related alerts detected through:

Email security gateways  
SIEM alerts  
User-reported suspicious emails  
Threat intelligence feeds  

Examples of applicable alerts:

SOC101 – Phishing Mail Detected  
SOC282 – Phishing Deceptive Mail  
SOC251 – QR Code Phishing (Quishing)

---

# 3. Roles and Responsibilities

SOC Analyst (Tier 1)

Perform alert triage  
Collect relevant log data  
Escalate confirmed incidents  

SOC Analyst (Tier 2)

Perform detailed threat analysis  
Investigate indicators of compromise  
Recommend containment actions  

Incident Response Team

Execute containment and remediation procedures  
Perform forensic investigation if required  

---

# 4. Detection and Analysis

## 4.1 Alert Validation

Review alert metadata and verify the following:

Sender email address  
Recipient email address  
SMTP source IP  
Email subject  
Timestamp of delivery  
Presence of attachments or URLs  

---

## 4.2 Email Content Analysis

Inspect the email for common phishing characteristics:

Urgency or fear-based messaging  
Credential verification requests  
Suspicious links or attachments  
Impersonation of trusted entities  

Examples:

"Verify your account immediately"  
"Your account will be suspended"  
"Urgent security update required"

---

## 4.3 URL and Attachment Analysis

If the email contains URLs or attachments:

Analyze using security analysis tools.

Recommended tools:

VirusTotal  
Any.Run  
Hybrid Analysis  
URLScan  

Analysts should verify:

Malicious detection results  
Credential harvesting behavior  
Malware payload delivery  

---

## 4.4 Email Delivery Verification

Determine whether the email was:

Blocked  
Delivered  
Quarantined  

If delivered:

Identify all affected recipients using email security logs.

---

## 4.5 User Interaction Analysis

Determine whether the user interacted with the malicious email.

Investigate:

URL click activity  
Attachment downloads  
Malware execution events  

Log sources:

Proxy logs  
Endpoint Detection and Response (EDR) telemetry  
Browser logs  
Authentication logs  

---

# 5. Containment

If the email is confirmed malicious, perform the following actions:

Delete the malicious email from user mailboxes  
Block sender domain and SMTP IP  
Block malicious URLs on web proxy  
Isolate infected endpoints if malware was executed  

---

# 6. Eradication

Remove all malicious artifacts from affected systems.

Actions include:

Malware removal from endpoints  
Removal of persistence mechanisms  
Updating email filtering rules  

---

# 7. Recovery

Restore affected systems and user accounts.

Actions include:

Reset compromised credentials  
Revoke active authentication sessions  
Enable Multi-Factor Authentication (MFA)  
Verify system integrity after remediation  

---

# 8. Post-Incident Activities

Conduct a post-incident review.

Activities include:

Review incident timeline  
Document lessons learned  
Update detection rules and playbooks  

---

# 9. MITRE ATT&CK Mapping

| Tactic | Technique |
|------|------|
| Initial Access | T1566 – Phishing |
| Execution | T1204 – User Execution |
| Credential Access | T1556 – Phishing Credential Harvesting |
| Defense Evasion | T1036 – Masquerading |

---

# 10. References

NIST SP 800-61 — Computer Security Incident Handling Guide  
MITRE ATT&CK Framework  
SANS Incident Handler Handbook
