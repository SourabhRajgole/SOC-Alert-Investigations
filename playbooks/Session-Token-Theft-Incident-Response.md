# 🛡️ Security Operations Center Playbook

## Incident Type

Session Token Theft (Cookie Hijacking / Session Replay)

------------------------------------------------------------------------

# 1. Objective

This playbook provides a structured approach to detect, investigate, and
respond to **session token theft**, where attackers hijack authenticated
sessions to access accounts without needing credentials or MFA.

Objectives:

-   Detect anomalous session activity
-   Identify stolen/replayed tokens
-   Assess account impact and data exposure
-   Contain active sessions quickly
-   Prevent re-use of compromised tokens

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Cloud identity platforms (Azure AD, Okta, Google Workspace)
-   SaaS applications (Microsoft 365, Salesforce)
-   Web applications using cookies/session tokens
-   VPN and SSO environments

------------------------------------------------------------------------

# 3. Threat Description

Session token theft allows attackers to bypass authentication by reusing
valid session cookies/tokens.

Common sources:

-   Infostealer malware (e.g., browser cookie theft)
-   Man-in-the-browser attacks
-   Phishing with session replay (e.g., Evilginx)
-   XSS vulnerabilities (cookie exfiltration)

Attack flow:

1.  User authenticates (MFA completed)
2.  Session token is issued and stored (browser cookie)
3.  Attacker steals token
4.  Attacker reuses token from different device/IP
5.  Gains access without re-authentication

------------------------------------------------------------------------

# 4. Severity Classification

Low: - Single anomalous session, no sensitive activity

Medium: - New location/device, no data access

High: - Session reuse from different geo/device - Suspicious activity
(mail access, file access)

Critical: - Privileged account access - Data exfiltration or mailbox
rule changes - Multiple sessions hijacked

------------------------------------------------------------------------

# 5. Escalation Criteria

Escalate if:

-   Same session ID/token used from different IPs/devices
-   Access from high-risk or impossible geolocation
-   Privileged account or admin portal accessed
-   Suspicious mailbox rules or file downloads observed
-   Multiple users affected (campaign/infostealer)

------------------------------------------------------------------------

# 6. Detection Sources

-   Azure AD Sign-in Logs / Risky Sign-ins
-   Okta System Logs
-   SaaS audit logs (M365 Unified Audit, Google Workspace)
-   EDR alerts (infostealer activity)
-   Proxy/VPN logs

------------------------------------------------------------------------

# 7. Sample Evidence

User: jane.doe@company.com

Session ID: abc123xyz

Login Event: IP: 52.23.10.5 (New York, USA) Device: Windows / Chrome

Subsequent Activity (same session): IP: 185.77.22.11 (Russia) Device:
Unknown / Headless browser

Actions: - Accessed mailbox - Created inbox forwarding rule

------------------------------------------------------------------------

# 8. Alert Triage

Collect:

-   Username
-   Session ID / Correlation ID
-   Source IPs and locations
-   Device information (User Agent, Device ID)
-   Application accessed
-   Timestamp(s)

Key Questions:

-   Same session used from multiple IPs?
-   Device mismatch (known vs unknown)?
-   MFA recently completed?
-   Any risky user actions (downloads, rules, admin changes)?

------------------------------------------------------------------------

# 9. Investigation Procedure

## Step 1 --- Validate Anomalous Session

-   Correlate session/token identifiers across logs
-   Confirm same session used from different IPs/devices

------------------------------------------------------------------------

## Step 2 --- Geo & IP Analysis

-   Compare geolocations (impossible travel indicators)
-   Check IP reputation (proxy, TOR, hosting providers)

------------------------------------------------------------------------

## Step 3 --- Device & User-Agent Analysis

-   Compare User-Agent strings
-   Check for headless browsers or automation indicators
-   Validate device ID (if available)

------------------------------------------------------------------------

## Step 4 --- User Activity Review

-   Mailbox access, downloads, sharing activity
-   Admin actions (role changes, token grants)
-   Creation of inbox rules / forwarding

------------------------------------------------------------------------

## Step 5 --- Endpoint Correlation

-   Check EDR for infostealer alerts
-   Look for browser credential/cookie access
-   Identify initial compromise vector

------------------------------------------------------------------------

## Step 6 --- Scope Analysis

-   Search for same IP across multiple users
-   Identify other sessions with similar behavior
-   Determine campaign vs single-user compromise

------------------------------------------------------------------------

# 10. Detection Queries

## KQL (Microsoft Sentinel / Defender)

SigninLogs \| summarize count(), make_set(IPAddress),
make_set(DeviceDetail) by UserPrincipalName, SessionId \| where
array_length(set_IPAddress) \> 1

## Splunk

index=signin_logs \| stats values(src_ip) as ips, values(user_agent) as
ua by user, session_id \| where mvcount(ips) \> 1

## Sigma (Concept)

title: Session Reuse Across Multiple IPs detection: selection:
session_reuse: true condition: selection

------------------------------------------------------------------------

# 11. Containment

-   Revoke all active sessions for affected user(s)
-   Force sign-out from all devices
-   Reset user credentials
-   Enforce MFA re-registration (if risk is high)
-   Block suspicious IPs

------------------------------------------------------------------------

# 12. Eradication

-   Remove malicious mailbox rules or persistence
-   Remove unauthorized OAuth app grants (if present)
-   Clean infected endpoints (if infostealer involved)

------------------------------------------------------------------------

# 13. Recovery

-   Restore legitimate access
-   Monitor account for re-authentication anomalies
-   Validate user device hygiene (patching, AV/EDR)

------------------------------------------------------------------------

# 14. Post-Incident Actions

-   Enable Conditional Access (device/location restrictions)
-   Enforce token lifetime policies
-   Implement device compliance checks
-   Educate users on phishing and session theft risks
-   Update detection rules for session anomalies

------------------------------------------------------------------------

# 15. Analyst Notes

In real SOC environments, session token theft is often missed because:

-   No failed logins occur
-   MFA is already satisfied
-   Activity appears "authenticated"

Key indicators:

-   Same session across different IPs/devices
-   Sudden location change without re-authentication
-   High-risk user actions shortly after login

Always prioritize **session revocation** as the first containment step.

------------------------------------------------------------------------

# 16. Response Timeline

T+0 min → Alert triggered\
T+5 min → Session anomaly confirmed\
T+10 min → Sessions revoked\
T+15 min → Account secured\
T+30 min → Activity reviewed and cleaned

------------------------------------------------------------------------

# 17. MITRE ATT&CK Mapping

T1550 --- Use of Stolen Authentication Tokens\
T1550.004 --- Web Session Cookie

------------------------------------------------------------------------

# 18. References

-   NIST SP 800-61\
-   MITRE ATT&CK Framework\
-   Microsoft Identity Protection Documentation\
-   SANS Incident Handler Handbook
