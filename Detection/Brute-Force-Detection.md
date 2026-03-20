# 🔍 Detection Rule: Brute Force Login Attempt

------------------------------------------------------------------------

## 1. Objective

Detect multiple failed login attempts from a single source within a
short time window, indicating a potential brute force attack.

------------------------------------------------------------------------

## 2. Log Sources

-   Windows Security Logs (Event ID 4625)
-   Linux Authentication Logs (/var/log/auth.log)
-   Azure AD Sign-in Logs
-   VPN / Firewall Logs

------------------------------------------------------------------------

## 3. Detection Logic

Trigger an alert when:

-   Multiple failed login attempts (\>10)
-   From the same IP address
-   Within a short timeframe (5 minutes)

------------------------------------------------------------------------

## 4. Detection Queries

### Splunk

    index=auth sourcetype=WinEventLog:Security EventCode=4625
    | stats count by src_ip, user
    | where count > 10

------------------------------------------------------------------------

### KQL (Microsoft Sentinel / Defender)

    SigninLogs
    | where ResultType != 0
    | summarize FailedAttempts = count() by IPAddress, UserPrincipalName, bin(TimeGenerated, 5m)
    | where FailedAttempts > 10

------------------------------------------------------------------------

## 5. Tuning & Thresholds

-   Adjust threshold based on environment (e.g., 5--20 attempts)
-   Exclude known internal IP ranges
-   Exclude service accounts if needed

------------------------------------------------------------------------

## 6. False Positives

-   Users entering incorrect passwords repeatedly
-   Misconfigured applications/services
-   Password reset attempts

------------------------------------------------------------------------

## 7. Severity

-   Medium: Multiple failed attempts without success
-   High: Followed by successful login (possible compromise)

------------------------------------------------------------------------

## 8. MITRE ATT&CK Mapping

-   T1110 --- Brute Force

------------------------------------------------------------------------

## 9. Analyst Notes

Key indicators of brute force:

-   High volume of failed logins
-   Single IP targeting multiple accounts
-   Rapid login attempts in short duration

------------------------------------------------------------------------

## 10. Recommended Response

-   Block source IP
-   Lock affected accounts
-   Enforce MFA
-   Monitor for successful login attempts

------------------------------------------------------------------------

## 11. Improvements

-   Add geo-location analysis
-   Combine with impossible travel detection
-   Correlate with successful login events
