# Security Operations Center Playbook

## Incident Type

Password Spraying Attack (Credential Abuse)

------------------------------------------------------------------------

# 1. Objective

This playbook provides SOC analysts with a structured approach to
detect, investigate, and respond to password spraying attacks.

Objectives:

-   Identify distributed authentication attempts
-   Detect compromised accounts early
-   Differentiate from brute force attacks
-   Contain attacker activity
-   Strengthen authentication defenses

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Active Directory / Windows logs
-   Cloud identity (Azure AD, Okta)
-   VPN authentication systems
-   Web applications

------------------------------------------------------------------------

# 3. Threat Description

Password spraying is an attack where:

-   A single password is tried across multiple accounts
-   Attackers avoid lockouts by limiting attempts per account

Typical pattern:

-   One IP → many users → same password
-   Slow and distributed attempts

------------------------------------------------------------------------

# 4. Severity Classification

Low: - Few failed attempts - No pattern

Medium: - Multiple users targeted - No successful login

High: - Successful login detected - Suspicious IP

Critical: - Multiple accounts compromised - Privileged accounts affected

------------------------------------------------------------------------

# 5. Escalation Criteria

Escalate if:

-   Successful login observed
-   Privileged account targeted
-   Same IP targeting many users
-   Repeated activity over time
-   External IP involved

------------------------------------------------------------------------

# 6. Detection Sources

-   Windows Event Logs (4625)
-   Azure AD Sign-in logs
-   VPN logs
-   SIEM alerts

------------------------------------------------------------------------

# 7. Sample Log Evidence

EventID: 4625\
Users targeted: user1, user2, user3\
Source IP: 185.22.11.90\
Failure Reason: Bad password

Pattern:

-   Same password attempt across multiple users
-   1 attempt per user every few minutes

------------------------------------------------------------------------

# 8. Alert Triage

Collect:

-   Source IP
-   List of usernames
-   Number of attempts
-   Time window
-   Authentication system

Key Questions:

-   Same IP targeting many users?
-   Any successful login?
-   Is IP external?
-   Is pattern slow and distributed?

------------------------------------------------------------------------

# 9. Investigation Procedure

## Step 1 --- Validate Pattern

-   Identify one password used across multiple users
-   Confirm distributed attack behavior

------------------------------------------------------------------------

## Step 2 --- Check for Success

Look for:

Event ID 4624 (successful login)

------------------------------------------------------------------------

## Step 3 --- IP Reputation

-   Check IP in threat intelligence sources
-   Identify known malicious infrastructure

------------------------------------------------------------------------

## Step 4 --- Scope Analysis

Search across logs:

index=auth_logs EventCode=4625 \| stats count by src_ip, user

------------------------------------------------------------------------

## Step 5 --- Correlation

-   VPN logs
-   Cloud login logs
-   Endpoint logs

------------------------------------------------------------------------

# 10. Detection Queries

## Splunk

index=windows EventCode=4625 \| stats count by src_ip, user \| where
count \> 5

## KQL

SigninLogs \| summarize count() by IPAddress, UserPrincipalName \| where
count\_ \> 5

## Sigma (Concept)

title: Password Spraying Detection detection: selection: EventID: 4625
condition: selection

------------------------------------------------------------------------

# 11. Containment

-   Block malicious IP
-   Enforce MFA
-   Monitor affected accounts
-   Apply rate limiting

------------------------------------------------------------------------

# 12. Eradication

-   Reset affected passwords
-   Remove unauthorized access
-   Audit authentication logs

------------------------------------------------------------------------

# 13. Recovery

-   Restore user access
-   Monitor login activity
-   Strengthen password policies

------------------------------------------------------------------------

# 14. Post-Incident Actions

-   Enforce strong password policy
-   Enable MFA for all users
-   Update detection rules
-   Conduct user awareness training

------------------------------------------------------------------------

# 15. Analyst Notes

In real SOC investigations, password spraying is often confused with
brute force.

Key difference:

-   Brute force → many passwords, one user
-   Password spraying → one password, many users

Attackers use this method to avoid account lockouts.

------------------------------------------------------------------------

# 16. Response Timeline

T+0 min → Alert triggered\
T+5 min → Pattern identified\
T+10 min → Scope analyzed\
T+15 min → IP blocked\
T+30 min → Accounts secured

------------------------------------------------------------------------

# 17. MITRE ATT&CK Mapping

T1110.003 -- Password Spraying

------------------------------------------------------------------------

# 18. References

-   NIST SP 800-61
-   MITRE ATT&CK Framework
-   SANS Incident Handler Handbook
