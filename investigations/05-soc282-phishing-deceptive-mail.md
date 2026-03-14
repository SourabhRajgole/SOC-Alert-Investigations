
# SOC Investigation 05 — Phishing Alert: Deceptive Mail Detected

## Alert Information

| Field | Value |
|------|------|
| Event ID | 257 |
| Rule Name | SOC282 – Phishing Alert - Deceptive Mail Detected |
| Severity | Medium |
| Event Type | Email Security |
| Detection Platform | LetsDefend SIEM |
| Hostname | Felix |
| Host IP | 172.16.20.151 |
| SMTP IP | 103.80.134.63 |
| Classification | True Positive |

---

## Alert Description

A phishing alert was triggered after a suspicious email was detected by the rule **SOC282 – Phishing Alert - Deceptive Mail Detected**.

The email was sent to **felix@letsdefend.io** from **free@coffeeshoop.com** with the subject **“Free Coffee Voucher”**.  
The message contained phishing-style language designed to pressure the user such as *“Hurry”* and *“This offer expires soon”*.

The device action was **Allowed**, meaning the email security system did not block the message and it was successfully delivered to the user's mailbox.

---

## Detection and Initial Triage

The investigation began by reviewing the email metadata and parsing the alert details.

Key information identified:

- Email Sent Time: May 13, 2024, 09:22 AM
- Sender Email: free@coffeeshoop.com
- Recipient Email: felix@letsdefend.io
- SMTP IP: 103.80.134.63
- Email Subject: Free Coffee Voucher

Initial review of the email content revealed suspicious characteristics commonly used in phishing campaigns such as urgency and promotional offers designed to lure the user.

The email contained a suspicious download link which required further analysis.

---

## Log Analysis

Email security logs confirmed that the message was delivered to the user.

The email contained the following malicious link:

https://files-ld.s3.us-east-2.amazonaws[.]com/59cbd215-76ea-434d-93ca-4d6aec3bac98-free-coffee.zip

After reviewing proxy logs associated with the user host **172.16.20.151**, it was confirmed that the user accessed the malicious URL.

The downloaded file executed **coffee.exe**, which initiated further malicious activity on the system.

---

## Threat Intelligence / Reputation Analysis

The suspicious URL was analyzed using threat intelligence platforms such as **VirusTotal**.

Results showed that multiple security engines flagged the URL as malicious.

Further malware analysis using sandboxing tools indicated that the downloaded file was associated with an **AsyncRAT variant**, which is commonly used for remote access and command-and-control communication.

---

## Investigation Findings

The investigation identified the following malicious behavior:

- The phishing email successfully reached the user.
- The user accessed the malicious URL.
- The malicious file **coffee.exe** was executed on the system.
- The malware established communication with a command-and-control server.

The C2 communication was observed with the following address:

37.120.233.226

This confirms that the host system was compromised after the phishing attack.

---

## Indicators of Compromise (IOCs)

| Type | Indicator |
|-----|-----------|
| Malicious URL | https://files-ld.s3.us-east-2.amazonaws[.]com/59cbd215-76ea-434d-93ca-4d6aec3bac98-free-coffee.zip |
| SMTP IP | 103.80.134.63 |
| C2 IP | 37.120.233.226 |
| Malware | coffee.exe |
| File Hash | CD903AD2211CF7D166646D75E57FB866000F4A3B870B5EC759929BE2FD81D334 |

---

## MITRE ATT&CK Mapping

| Technique | Description |
|----------|-------------|
| T1566 | Phishing |
| T1059 | Command and Scripting Interpreter |
| T1204 | User Execution |
| T1087 | Account Discovery |
| T1007 | System Service Discovery |

---

## Investigation Conclusion

The alert was classified as a **True Positive**.

The phishing email successfully deceived the user, leading to the execution of malicious software on the host system.  
The malware established communication with a command-and-control server, confirming system compromise.

Immediate containment and remediation actions were required to prevent further damage.

---

## Recommended Response Actions

Recommended response steps:

- Isolate the infected host from the network
- Remove the malicious email from user mailboxes
- Reset compromised user credentials
- Block malicious IP and URL indicators
- Perform malware removal and forensic investigation

---

## Lessons Learned

Phishing attacks often use promotional messages or urgent offers to trick users into clicking malicious links.

Security teams should implement:

- stronger email filtering
- user awareness training
- sandbox analysis for attachments and URLs

These measures help reduce the success rate of phishing attacks.

