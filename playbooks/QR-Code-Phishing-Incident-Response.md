#  Security Operations Center Playbook

## Incident Type

QR Code Phishing (Quishing)

------------------------------------------------------------------------

# 1. Objective

This playbook provides a structured approach to detect, investigate, and
respond to QR code phishing attacks where users are redirected to
malicious sites via scanned QR codes.

Objectives:

-   Identify malicious QR-based phishing attempts
-   Analyze redirection and payload behavior
-   Determine user interaction and impact
-   Contain potential compromise
-   Prevent similar attacks

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Email platforms (Microsoft 365, Gmail)
-   Mobile devices and endpoints
-   Web gateways and proxy logs
-   SIEM and EDR telemetry

------------------------------------------------------------------------

# 3. Threat Description

QR phishing (Quishing) involves embedding malicious URLs inside QR codes
to bypass traditional email security filters.

Attack flow:

1.  User receives QR code via email/document
2.  User scans QR code (usually on mobile device)
3.  Redirected to phishing page
4.  Credentials or sensitive data captured

Common characteristics:

-   QR codes in PDFs or images
-   Shortened or obfuscated URLs
-   Fake login portals (Microsoft, Google, banking)

------------------------------------------------------------------------

# 4. Severity Classification

Low: - QR code detected but not scanned

Medium: - QR scanned but no credential entry

High: - Credential entry suspected

Critical: - Confirmed credential compromise - Multiple users affected

------------------------------------------------------------------------

# 5. Escalation Criteria

Escalate if:

-   User scanned QR and accessed suspicious site
-   Credentials entered
-   Multiple users received same QR campaign
-   Domain impersonates trusted service

------------------------------------------------------------------------

# 6. Detection Sources

-   Email security alerts
-   User-reported suspicious emails
-   Web proxy logs (mobile traffic)
-   SIEM alerts for suspicious domains

------------------------------------------------------------------------

# 7. Sample Evidence

Email contains:

QR Code → hxxps://secure-login-microsoft\[.\]com

Observed behavior:

-   QR leads to fake login page
-   Domain recently registered
-   No SSL reputation

------------------------------------------------------------------------

# 8. Alert Triage

Collect:

-   Recipient email
-   QR code image/file
-   Embedded URL
-   Timestamp
-   User interaction status

Key Questions:

-   Did the user scan the QR code?
-   Was any data entered?
-   Is domain malicious or newly registered?
-   Are multiple users impacted?

------------------------------------------------------------------------

# 9. Investigation Procedure

## Step 1 --- Extract QR Content

-   Decode QR code
-   Identify embedded URL

## Step 2 --- URL Analysis

-   Check domain reputation (VirusTotal)
-   Analyze redirects
-   Identify phishing page

## Step 3 --- User Interaction Check

-   Confirm if QR was scanned
-   Verify credential submission

## Step 4 --- Endpoint / Mobile Analysis

-   Check browser activity
-   Review login attempts

## Step 5 --- Scope Analysis

Search for similar emails or domains across environment

------------------------------------------------------------------------

# 10. Detection Queries

## Splunk

index=email "QR" \| stats count by sender, recipient

## KQL

EmailEvents \| where Subject contains "scan" \| summarize count() by
SenderFromAddress, RecipientEmailAddress

------------------------------------------------------------------------

# 11. Containment

-   Block malicious domain
-   Remove email from all users
-   Disable compromised accounts
-   Enforce MFA

------------------------------------------------------------------------

# 12. Eradication

-   Remove phishing emails
-   Reset credentials
-   Clear browser sessions

------------------------------------------------------------------------

# 13. Recovery

-   Restore user access
-   Monitor login activity
-   Strengthen email filtering

------------------------------------------------------------------------

# 14. Post-Incident Actions

-   Improve QR detection rules
-   Conduct user awareness training
-   Block similar domains
-   Update threat intelligence

------------------------------------------------------------------------

# 15. Analyst Notes

QR phishing is harder to detect because:

-   It bypasses URL scanning tools
-   Users scan using personal devices
-   Redirection happens outside corporate network

Key indicators:

-   Newly registered domains
-   Login pages mimicking Microsoft/Google
-   Mobile-based authentication attempts

------------------------------------------------------------------------

# 16. Response Timeline

T+0 min → Alert triggered T+5 min → QR decoded T+10 min → URL analyzed
T+15 min → Domain blocked T+30 min → Users notified

------------------------------------------------------------------------

# 17. MITRE ATT&CK

T1566 --- Phishing

------------------------------------------------------------------------

# 18. References

-   NIST SP 800-61
-   MITRE ATT&CK Framework
