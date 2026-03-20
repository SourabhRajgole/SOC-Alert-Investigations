#  Security Operations Center Playbook

## Incident Type

Phishing (Standard, Spear-Phishing, Attachment-Based)

------------------------------------------------------------------------

# 1. Objective

This playbook provides a structured approach to detect, investigate, and
respond to phishing attacks targeting users through email-based social
engineering.

Objectives:

-   Identify phishing attempts and malicious intent
-   Determine user interaction and impact
-   Analyze email artifacts and payloads
-   Contain potential compromise
-   Prevent further attacks

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Email platforms (Microsoft 365, Gmail, Exchange)
-   End-user endpoints
-   Email security gateways
-   SIEM and EDR telemetry

------------------------------------------------------------------------

# 3. Threat Description

Phishing attacks attempt to trick users into:

-   Clicking malicious links
-   Downloading malware attachments
-   Entering credentials into fake login pages

Types:

-   Standard phishing → mass campaigns
-   Spear phishing → targeted users
-   Attachment-based phishing → malware delivery

------------------------------------------------------------------------

# 4. Severity Classification

Low: - No user interaction - Known spam/phishing template

Medium: - Email delivered to user inbox - Suspicious indicators present

High: - User clicked link or opened attachment - Credential submission
suspected

Critical: - Credentials confirmed compromised - Malware executed or
multiple users affected

------------------------------------------------------------------------

# 5. Escalation Criteria

Escalate if:

-   User clicked phishing link
-   Credentials entered on suspicious site
-   Attachment executed
-   Multiple users received same email
-   Email impersonates internal executive (BEC risk)

------------------------------------------------------------------------

# 6. Detection Sources

-   Email security gateway alerts
-   User-reported phishing emails
-   SIEM alerts (URL clicks, downloads)
-   EDR telemetry (attachment execution)

------------------------------------------------------------------------

# 7. Sample Evidence

From: support@micros0ft-security.com Reply-To: attacker@gmail.com SPF:
Fail DKIM: Fail

URL: hxxps://login-microsoft-secure\[.\]com

Attachment: invoice_2026.zip → payload.exe

------------------------------------------------------------------------

# 8. Alert Triage

Collect:

-   Recipient email
-   Sender email/domain
-   Email subject
-   Timestamp
-   Links and attachments
-   User interaction status

------------------------------------------------------------------------

# 9. Investigation Procedure

## Step 1 --- Analyze Email Header

-   Check SPF, DKIM, DMARC
-   Identify spoofed domains

## Step 2 --- URL Analysis

-   Check domain reputation
-   Identify lookalike domains

## Step 3 --- Attachment Analysis

-   Identify file type
-   Check hash reputation

## Step 4 --- User Interaction Check

-   Verify link clicks
-   Confirm credential entry

## Step 5 --- Endpoint Analysis

-   Review EDR logs
-   Identify suspicious processes

## Step 6 --- Scope Analysis

Search for similar emails across environment

------------------------------------------------------------------------

# 10. Detection Queries

## Splunk

index=email subject="urgent" \| stats count by sender, recipient

## KQL

EmailEvents \| where Subject contains "urgent" \| summarize count() by
SenderFromAddress, RecipientEmailAddress

------------------------------------------------------------------------

# 11. Containment

-   Block sender domain
-   Quarantine emails
-   Block malicious URLs
-   Isolate endpoint if needed

------------------------------------------------------------------------

# 12. Eradication

-   Remove emails
-   Delete malicious files
-   Reset credentials

------------------------------------------------------------------------

# 13. Recovery

-   Monitor user accounts
-   Reinforce email security

------------------------------------------------------------------------

# 14. Post-Incident

-   Update rules
-   Conduct awareness training

------------------------------------------------------------------------

# 15. Analyst Notes

Most phishing alerts are low risk unless user interaction occurs.

-   No click → low risk
-   Click → medium/high risk
-   Credential submission → critical

------------------------------------------------------------------------

# 16. Response Timeline

T+0 min → Alert triggered T+5 min → Email analyzed T+10 min → User
verified T+15 min → Domain blocked

------------------------------------------------------------------------

# 17. MITRE ATT&CK

T1566 --- Phishing

------------------------------------------------------------------------

# 18. References

-   NIST SP 800-61
-   MITRE ATT&CK
