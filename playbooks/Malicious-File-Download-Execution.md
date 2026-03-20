# Security Operations Center Playbook

## Incident Type

Malicious File Download / Execution (EDR/AV Alert)

------------------------------------------------------------------------

# 1. Objective

This playbook provides SOC analysts with a standardized procedure to
investigate and respond to malicious file download or execution alerts.

Objectives:

-   Identify malicious or suspicious files
-   Determine execution context
-   Assess impact
-   Contain affected systems
-   Remove malicious artifacts

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Endpoints (Windows, Linux, macOS)
-   Servers
-   EDR/AV tools
-   SIEM logs

------------------------------------------------------------------------

# 3. Threat Description

Malicious files are delivered through:

-   Phishing attachments
-   Malicious websites
-   Downloads
-   Cloud sharing links

Common file types:

-   .exe, .dll
-   .ps1, .js
-   .zip, .rar
-   .docm, .xlsm

------------------------------------------------------------------------

# 4. Detection Sources

-   EDR alerts
-   Antivirus alerts
-   SIEM detections
-   Web proxy logs

------------------------------------------------------------------------

# 5. Alert Triage

Collect:

-   Hostname
-   Username
-   File name
-   File hash
-   Process details
-   Timestamp

------------------------------------------------------------------------

# 6. Investigation Procedure

## Step 1 --- Validate Detection

Confirm detection in EDR.

------------------------------------------------------------------------

## Step 2 --- Hash Analysis

Check hash in VirusTotal.

------------------------------------------------------------------------

## Step 3 --- Process Tree

Check parent-child processes.

------------------------------------------------------------------------

## Step 4 --- File Location

Check suspicious directories:

-   Downloads
-   Temp
-   AppData

------------------------------------------------------------------------

## Step 5 --- Network Activity

Check for external connections.

------------------------------------------------------------------------

# 7. Containment

-   Isolate endpoint
-   Kill processes
-   Block hash

------------------------------------------------------------------------

# 8. Eradication

-   Delete files
-   Remove persistence

------------------------------------------------------------------------

# 9. Recovery

-   Restore system
-   Patch vulnerabilities

------------------------------------------------------------------------

# 10. Post-Incident

-   Update rules
-   Document findings

------------------------------------------------------------------------

# 11. MITRE ATT&CK

T1204 -- User Execution
