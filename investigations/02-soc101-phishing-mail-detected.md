# SOC Investigation 02 --- Phishing Mail Detected

## Alert Information

  Field                Value
  -------------------- ----------------------------------
  Event ID             8
  Rule Name            SOC101 -- Phishing Mail Detected
  Severity             Low
  Event Type           Email Security
  Detection Platform   LetsDefend SIEM
  Mail Server          Exchange
  Recipient            mark@letsdefend.io
  Classification       True Positive

------------------------------------------------------------------------

## Alert Description

The SIEM generated an alert indicating a potentially malicious email
detected by the email security monitoring system. The alert rule
**SOC101 -- Phishing Mail Detected** triggered due to suspicious
characteristics identified in the incoming email.

Phishing attacks often attempt to trick users into opening malicious
attachments or clicking harmful links. In this case, the email appeared
to be related to a **shipping notification**, but it included an
attachment that required further investigation.

------------------------------------------------------------------------

## Detection and Initial Triage

During the investigation, the following verification steps were
performed:

1.  Reviewed the SIEM detection rule **SOC101**.
2.  Verified whether the email was successfully delivered to the
    recipient.
3.  Checked for the presence of suspicious URLs or attachments in the
    email.
4.  Analyzed the attachment using threat intelligence platforms.
5.  Confirmed whether the recipient interacted with the email.

The alert details showed that the email was **allowed and delivered** to
the user mailbox.

------------------------------------------------------------------------

## Email Details

Event Time

    Aug 29, 2020, 11:05 PM

SMTP Address

    63.35.133.186

Sender Address

    info@nexoiberica.com

Recipient Address

    mark@letsdefend.io

Email Subject

    UPS Express

Device Action

    Allowed

------------------------------------------------------------------------

## Email Content Analysis

The email subject **"UPS Express"** resembles typical phishing campaigns
that impersonate shipping companies. Attackers frequently use delivery
notifications to convince users to open attachments or download files.

Although the email content did not contain obvious phishing keywords, it
contained an **attachment**, which raised suspicion and required further
analysis.

------------------------------------------------------------------------

## Attachment Analysis

The attachment included in the email was analyzed using malware
detection platforms such as **VirusTotal**.

The analysis results showed that the file was **flagged as malicious by
multiple security vendors**.

This indicates that the email was part of a phishing campaign intended
to deliver malware to the recipient's system.

------------------------------------------------------------------------

## User Interaction Analysis

User activity logs were reviewed to determine whether the recipient
interacted with the email.

The investigation confirmed:

-   The user **did not open the attachment**
-   The user **did not click any links**
-   The user **did not reply to the email**

Since the user did not interact with the malicious attachment, the
malware was not executed.

------------------------------------------------------------------------

## Indicators of Compromise (IOCs)

  Type                  Indicator
  --------------------- -------------------------------
  SMTP IP               63.35.133.186
  Sender Email          info@nexoiberica.com
  Recipient Email       mark@letsdefend.io
  Email Subject         UPS Express
  Malicious Component   Attachment flagged as malware

------------------------------------------------------------------------

## MITRE ATT&CK Mapping

  Technique   Description
  ----------- --------------------------
  T1566       Phishing
  T1566.001   Spearphishing Attachment

------------------------------------------------------------------------

## Investigation Conclusion

The alert was determined to be a **True Positive**.

Although the email did not contain strong phishing language, the
presence of a malicious attachment confirmed that the message was part
of a phishing campaign.

Fortunately, the recipient **did not interact with the attachment**,
preventing malware execution and system compromise.

------------------------------------------------------------------------

## Recommended Response Actions

If this incident occurred in a production environment, the following
actions would be recommended:

-   Remove the malicious email from user mailboxes
-   Block the sender email address
-   Block the SMTP source IP address
-   Scan the mail environment for similar emails
-   Conduct user awareness training on phishing attacks

------------------------------------------------------------------------

## Lessons Learned

Phishing campaigns often disguise themselves as legitimate service
notifications such as shipping updates or invoices. Even when the email
content appears harmless, attachments may contain malicious payloads.

Organizations should deploy strong email filtering, attachment scanning,
and phishing awareness programs to reduce the risk of such attacks.
