# Security Operations Center Playbook

## Incident Type

Malicious File Download / Execution (Endpoint Threat)

------------------------------------------------------------------------

# 1. Objective

This playbook provides SOC analysts with a structured approach to
detect, investigate, and respond to malicious file download and
execution events.

Objectives:

-   Identify malicious files and execution behavior
-   Determine infection vector (email, web, etc.)
-   Assess system impact and spread
-   Contain and remove malicious artifacts
-   Prevent reinfection

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Endpoints (Windows, Linux, macOS)
-   Servers
-   EDR/XDR platforms
-   Email and web gateways
-   Cloud file-sharing services

------------------------------------------------------------------------

# 3. Threat Description

Malicious files are commonly used for:

-   Initial access (phishing attachments)
-   Malware delivery (trojans, loaders)
-   Ransomware deployment
-   Credential theft

Common delivery methods:

-   Email attachments (DOCM, ZIP, PDF)
-   Malicious downloads (drive-by)
-   Cloud links (OneDrive, Google Drive)
-   USB/removable media

------------------------------------------------------------------------

# 4. Severity Classification

Low: - File downloaded but not executed - Known safe/false positive

Medium: - Suspicious file executed - No further activity

High: - Confirmed malware execution - Suspicious process chain or
persistence

Critical: - Ransomware behavior or lateral movement - Multiple systems
affected

------------------------------------------------------------------------

# 5. Escalation Criteria

Escalate if:

-   File execution confirmed as malicious
-   Persistence mechanisms detected
-   Network communication to known malicious IPs
-   Multiple endpoints affected
-   Privileged account involved

------------------------------------------------------------------------

# 6. Detection Sources

-   EDR alerts (process/file behavior)
-   Antivirus detections
-   SIEM correlation rules
-   Web proxy logs
-   Email security logs

------------------------------------------------------------------------

# 7. Sample Log Evidence

Process Tree:

winword.exe → powershell.exe → suspicious.exe

Command Line:

powershell -enc JABlAGM...

File Path:

C:`\Users`{=tex}`\john`{=tex}`\Downloads`{=tex}`\invoice`{=tex}.exe

------------------------------------------------------------------------

# 8. Alert Triage

Collect:

-   Hostname
-   Username
-   File name and path
-   File hash (SHA256)
-   Parent process
-   Timestamp
-   Source (email/web)

Key Questions:

-   Was the file executed?
-   Was it blocked or allowed?
-   Is hash known malicious?
-   Any follow-up activity?

------------------------------------------------------------------------

# 9. Investigation Procedure

## Step 1 --- Validate Detection

-   Confirm alert in EDR
-   Check detection verdict and action taken

------------------------------------------------------------------------

## Step 2 --- Hash Analysis

-   Check file hash in VirusTotal
-   Identify malware family (if known)

------------------------------------------------------------------------

## Step 3 --- Process Analysis

-   Review parent-child process chain
-   Look for suspicious execution patterns

Examples:

-   winword → powershell
-   browser → cmd → script

------------------------------------------------------------------------

## Step 4 --- File Location & Persistence

Check:

-   Downloads folder
-   AppData directories
-   Temp folders

Look for:

-   Registry Run keys
-   Scheduled tasks
-   Startup entries

------------------------------------------------------------------------

## Step 5 --- Network Activity

-   Check outbound connections
-   Identify C2 communication
-   Correlate with threat intelligence

------------------------------------------------------------------------

## Step 6 --- Scope Analysis

Search across environment:

hash="`<sha256>`{=html}" \| stats count by host

------------------------------------------------------------------------

# 10. Detection Queries

## Splunk

index=edr_logs file_action=executed \| stats count by file_hash, host

## KQL

DeviceProcessEvents \| where FileName endswith ".exe" \| summarize
count() by SHA256, DeviceName

## Sigma (Concept)

title: Suspicious File Execution detection: selection: file_execution:
true condition: selection

------------------------------------------------------------------------

# 11. Containment

-   Isolate affected endpoint
-   Kill malicious processes
-   Block file hash across EDR
-   Block associated domains/IPs

------------------------------------------------------------------------

# 12. Eradication

-   Remove malicious files
-   Delete persistence mechanisms
-   Perform full system scan

------------------------------------------------------------------------

# 13. Recovery

-   Restore system if needed
-   Patch vulnerabilities
-   Reinforce endpoint controls

------------------------------------------------------------------------

# 14. Post-Incident Actions

-   Update detection rules
-   Add IOCs to blocklists
-   Improve email/web filtering
-   Conduct user awareness training

------------------------------------------------------------------------

# 15. Analyst Notes

In real investigations, many alerts are triggered by:

-   User-downloaded tools (false positives)
-   Testing software
-   Legitimate admin scripts

Key differentiators:

-   Suspicious process chain → high risk
-   Unknown hash + external source → suspicious
-   Network beaconing → confirmed compromise

------------------------------------------------------------------------

# 16. Response Timeline

T+0 min → Alert triggered\
T+5 min → Initial triage\
T+10 min → Hash analyzed\
T+20 min → Endpoint isolated\
T+45 min → Malware removed

------------------------------------------------------------------------

# 17. MITRE ATT&CK Mapping

T1204 -- User Execution\
T1059 -- Command and Scripting Interpreter\
T1027 -- Obfuscated Files

------------------------------------------------------------------------

# 18. References

-   NIST SP 800-61
-   MITRE ATT&CK Framework
-   SANS Incident Handler Handbook
