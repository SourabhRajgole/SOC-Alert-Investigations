# Incident Response Playbook

## Incident Type
QR Code Phishing (Quishing)

---

# Introduction

QR code phishing, commonly known as "Quishing", is a phishing technique where attackers embed malicious URLs inside QR codes to bypass email security filters.

Users scan the QR code with mobile devices which redirects them to credential harvesting websites.

---

# Summary

This playbook outlines response steps for QR-code based phishing incidents and helps responders to:

• Decode malicious QR codes  
• Investigate embedded URLs  
• Identify compromised accounts  
• Remove malicious emails

---

# Incident Description

Common quishing scenarios include:

• QR code login verification emails  
• QR codes embedded inside PDFs  
• QR codes used to bypass URL filtering

These attacks often target:

Microsoft 365 accounts  
Corporate VPN credentials  
SSO login portals

---

# Incident Response Process

## Part 1 — Acquire, Preserve, Document Evidence

Collect:

Sender email  
Recipient  
Email body  
QR code image  
Decoded URL

Decode QR code using:

CyberChef  
QR code decoder tools

Analyze decoded URL with:

VirusTotal  
URLScan

---

## Part 2 — Containment

Remove the phishing email from mailboxes.

Block malicious domains.

Disable compromised accounts if credentials were submitted.

---

## Part 3 — Eradication

Reset compromised passwords.

Revoke active authentication sessions.

Remove malicious persistence mechanisms.

---

## Part 4 — Recovery

Enable MFA.

Monitor authentication logs.

Ensure attacker access is removed.

---

## Part 5 — Post-Incident Activity

Update phishing detection rules.

Conduct user awareness training focused on QR code threats.

---

# MITRE ATT&CK Mapping

T1566 Phishing  
T1204 User Execution  
T1556 Credential Harvesting

---

# References

Microsoft Security Playbooks  
NIST SP 800-61
