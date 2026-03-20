# Security Operations Center Playbook

## Incident Type

Impossible Travel (Geographic Anomaly Detection)

------------------------------------------------------------------------

# 1. Objective

This playbook provides SOC analysts with a structured approach to
investigate and respond to impossible travel alerts, where a user logs
in from geographically distant locations within an unrealistic time
frame.

Objectives:

-   Identify suspicious login activity
-   Detect potential credential compromise
-   Differentiate between benign and malicious behavior
-   Contain compromised accounts
-   Prevent unauthorized access

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Cloud identity platforms (Azure AD, Okta, Google Workspace)
-   VPN authentication systems
-   SaaS applications (Microsoft 365, Salesforce)
-   Remote access logs

------------------------------------------------------------------------

# 3. Threat Description

Impossible travel occurs when:

-   A user logs in from Location A
-   Shortly after logs in from Location B
-   Travel between locations is physically impossible in given time

Common causes:

-   Credential theft
-   Session hijacking
-   VPN/proxy usage
-   Shared accounts

------------------------------------------------------------------------

# 4. Severity Classification

Low: - Known VPN usage - Corporate proxy involved

Medium: - New location but plausible travel - No suspicious activity
post-login

High: - Multiple distant locations in short time - Unusual device or IP

Critical: - Confirmed malicious access - Data access or changes observed

------------------------------------------------------------------------

# 5. Escalation Criteria

Escalate if:

-   Suspicious login confirmed from foreign IP
-   Multiple impossible travel events detected
-   Privileged account involved
-   Data access or downloads observed
-   MFA bypass or failure noted

------------------------------------------------------------------------

# 6. Detection Sources

-   Azure AD Sign-in Logs
-   Okta System Logs
-   VPN logs
-   SIEM correlation alerts

------------------------------------------------------------------------

# 7. Sample Log Evidence

User: john.doe

Login 1: IP: 192.168.1.10\
Location: New York, USA\
Time: 10:00 AM

Login 2: IP: 45.67.23.11\
Location: Moscow, Russia\
Time: 10:20 AM

Travel time impossible → alert triggered

------------------------------------------------------------------------

# 8. Alert Triage

Collect:

-   Username
-   Source IPs
-   Locations
-   Timestamp
-   Device info
-   Authentication method

Key Questions:

-   Is VPN involved?
-   Is IP known corporate range?
-   Same device or different?
-   Any MFA used?

------------------------------------------------------------------------

# 9. Investigation Procedure

## Step 1 --- Validate Alert

-   Confirm login timestamps and locations
-   Calculate travel feasibility

------------------------------------------------------------------------

## Step 2 --- Check IP Reputation

-   Analyze IPs using threat intelligence
-   Identify proxy/VPN services

------------------------------------------------------------------------

## Step 3 --- Device & Session Analysis

-   Compare device IDs
-   Check session tokens

------------------------------------------------------------------------

## Step 4 --- Check User Behavior

-   Accessed resources?
-   File downloads?
-   Admin actions?

------------------------------------------------------------------------

## Step 5 --- Scope Analysis

Search for:

-   Similar activity across users
-   Same IP across accounts

------------------------------------------------------------------------

# 10. Detection Queries

## KQL

SigninLogs \| summarize count() by IPAddress, UserPrincipalName,
Location \| where count\_ \> 1

## Splunk

index=signin_logs \| stats count by user, src_ip, location

## Sigma (Concept)

title: Impossible Travel Detection detection: selection:
multiple_locations: true

------------------------------------------------------------------------

# 11. Containment

-   Force password reset
-   Revoke active sessions
-   Enforce MFA
-   Block suspicious IPs

------------------------------------------------------------------------

# 12. Eradication

-   Remove unauthorized access
-   Disable compromised tokens
-   Review permissions

------------------------------------------------------------------------

# 13. Recovery

-   Restore secure access
-   Monitor account activity
-   Validate user identity

------------------------------------------------------------------------

# 14. Post-Incident Actions

-   Update detection rules
-   Improve geo-anomaly detection
-   Educate users on credential safety

------------------------------------------------------------------------

# 15. Analyst Notes

In real scenarios, impossible travel alerts often result from:

-   VPN usage
-   Mobile device switching networks
-   Corporate proxy infrastructure

Key differentiators:

-   Same device + VPN → likely benign
-   Different device + foreign IP → suspicious
-   High-risk country + no MFA → high severity

------------------------------------------------------------------------

# 16. Response Timeline

T+0 min → Alert triggered\
T+5 min → Triage started\
T+10 min → Location analysis completed\
T+20 min → Account secured\
T+45 min → Incident documented

------------------------------------------------------------------------

# 17. MITRE ATT&CK Mapping

T1078 -- Valid Accounts

------------------------------------------------------------------------

# 18. References

-   NIST SP 800-61
-   MITRE ATT&CK Framework
-   Microsoft Identity Protection Docs
