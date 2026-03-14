# Security Operations Center Playbook

## Incident Type

Session Token Theft / Cookie Hijacking

------------------------------------------------------------------------

# 1. Objective

This playbook provides SOC analysts with a standardized process to
investigate and respond to **session token theft incidents**, also known
as **session hijacking or cookie theft**.

Objectives:

-   Detect unauthorized reuse of authentication tokens
-   Identify compromised sessions
-   Contain account takeover activity
-   Remove attacker persistence
-   Improve authentication security controls

------------------------------------------------------------------------

# 2. Scope

This playbook applies to web and cloud authentication platforms
including:

-   Microsoft 365 / Azure AD
-   Google Workspace
-   Okta
-   SaaS applications
-   Cloud consoles (AWS, Azure, GCP)
-   Web application authentication systems

Detection sources include:

-   SIEM alerts
-   Identity provider logs
-   Endpoint Detection and Response (EDR)
-   Cloud audit logs
-   Web proxy logs

------------------------------------------------------------------------

# 3. Threat Description

Session token theft occurs when an attacker steals a valid
authentication session token (such as a cookie or OAuth token) and
reuses it to access services without needing the user's password or MFA.

Common methods attackers use:

-   Malware stealing browser cookies
-   Man-in-the-middle attacks
-   Malicious browser extensions
-   Phishing frameworks that capture session tokens
-   Compromised endpoints

Typical attack flow:

1.  User authenticates successfully
2.  Attacker steals session token
3.  Attacker reuses token to access services
4.  Authentication bypass occurs because MFA has already been completed
5.  Attacker performs actions as the legitimate user

------------------------------------------------------------------------

# 4. Detection Sources

Session token theft may be detected through:

-   Identity provider anomaly alerts
-   Multiple sessions from different IP locations
-   Login without MFA challenge
-   Abnormal device fingerprint changes
-   Suspicious API activity

Key indicators:

-   Same session token used from multiple IPs
-   Rapid geographic location changes
-   Login activity from TOR or proxy networks
-   Suspicious user-agent changes

------------------------------------------------------------------------

# 5. Alert Triage

SOC analyst should collect:

-   Username
-   Session ID / Token identifier
-   Source IP addresses
-   Login locations
-   Device identifiers
-   Authentication timestamps

Example alert:

Alert: Suspicious Session Reuse Detected\
User: alice@company.com\
Session reused from two different IP addresses within 5 minutes.

Initial triage questions:

-   Was the login performed by the legitimate user?
-   Did the user recently change devices?
-   Is the IP address associated with a VPN or proxy?
-   Was MFA bypassed during the login?

------------------------------------------------------------------------

# 6. Investigation Procedure

## Step 1 --- Review Authentication Logs

Search identity provider logs for activity related to the user.

Example SIEM query:

    user = "target_user"
    | stats count by source_ip, device_id

Check for:

-   Multiple sessions from different locations
-   Session reuse across IP addresses
-   Authentication without MFA challenge

------------------------------------------------------------------------

## Step 2 --- Analyze Source IP Addresses

Investigate IP addresses involved.

Check:

-   IP geolocation
-   Threat intelligence reputation
-   VPN / TOR usage

Use intelligence platforms such as:

-   VirusTotal
-   AbuseIPDB
-   GreyNoise

------------------------------------------------------------------------

## Step 3 --- Validate User Activity

Contact the user and confirm:

-   Did they log in from both locations?
-   Did they use multiple devices?
-   Did they install new browser extensions?

If user denies activity, assume potential compromise.

------------------------------------------------------------------------

## Step 4 --- Review Post-Authentication Activity

Investigate actions performed during the suspicious session.

Review:

-   File downloads
-   Email access
-   Administrative actions
-   API token creation
-   Cloud console actions

Indicators of compromise:

-   Data downloads
-   Privilege escalation attempts
-   Creation of persistence tokens

------------------------------------------------------------------------

# 7. Containment

If session compromise is confirmed:

Immediate actions:

-   Revoke active sessions
-   Invalidate authentication tokens
-   Force password reset
-   Disable account temporarily if required

Block malicious IP addresses if confirmed.

------------------------------------------------------------------------

# 8. Eradication

Remove attacker persistence.

Actions include:

-   Remove unauthorized OAuth tokens
-   Remove suspicious API tokens
-   Review mailbox forwarding rules
-   Audit privileged access assignments

------------------------------------------------------------------------

# 9. Recovery

Restore secure user access.

Recommended improvements:

-   Enforce MFA for sensitive operations
-   Implement session expiration policies
-   Deploy device-based authentication checks
-   Monitor for abnormal session behavior

Continue monitoring authentication activity.

------------------------------------------------------------------------

# 10. Post-Incident Activities

Conduct incident review.

Tasks include:

-   Root cause analysis
-   Endpoint compromise investigation
-   Detection rule tuning
-   Security awareness training

------------------------------------------------------------------------

# 11. Roles and Responsibilities

SOC Analyst

-   Investigate authentication anomalies
-   Determine scope of compromise
-   Document incident findings

Incident Response Team

-   Contain compromised sessions
-   Coordinate remediation

Security Engineering

-   Improve identity monitoring
-   Deploy stronger authentication controls

------------------------------------------------------------------------

# 12. MITRE ATT&CK Mapping

Credential Access\
T1539 -- Steal Web Session Cookie

Initial Access\
T1078 -- Valid Accounts

Defense Evasion\
T1550 -- Use of Stolen Authentication Tokens

------------------------------------------------------------------------

# 13. References

-   NIST SP 800-61 Incident Handling Guide
-   MITRE ATT&CK Framework
-   SANS Incident Handler Handbook
-   OWASP Session Management Guidelines
