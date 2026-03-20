# Security Operations Center Playbook

## Incident Type

MFA Fatigue Attack (Push Notification Abuse)

------------------------------------------------------------------------

# 1. Objective

This playbook provides SOC analysts with a structured approach to detect
and respond to MFA fatigue attacks, where attackers attempt to gain
access by overwhelming users with repeated MFA push notifications.

Objectives:

-   Detect suspicious MFA push patterns
-   Identify compromised credentials
-   Prevent unauthorized access
-   Contain and secure affected accounts

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Cloud identity platforms (Azure AD, Okta, Duo)
-   VPN authentication systems
-   SaaS applications with MFA enabled

------------------------------------------------------------------------

# 3. Threat Description

MFA fatigue attacks occur when:

-   Attacker has valid username/password
-   Sends repeated MFA push requests
-   User eventually accepts request (fatigue or confusion)

Common characteristics:

-   Multiple MFA prompts in short time
-   Login attempts from unfamiliar locations
-   Off-hours authentication attempts

------------------------------------------------------------------------

# 4. Severity Classification

Low: - Few MFA denials - Known user testing

Medium: - Multiple MFA requests - No approval

High: - MFA approved after repeated requests - Suspicious IP or device

Critical: - Confirmed unauthorized access - Privileged account
compromise

------------------------------------------------------------------------

# 5. Escalation Criteria

Escalate if:

-   MFA approval follows repeated push attempts
-   Suspicious IP or location detected
-   Privileged account targeted
-   Multiple users targeted from same IP
-   User reports unexpected MFA prompts

------------------------------------------------------------------------

# 6. Detection Sources

-   Azure AD Sign-in Logs
-   Okta System Logs
-   Duo MFA logs
-   SIEM correlation alerts

------------------------------------------------------------------------

# 7. Sample Log Evidence

User: jane.doe

Attempts: 10 MFA push requests within 5 minutes

Final Event: MFA Approved

IP: 203.45.12.67\
Location: Unknown/Foreign

------------------------------------------------------------------------

# 8. Alert Triage

Collect:

-   Username
-   Number of MFA attempts
-   IP address
-   Location
-   Device info
-   MFA result (approved/denied)

Key Questions:

-   Did user approve MFA?
-   Is location unusual?
-   Is device recognized?
-   Any prior suspicious activity?

------------------------------------------------------------------------

# 9. Investigation Procedure

## Step 1 --- Validate Alert

-   Confirm number of MFA attempts
-   Identify approval/denial pattern

------------------------------------------------------------------------

## Step 2 --- Analyze Login Context

-   Check IP reputation
-   Identify geolocation anomalies

------------------------------------------------------------------------

## Step 3 --- Device & Session Review

-   Compare device ID
-   Check session creation after MFA approval

------------------------------------------------------------------------

## Step 4 --- User Confirmation

-   Contact user
-   Verify if MFA approval was legitimate

------------------------------------------------------------------------

## Step 5 --- Scope Analysis

Search for:

-   Same IP targeting multiple users
-   Repeated MFA fatigue patterns

------------------------------------------------------------------------

# 10. Detection Queries

## KQL

SigninLogs \| where AuthenticationRequirement ==
"multiFactorAuthentication" \| summarize count() by UserPrincipalName,
IPAddress \| where count\_ \> 5

## Splunk

index=auth_logs mfa_status=denied \| stats count by user, src_ip \|
where count \> 5

## Sigma (Concept)

title: MFA Fatigue Attack detection: selection: mfa_requests: high
condition: selection

------------------------------------------------------------------------

# 11. Containment

-   Block suspicious IP
-   Force password reset
-   Revoke active sessions
-   Require MFA re-registration

------------------------------------------------------------------------

# 12. Eradication

-   Remove unauthorized sessions
-   Reset credentials
-   Review account permissions

------------------------------------------------------------------------

# 13. Recovery

-   Confirm secure login
-   Monitor account activity
-   Reinforce MFA policies

------------------------------------------------------------------------

# 14. Post-Incident Actions

-   Enable number matching MFA
-   Limit push notifications
-   Train users to report unexpected MFA prompts
-   Update detection rules

------------------------------------------------------------------------

# 15. Analyst Notes

In real SOC environments, MFA fatigue attacks are increasingly common.

Key observations:

-   Users often approve MFA accidentally
-   Attackers target users during off-hours
-   Push-based MFA is weaker than number matching

Best practice:

-   Always verify with user before closing alert
-   Treat MFA approval after multiple pushes as HIGH risk

------------------------------------------------------------------------

# 16. Response Timeline

T+0 min → Alert triggered\
T+5 min → MFA pattern identified\
T+10 min → User contacted\
T+15 min → Account secured\
T+30 min → Sessions revoked

------------------------------------------------------------------------

# 17. MITRE ATT&CK Mapping

T1621 -- Multi-Factor Authentication Request Generation

------------------------------------------------------------------------

# 18. References

-   NIST SP 800-61
-   MITRE ATT&CK Framework
-   Microsoft Identity Protection
