# Incident Response Playbook

## Incident Type
Phishing Email (Standard, Spear Phishing, Attachment-Based)

---

# Introduction

This playbook provides procedures for identifying, investigating, and responding to phishing email incidents.

Phishing attacks attempt to deceive users into revealing sensitive information or executing malicious payloads through fraudulent emails.

These attacks may involve:

• Credential harvesting websites  
• Malicious attachments  
• Malware delivery  
• Business Email Compromise (BEC)

The objective of this playbook is to ensure that phishing incidents are investigated consistently and that compromised accounts or systems are quickly contained.

---

# Summary

This playbook outlines response procedures for phishing incidents based on the NIST Incident Response Lifecycle.

The playbook helps analysts to:

• Identify phishing emails  
• Investigate indicators of compromise  
• Contain compromised accounts  
• Remove malicious emails from the environment  
• Conduct post-incident analysis

---

# Incident Description

Phishing emails typically include:

• Malicious links  
• Fake login pages  
• Malware attachments  
• Requests for sensitive information

Common phishing indicators include:

• Suspicious sender addresses  
• Urgent requests for action  
• Login verification requests  
• Unexpected attachments

---

# Incident Response Process

## Part 1 — Acquire, Preserve, Document Evidence

Collect the following information:

Sender email address  
Recipient email address  
Email subject  
Message ID  
Email headers  
Attachment hashes  
Embedded URLs

Investigate:

• Email header analysis  
• Domain reputation  
• URL reputation  
• Attachment analysis

Recommended tools:

VirusTotal  
URLScan  
Hybrid Analysis  
Email gateway logs

Determine whether:

• Other users received the email  
• The user interacted with the email  
• Credentials were submitted

---

## Part 2 — Containment

If the email is confirmed malicious:

• Remove the email from all mailboxes  
• Block sender domain  
• Block malicious URLs  
• Disable compromised accounts

---

## Part 3 — Eradication

Remove attacker persistence mechanisms:

Reset compromised passwords  
Revoke authentication tokens  
Remove malicious email rules  
Delete malicious files

---

## Part 4 — Recovery

Restore normal user access:

Re-enable accounts  
Enable MFA if not enabled  
Monitor authentication activity

---

## Part 5 — Post-Incident Activity

Conduct lessons learned review:

Update email filtering rules  
Improve phishing detection policies  
Conduct user awareness training

---

# MITRE ATT&CK Mapping

Initial Access — T1566 Phishing  
Credential Access — T1556 Credential Harvesting  
Execution — T1204 User Execution

---

# References

NIST SP 800-61 Incident Handling Guide  
Microsoft Security Operations Playbooks  
SANS Incident Handler Handbook
