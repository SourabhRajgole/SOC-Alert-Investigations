# Security Operations Center Playbook

## Incident Type

Password Spraying Attack

------------------------------------------------------------------------

# 1. Objective

This playbook provides a structured investigation and response procedure
for password spraying attacks detected within authentication
infrastructure.

Objectives: - Detect credential spraying attempts early - Identify
compromised accounts - Contain unauthorized access - Restore secure
authentication operations - Improve future detection capabilities

------------------------------------------------------------------------

# 2. Scope

Applies to authentication systems including:

-   Active Directory
-   Microsoft 365 / Azure AD
-   VPN authentication portals
-   Web application logins
-   Cloud identity providers
-   RDP authentication
-   SSH authentication services

------------------------------------------------------------------------

# 3. Threat Description

Password spraying is a credential attack technique where attackers
attempt a small set of commonly used passwords across many user
accounts.

Typical attacker workflow:

1.  Collect valid usernames
2.  Attempt common passwords across multiple accounts
3.  Spread attempts to avoid lockouts
4.  Gain access to one or more accounts
5.  Use valid credentials for persistence or lateral movement

Example commonly used passwords:

    Spring2025!
    Welcome123
    Password123
    CompanyName2025
    Winter2024!

------------------------------------------------------------------------

# 4. Detection Sources

Primary log sources:

-   Active Directory authentication logs
-   Azure AD sign‑in logs
-   VPN authentication logs
-   Firewall authentication logs
-   EDR alerts
-   SIEM correlation alerts

Common indicators:

-   Multiple authentication failures across many accounts
-   Same IP attempting login against many usernames
-   Authentication attempts from unusual geographic locations
-   Repeated login attempts with common password patterns

------------------------------------------------------------------------

# 5. Alert Triage

SOC analyst should collect:

-   Source IP
-   Target usernames
-   Timestamp range
-   Authentication protocol
-   Login success / failure
-   Geographic source

Example alert:

Alert: Multiple authentication failures detected across 30 user
accounts\
Source IP: 185.xxx.xxx.xxx\
Protocol: VPN Authentication\
Attempts: 150 login attempts within 10 minutes

Initial triage questions:

-   Are multiple accounts targeted?
-   Is the same IP attempting multiple logins?
-   Are attempts spaced to avoid lockout?
-   Did any login succeed?

------------------------------------------------------------------------

# 6. Investigation Procedure

## Step 1 --- Identify Source of Attack

Search authentication logs for attacker IP.

Example SIEM query:

    source_ip = attacker_IP
    | stats count by username

Indicators:

-   Many usernames targeted
-   Few attempts per account
-   Same IP repeating across accounts

------------------------------------------------------------------------

## Step 2 --- Identify Targeted Accounts

Determine if targeted users include:

-   Administrators
-   IT personnel
-   Service accounts
-   High‑privilege users

Prioritize investigation if privileged accounts are involved.

------------------------------------------------------------------------

## Step 3 --- Identify Successful Logins

Relevant Windows Event IDs:

    4624 – Successful login
    4625 – Failed login

If a login succeeded:

Investigate:

-   Login location
-   Device used
-   Login time

Indicators of compromise:

-   Login from unusual country
-   Login from previously unseen IP
-   Login outside working hours

------------------------------------------------------------------------

## Step 4 --- Investigate Post‑Authentication Activity

If compromise occurred review:

-   Endpoint activity
-   Privilege escalation attempts
-   Network connections
-   Access to sensitive systems

Suspicious commands to review:

    whoami
    net user
    net localgroup administrators
    powershell -enc

Also check:

-   Mailbox forwarding rules
-   Data downloads
-   Cloud console activity

------------------------------------------------------------------------

# 7. Containment

If password spraying confirmed:

Immediate actions:

-   Block attacker IP on firewall
-   Block IP on VPN gateway
-   Enforce temporary login restrictions

If accounts compromised:

-   Force password reset
-   Revoke active sessions
-   Disable accounts temporarily if needed

------------------------------------------------------------------------

# 8. Eradication

Remove attacker persistence.

Actions:

-   Reset affected credentials
-   Disable legacy authentication protocols
-   Remove unauthorized tokens
-   Review privileged account access

------------------------------------------------------------------------

# 9. Recovery

Restore secure operations.

Recommended improvements:

-   Enable Multi‑Factor Authentication (MFA)
-   Implement stronger password policies
-   Configure account lockout thresholds
-   Improve authentication monitoring

Continue monitoring logs for repeated attempts.

------------------------------------------------------------------------

# 10. Post‑Incident Activities

Conduct a post‑incident review:

-   Root cause analysis
-   Detection rule evaluation
-   Password policy review
-   User awareness improvements

Lessons learned should be incorporated into future playbooks.

------------------------------------------------------------------------

# 11. Roles and Responsibilities

SOC Analyst - Investigate authentication alerts - Collect evidence -
Determine attack scope

Incident Responder - Contain compromised systems - Coordinate
remediation actions

Security Engineering - Improve detection rules - Update monitoring
controls

------------------------------------------------------------------------

# 12. MITRE ATT&CK Mapping

Credential Access\
T1110.003 --- Password Spraying

Initial Access\
T1078 --- Valid Accounts

Discovery\
T1087 --- Account Discovery

------------------------------------------------------------------------

# 13. References

-   NIST SP 800‑61 Incident Handling Guide
-   MITRE ATT&CK Framework
-   SANS Incident Handler Handbook
