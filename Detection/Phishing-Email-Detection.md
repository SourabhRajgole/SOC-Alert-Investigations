# 🔍 Detection Rule: Phishing Email Detection

------------------------------------------------------------------------

## 1. Objective

Detect potentially malicious phishing emails by identifying suspicious
sender characteristics, email content, and embedded URLs or attachments.

------------------------------------------------------------------------

## 2. Log Sources

-   Email Security Gateway Logs (Microsoft Defender for Office 365,
    Proofpoint, Mimecast)
-   Exchange / Microsoft 365 Email Logs
-   SIEM (email ingestion logs)
-   Web Proxy Logs (URL click tracking)

------------------------------------------------------------------------

## 3. Detection Logic

Trigger an alert when one or more of the following conditions are met:

-   Email contains suspicious keywords (urgent, verify, invoice,
    password reset)
-   Sender domain is newly registered or resembles a known domain
    (typosquatting)
-   SPF/DKIM/DMARC validation fails
-   Email contains external links flagged as malicious or suspicious
-   Attachment includes executable or macro-enabled files (.exe, .zip,
    .docm, .xlsm)

------------------------------------------------------------------------

## 4. Detection Queries

### Splunk

    index=email_logs
    | search subject="urgent" OR subject="verify" OR subject="invoice"
    | stats count by sender, recipient, subject

------------------------------------------------------------------------

### KQL (Microsoft Sentinel / Defender)

    EmailEvents
    | where Subject has_any ("urgent", "verify", "invoice", "password")
    | summarize count() by SenderFromAddress, RecipientEmailAddress, Subject

------------------------------------------------------------------------

### URL-Based Detection (KQL)

    EmailUrlInfo
    | where UrlDomain !contains "trusted-domain.com"
    | summarize count() by UrlDomain, RecipientEmailAddress

------------------------------------------------------------------------

## 5. Tuning & Thresholds

-   Adjust keyword filters based on organization usage
-   Exclude known trusted domains and internal senders
-   Allowlist legitimate bulk email senders
-   Tune for high-risk departments (finance, HR)

------------------------------------------------------------------------

## 6. False Positives

-   Legitimate urgent business emails
-   Marketing campaigns
-   Internal system notifications
-   Security awareness phishing simulations

------------------------------------------------------------------------

## 7. Severity

-   Low: Suspicious email, no interaction
-   Medium: Email delivered with suspicious indicators
-   High: User clicked link or downloaded attachment
-   Critical: Credential submission or malware execution confirmed

------------------------------------------------------------------------

## 8. MITRE ATT&CK Mapping

-   T1566 --- Phishing\
-   T1566.001 --- Spear Phishing Attachment\
-   T1566.002 --- Spear Phishing Link

------------------------------------------------------------------------

## 9. Analyst Notes

Phishing is one of the most common alerts in SOC environments.

Key indicators:

-   Lookalike domains
-   External sender impersonation
-   Unexpected attachments or login prompts

------------------------------------------------------------------------

## 10. Recommended Response

-   Block sender domain and IP
-   Quarantine emails across all users
-   Block malicious URLs
-   Reset credentials if needed
-   Enforce MFA

------------------------------------------------------------------------

## 11. Improvements

-   Add domain age checks
-   Integrate URL sandboxing
-   Correlate with user click activity
