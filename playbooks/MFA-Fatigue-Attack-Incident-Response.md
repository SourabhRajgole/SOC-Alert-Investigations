# Security Operations Center Playbook

## Incident Type

MFA Fatigue / Push Notification Bombing Attack

------------------------------------------------------------------------

# 1. Objective

This playbook provides SOC analysts with a structured investigation and
response procedure for **MFA Fatigue attacks**, also known as **push
notification bombing**.

Objectives:

-   Detect repeated MFA push requests targeting a user
-   Identify possible credential compromise
-   Prevent unauthorized authentication approval
-   Contain compromised user accounts
-   Improve identity protection controls

------------------------------------------------------------------------

# 2. Scope

This playbook applies to authentication platforms using push-based MFA,
including:

-   Microsoft 365 / Azure AD
-   Okta
-   Duo Security
-   Google Workspace
-   Enterprise VPN authentication
-   Cloud identity providers

Alerts may originate from:

-   Identity protection alerts
-   SIEM detection rules
-   Authentication monitoring systems
-   User reports of repeated MFA prompts

------------------------------------------------------------------------

# 3. Threat Description

MFA Fatigue attacks occur when an attacker repeatedly triggers MFA push
notifications to a victim in hopes that the user will eventually approve
the request.

Typical attack sequence:

1.  Attacker obtains valid credentials (phishing, password spraying,
    credential stuffing)
2.  Attacker attempts login repeatedly
3.  MFA push notifications are repeatedly sent to the user
4.  User eventually approves the request
5.  Attacker gains authenticated access

These attacks rely on **user fatigue or confusion**.

Real-world incidents have shown attackers sending **dozens or hundreds
of MFA prompts** until a user accidentally approves one.

------------------------------------------------------------------------

# 4. Detection Sources

Primary detection sources include:

-   Identity provider authentication logs
-   MFA authentication logs
-   Azure AD sign-in logs
-   Okta authentication logs
-   SIEM alerts
-   User reports

Key indicators include:

-   Large number of MFA push notifications in short time window
-   Repeated authentication attempts for same user
-   Login attempts from unusual IP addresses
-   Authentication attempts from unfamiliar locations

------------------------------------------------------------------------

# 5. Alert Triage

SOC analyst should collect:

-   Username
-   Source IP address
-   Login location
-   Number of MFA requests
-   Authentication timestamps
-   Device used for login

Example alert:

Alert: Excessive MFA Push Requests Detected\
User: jane.doe@company.com\
Attempts: 25 MFA requests in 10 minutes\
Source IP: 91.xxx.xxx.xxx

Initial triage questions:

-   Did the user approve any MFA requests?
-   Are login attempts coming from unfamiliar locations?
-   Does the IP belong to VPN or anonymization infrastructure?
-   Did the user report suspicious MFA prompts?

------------------------------------------------------------------------

# 6. Investigation Procedure

## Step 1 --- Review Authentication Logs

Search identity provider logs for activity related to the user.

Example SIEM query:

    user = "target_user"
    | stats count by source_ip, authentication_result

Look for:

-   Multiple MFA requests
-   Repeated login attempts
-   Authentication attempts from same IP

------------------------------------------------------------------------

## Step 2 --- Verify User Activity

Contact the affected user to confirm:

-   Whether they attempted login
-   Whether they approved any MFA request
-   Whether they received repeated push notifications

If the user confirms they did **not initiate the login**, assume
potential compromise.

------------------------------------------------------------------------

## Step 3 --- Investigate Source IP

Analyze the source IP address.

Check:

-   IP geolocation
-   Threat intelligence reputation
-   VPN or proxy infrastructure

Use threat intelligence sources such as:

-   VirusTotal
-   AbuseIPDB
-   GreyNoise

------------------------------------------------------------------------

## Step 4 --- Investigate Post-Authentication Activity

If the attacker successfully authenticated, review:

-   File access
-   Email activity
-   Cloud console activity
-   Administrative actions

Check for:

-   Privilege escalation
-   Data access or downloads
-   Creation of persistence mechanisms

------------------------------------------------------------------------

# 7. Containment

If MFA fatigue attack is confirmed:

Immediate actions:

-   Reset user password
-   Revoke active sessions
-   Revoke authentication tokens
-   Temporarily disable account if necessary

Block malicious IP addresses where applicable.

------------------------------------------------------------------------

# 8. Eradication

Remove attacker persistence.

Actions include:

-   Remove unauthorized OAuth applications
-   Remove malicious mailbox rules
-   Remove suspicious API tokens
-   Audit privileged access

------------------------------------------------------------------------

# 9. Recovery

Restore secure user access.

Recommended improvements:

-   Enable number matching MFA
-   Enable location-based authentication checks
-   Implement risk-based conditional access policies
-   Implement MFA prompt limits

Continue monitoring authentication logs for suspicious activity.

------------------------------------------------------------------------

# 10. Post-Incident Activities

Conduct post-incident review.

Tasks include:

-   Root cause analysis
-   Review identity monitoring rules
-   Update detection thresholds
-   Provide user security awareness training

------------------------------------------------------------------------

# 11. Roles and Responsibilities

SOC Analyst

-   Investigate authentication anomalies
-   Validate user activity
-   Document investigation findings

Incident Response Team

-   Contain compromised accounts
-   Coordinate remediation actions

Security Engineering

-   Implement stronger authentication protections
-   Improve monitoring rules

------------------------------------------------------------------------

# 12. MITRE ATT&CK Mapping

Credential Access\
T1110 -- Brute Force

Initial Access\
T1078 -- Valid Accounts

Defense Evasion\
T1621 -- Multi-Factor Authentication Request Generation

------------------------------------------------------------------------

# 13. References

-   NIST SP 800-61 Incident Handling Guide
-   MITRE ATT&CK Framework
-   Microsoft Identity Security Documentation
-   SANS Incident Handler Handbook
