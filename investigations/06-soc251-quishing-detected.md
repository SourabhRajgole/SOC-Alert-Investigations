
# SOC Investigation 06 — Quishing Detected (QR Code Phishing)

## Alert Information

| Field | Value |
|------|------|
| Event ID | 214 |
| Rule Name | SOC251 – Quishing Detected (QR Code Phishing) |
| Severity | Medium |
| Event Type | Email Security |
| Detection Platform | LetsDefend SIEM |
| Hostname | Claire |
| Host IP | 172.16.17.181 |
| SMTP IP | 158.69.201.47 |
| Classification | True Positive |

---

## Alert Description

A phishing alert was triggered by the rule **SOC251 – Quishing Detected (QR Code Phishing)** after a suspicious QR code was identified in an email.

The email was sent to **Claire** from **security@microsecmfa.com** with the subject:

**"New Year's Mandatory Security Update: Implementing Multi‑Factor Authentication (MFA)"**

The email requested the user to scan a QR code to update MFA security. QR codes in phishing campaigns are often used to hide malicious URLs and bypass traditional email security filters.

The email security system marked the action as **Allowed**, meaning the email was delivered to the user inbox.

---

## Detection and Initial Triage

Initial investigation focused on analyzing the email metadata and message content.

Key information gathered:

- Email Sent Time: Jan 01, 2024, 12:00 PM  
- Sender Email: security@microsecmfa.com  
- Recipient Email: claire@letsdefend.io  
- SMTP Address: 158.69.201.47  
- Email Subject: New Year's Mandatory Security Update: Implementing Multi‑Factor Authentication (MFA)

The message contained urgency language suggesting the user must update MFA security or risk losing access to their account. This pressure tactic is commonly used in phishing attacks.

The email included a **QR code**, which required further investigation.

---

## Quishing Analysis

To analyze the QR code, the encoded data was extracted using **CyberChef**.

The decoded QR code contained the following URL:

https://ipfs[.]io/ipfs/Qmbr8wmr41C35c3K2GfiP2F8YGzLhYpKpb4K66KU6mLmL4#

Further investigation using threat intelligence and sandbox platforms revealed:

- The URL hosts a **fake webmail login page**
- The website collects user credentials via a malicious POST request
- The captured credentials are sent to:

https://www.nsggroup[.]it/fhfh/ffftt/hhnew.php

This behavior indicates that the site is designed to **steal user credentials**.

VirusTotal analysis showed that **multiple security engines flagged the URL as malicious and phishing-related**.

---

## Investigation Findings

The investigation identified the following:

- The phishing email successfully bypassed the email security system.
- The email used a **QR code to hide a malicious URL**.
- The QR code directed the user to a **credential harvesting website**.
- The malicious site attempted to collect usernames and passwords through a fake login form.
- The attack infrastructure has been previously associated with phishing campaigns.

There were no endpoint logs showing execution activity, which is likely because the QR code could have been scanned using a **mobile device**, bypassing workstation monitoring systems.

---

## Indicators of Compromise (IOCs)

| Type | Indicator |
|-----|-----------|
| Domain | ipfs[.]io |
| Domain | nsggroup[.]it |
| URL | https://ipfs[.]io/ipfs/Qmbr8wmr41C35c3K2GfiP2F8YGzLhYpKpb4K66KU6mLmL4# |
| URL | https://www.nsggroup[.]it/fhfh/ffftt/hhnew.php |
| SMTP IP | 158.69.201.47 |
| Email Subject | New Year's Mandatory Security Update: Implementing Multi‑Factor Authentication (MFA) |

---

## MITRE ATT&CK Mapping

| Tactic | Technique |
|------|------|
| Reconnaissance | Gather Victim Identity Information |
| Reconnaissance | Phishing for Information |
| Initial Access | Phishing |
| Execution | User Execution |
| Defense Evasion | Obfuscated Files or Information |
| Defense Evasion | Impersonation |

---

## Investigation Conclusion

The alert was classified as a **True Positive**.

The attacker attempted to steal user credentials through a **QR code phishing attack (Quishing)**. The QR code redirected the user to a fake login page designed to collect credentials.

Although there was no evidence of endpoint execution, the phishing infrastructure and malicious URL confirm that the email was part of a credential harvesting campaign.

---

## Recommended Response Actions

Recommended security actions:

- Remove the phishing email from user mailboxes
- Reset potentially compromised credentials
- Block malicious domains and URLs
- Educate users about QR code phishing attacks
- Monitor authentication logs for suspicious login attempts

---

## Lessons Learned

Attackers increasingly use **QR codes in phishing emails** to bypass traditional email security filters.

Security teams should:

- inspect QR codes within emails
- implement QR scanning analysis tools
- train employees to avoid scanning unknown QR codes

These practices reduce the effectiveness of modern phishing techniques.

