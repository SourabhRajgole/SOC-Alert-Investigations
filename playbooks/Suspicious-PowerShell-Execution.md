#  Security Operations Center Playbook

## Incident Type

Suspicious PowerShell Execution (Encoded / Obfuscated Commands)

------------------------------------------------------------------------

# 1. Objective

This playbook provides a structured approach to detect, investigate, and
respond to suspicious PowerShell activity, often used in
post-exploitation and malware execution.

Objectives:

-   Identify malicious PowerShell usage
-   Detect obfuscation and encoded commands
-   Analyze execution behavior
-   Contain compromised systems
-   Prevent further exploitation

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Windows endpoints
-   Servers
-   EDR/XDR platforms
-   SIEM monitoring systems

------------------------------------------------------------------------

# 3. Threat Description

Attackers use PowerShell for:

-   Command execution
-   Payload delivery
-   Persistence
-   Lateral movement

Common patterns:

-   Encoded commands (-enc)
-   Obfuscated scripts
-   Download cradle attacks

------------------------------------------------------------------------

# 4. Severity Classification

Low: - Admin usage - Known scripts

Medium: - Suspicious command usage - Unknown scripts

High: - Encoded/obfuscated commands - External connections

Critical: - Confirmed malicious activity - Persistence or lateral
movement

------------------------------------------------------------------------

# 5. Escalation Criteria

Escalate if:

-   Encoded commands detected
-   PowerShell spawned from suspicious parent process
-   External network communication observed
-   Privileged account used

------------------------------------------------------------------------

# 6. Detection Sources

-   Windows Event Logs (Event ID 4688)
-   PowerShell Logs (Event ID 4104)
-   EDR alerts
-   SIEM correlation rules

------------------------------------------------------------------------

# 7. Sample Evidence

Command:

powershell -enc SQBtAGUAbgB0...

Process Tree:

winword.exe → powershell.exe

------------------------------------------------------------------------

# 8. Alert Triage

Collect:

-   Hostname
-   Username
-   Command line
-   Parent process
-   Timestamp

------------------------------------------------------------------------

# 9. Investigation Procedure

## Step 1 --- Validate Execution

-   Confirm PowerShell execution in logs

## Step 2 --- Decode Command

-   Decode base64 encoded commands

## Step 3 --- Process Analysis

-   Check parent-child relationship

## Step 4 --- Network Activity

-   Identify external connections

## Step 5 --- Scope Analysis

-   Check similar activity across hosts

------------------------------------------------------------------------

# 10. Detection Queries

## Splunk

index=windows powershell \| search "enc"

## KQL

DeviceProcessEvents \| where ProcessCommandLine contains "-enc"

------------------------------------------------------------------------

# 11. Containment

-   Isolate endpoint
-   Kill PowerShell process
-   Block malicious domains

------------------------------------------------------------------------

# 12. Eradication

-   Remove scripts
-   Delete persistence

------------------------------------------------------------------------

# 13. Recovery

-   Restore system
-   Monitor activity

------------------------------------------------------------------------

# 14. Post-Incident

-   Update rules
-   Restrict PowerShell usage

------------------------------------------------------------------------

# 15. Analyst Notes

PowerShell is widely used in attacks due to its flexibility.

Key indicators:

-   Encoded commands
-   Suspicious parent processes
-   Network callbacks

------------------------------------------------------------------------

# 16. Response Timeline

T+0 min → Alert triggered T+5 min → Command analyzed T+10 min → Endpoint
isolated

------------------------------------------------------------------------

# 17. MITRE ATT&CK

T1059 --- Command and Scripting Interpreter

------------------------------------------------------------------------

# 18. References

-   NIST SP 800-61
-   MITRE ATT&CK
