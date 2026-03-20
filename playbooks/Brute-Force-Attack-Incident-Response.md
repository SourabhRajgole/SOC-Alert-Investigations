# Security Operations Center Playbook

## Incident Type

Brute Force Attack (Authentication Abuse)

------------------------------------------------------------------------

# 1. Objective

This playbook provides a structured approach for SOC analysts to detect,
investigate, and respond to brute force attacks targeting authentication
systems.

Objectives:

-   Identify unauthorized login attempts
-   Differentiate between brute force and benign failures
-   Assess risk and potential compromise
-   Contain attacker activity
-   Prevent future attempts

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Windows systems (Event Logs)
-   VPN and remote access systems
-   Cloud identity providers (Azure AD, Okta)
-   Linux SSH authentication logs
-   Web applications and APIs

------------------------------------------------------------------------

# 3. Threat Description

A brute force attack involves repeated login attempts using different
passwords to gain unauthorized access to an account.

Attack patterns:

-   Multiple passwords → single account (classic brute force)
-   High-frequency login failures from one IP
-   Automated tools/scripts targeting exposed services

------------------------------------------------------------------------

# 4. Severity Classification

Low: - Few failed attempts - Known user mistake

Medium: - Repeated failures from same IP - No successful login

High: - Successful login after multiple failures - Privileged account
targeted

Critical: - Multiple accounts compromised - Lateral movement observed

------------------------------------------------------------------------

# 5. Escalation Criteria

Escalate if:

-   Successful login detected after brute force attempts
-   Privileged or admin account targeted
-   Multiple hosts affected
-   External malicious IP confirmed
-   Suspicious post-login activity observed

------------------------------------------------------------------------

# 6. Detection Sources

-   Windows Event Logs (Event ID 4625, 4624)
-   VPN logs
-   Firewall logs
-   SIEM correlation rules
-   Cloud authentication logs

------------------------------------------------------------------------

# 7. Sample Log Evidence

Example:

EventID: 4625\
Account Name: admin\
Source IP: 185.234.72.11\
Failure Reason: Bad password\
Logon Type: 10 (RDP)

Observation:

-   50+ failed attempts within 2 minutes
-   Same IP targeting same account

------------------------------------------------------------------------

# 8. Alert Triage

Collect:

-   Username
-   Source IP
-   Number of attempts
-   Timestamp range
-   Target system
-   Authentication method

Initial Questions:

-   Is the IP internal or external?
-   Is the account privileged?
-   Any successful login?
-   Is this normal user behavior?

------------------------------------------------------------------------

# 9. Investigation Procedure

## Step 1 --- Validate Alert

-   Confirm number of failed attempts
-   Check alert accuracy in SIEM

------------------------------------------------------------------------

## Step 2 --- Analyze Login Pattern

-   Same IP → many passwords → brute force
-   Same password → many users → password spraying

------------------------------------------------------------------------

## Step 3 --- Check for Success

Look for:

Event ID 4624 (successful login)

If found → HIGH severity

------------------------------------------------------------------------

## Step 4 --- IP Reputation

-   Check IP using threat intelligence
-   Look for known malicious sources

------------------------------------------------------------------------

## Step 5 --- Scope Analysis

Search across environment:

index=auth_logs EventCode=4625 \| stats count by src_ip, user

------------------------------------------------------------------------

## Step 6 --- Correlate Logs

-   Firewall logs (connection attempts)
-   VPN logs
-   EDR logs

------------------------------------------------------------------------

# 10. Detection Queries

## Splunk

index=windows EventCode=4625 \| stats count by src_ip, user \| where
count \> 20

## KQL

SigninLogs \| summarize count() by IPAddress, UserPrincipalName \| where
count\_ \> 20

## Sigma Rule (Concept)

title: Brute Force Detection detection: selection: EventID: 4625
condition: selection \| count \> 20

------------------------------------------------------------------------

# 11. Containment

-   Block malicious IP at firewall
-   Lock targeted account (if needed)
-   Enforce MFA
-   Rate-limit authentication attempts

------------------------------------------------------------------------

# 12. Eradication

-   Remove attacker access
-   Reset compromised credentials
-   Audit affected systems

------------------------------------------------------------------------

# 13. Recovery

-   Unlock user accounts
-   Monitor login activity
-   Reinforce authentication policies

------------------------------------------------------------------------

# 14. Post-Incident Actions

-   Update detection rules
-   Add IPs to blocklists
-   Conduct user awareness training
-   Review authentication policies

------------------------------------------------------------------------

# 15. Analyst Notes

In real investigations, many brute force alerts are false positives
caused by:

-   Users forgetting passwords
-   Automated scripts
-   Internal vulnerability scans

Key differentiators:

-   High frequency attempts → likely attack
-   External IP targeting privileged account → high risk
-   Successful login after failures → confirmed compromise

------------------------------------------------------------------------

# 16. Response Timeline

T+0 min → Alert triggered\
T+5 min → Initial triage\
T+10 min → Pattern confirmed\
T+15 min → IP blocked\
T+30 min → Account secured\
T+1 hr → Incident documented

------------------------------------------------------------------------

# 17. MITRE ATT&CK Mapping

T1110 -- Brute Force

------------------------------------------------------------------------

# 18. References

-   NIST SP 800-61
-   MITRE ATT&CK Framework
-   SANS Incident Handler Handbook
