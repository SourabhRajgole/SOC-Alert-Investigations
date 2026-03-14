# Security Operations Center Playbook

## Incident Type

Brute Force Authentication Attack

------------------------------------------------------------------------

# 1. Objective

This playbook provides SOC analysts with a structured process for
detecting, investigating, and responding to brute force authentication
attacks.

Objectives:

-   Identify repeated login attempts targeting a single account
-   Determine whether the attacker successfully authenticated
-   Contain unauthorized access attempts
-   Protect exposed authentication services
-   Strengthen monitoring and authentication controls

------------------------------------------------------------------------

# 2. Scope

This playbook applies to authentication services including:

-   Remote Desktop Protocol (RDP)
-   Secure Shell (SSH)
-   VPN authentication portals
-   Web application login portals
-   Active Directory authentication
-   Cloud identity providers

The procedures apply to alerts triggered by:

-   SIEM correlation alerts
-   Firewall alerts
-   Endpoint Detection and Response alerts
-   Authentication monitoring systems
-   Threat intelligence alerts

------------------------------------------------------------------------

# 3. Threat Description

A brute force attack occurs when an attacker repeatedly attempts
different password combinations against a single user account until the
correct password is discovered.

These attacks are commonly observed against:

-   RDP servers exposed to the internet
-   SSH servers on Linux hosts
-   VPN gateways
-   Cloud authentication portals

Typical attack flow:

1.  Attacker identifies exposed authentication service
2.  Attacker repeatedly attempts passwords against a target account
3.  Authentication attempts occur at high frequency
4.  Eventually a successful login may occur
5.  Attacker attempts persistence or lateral movement

Common targeted usernames:

    administrator
    admin
    root
    guest
    test

------------------------------------------------------------------------

# 4. Detection Sources

Primary log sources include:

-   Windows Security Event Logs
-   Linux authentication logs
-   VPN authentication logs
-   Firewall logs
-   Identity provider logs
-   Endpoint Detection and Response telemetry

Key indicators:

-   Large number of failed login attempts for one user account
-   Authentication attempts from suspicious external IP addresses
-   Login attempts occurring at high frequency
-   Login attempts originating from multiple countries

------------------------------------------------------------------------

# 5. Alert Triage

SOC analyst should collect:

-   Source IP address
-   Target username
-   Destination host
-   Authentication protocol
-   Login attempt count
-   Timestamp range

Example alert:

Alert: RDP Brute Force Attempt Detected\
Source IP: 203.xxx.xxx.xxx\
Target User: Administrator\
Attempts: 500 login attempts in 10 minutes

Initial triage questions:

-   Is a single account being targeted repeatedly?
-   Is the attack coming from an external IP?
-   Is the attack ongoing?
-   Has any login attempt succeeded?

------------------------------------------------------------------------

# 6. Investigation Procedure

## Step 1 --- Investigate Source IP

Search SIEM logs for activity from the attacker IP.

Example SIEM query:

    source_ip = attacker_IP

Check for:

-   Multiple authentication failures
-   Repeated login attempts
-   Login attempts against the same username

------------------------------------------------------------------------

## Step 2 --- Review Authentication Logs

Relevant Windows Event IDs:

    4625 – Failed login
    4624 – Successful login

Look for patterns:

-   Hundreds of failed attempts
-   High-frequency login attempts
-   Same username repeatedly targeted

------------------------------------------------------------------------

## Step 3 --- Identify Successful Authentication

Determine if the attacker eventually logged in.

Investigate:

-   Successful login events
-   Login time and location
-   Device used for authentication

Indicators of compromise:

-   Successful login after many failures
-   Login from unusual country
-   Login outside normal working hours

------------------------------------------------------------------------

## Step 4 --- Investigate Post-Login Activity

If authentication succeeded, review system activity.

Check:

-   Process execution logs
-   Command execution
-   Network connections

Suspicious commands:

    whoami
    net user
    net localgroup administrators
    ipconfig
    netstat
    powershell -enc

Also investigate:

-   Creation of new user accounts
-   Privilege escalation attempts
-   Lateral movement to other systems

------------------------------------------------------------------------

# 7. Containment

If brute force activity is confirmed:

Immediate actions:

-   Block attacker IP at firewall
-   Block IP on VPN gateway
-   Enable temporary rate limiting

If authentication succeeded:

-   Terminate active sessions
-   Force password reset
-   Disable account temporarily if necessary

------------------------------------------------------------------------

# 8. Eradication

Remove attacker access.

Actions include:

-   Reset compromised credentials
-   Remove unauthorized accounts
-   Patch exposed services
-   Disable unnecessary authentication services

------------------------------------------------------------------------

# 9. Recovery

Restore secure operations.

Recommended improvements:

-   Enable Multi-Factor Authentication (MFA)
-   Restrict RDP access using VPN
-   Implement account lockout policies
-   Deploy geo-blocking if applicable

Continue monitoring authentication logs for additional attempts.

------------------------------------------------------------------------

# 10. Post-Incident Activities

Conduct incident review.

Tasks include:

-   Root cause analysis
-   Authentication control review
-   Detection rule tuning
-   Security monitoring improvements

Lessons learned should be used to improve SOC detection capabilities.

------------------------------------------------------------------------

# 11. Roles and Responsibilities

SOC Analyst - Investigate authentication alerts - Determine attack
scope - Collect evidence

Incident Response Team - Contain compromised accounts - Coordinate
remediation actions

Security Engineering Team - Improve detection rules - Implement
additional security controls

------------------------------------------------------------------------

# 12. MITRE ATT&CK Mapping

Credential Access\
T1110 -- Brute Force

Initial Access\
T1078 -- Valid Accounts

Discovery\
T1087 -- Account Discovery

------------------------------------------------------------------------

# 13. References

-   NIST SP 800-61 Incident Handling Guide
-   MITRE ATT&CK Framework
-   SANS Incident Handler Handbook
