# Security Operations Center Playbook

## Incident Type

Suspicious PowerShell Execution

------------------------------------------------------------------------

# 1. Objective

This playbook provides SOC analysts with a structured process to
investigate and respond to suspicious PowerShell execution alerts.

Objectives:

-   Identify malicious or suspicious PowerShell activity
-   Detect encoded or obfuscated commands
-   Determine attacker intent
-   Contain compromised systems
-   Prevent further execution

------------------------------------------------------------------------

# 2. Scope

Applies to:

-   Windows endpoints
-   Servers running PowerShell
-   EDR-monitored systems
-   SIEM log sources (Windows Event Logs)

------------------------------------------------------------------------

# 3. Threat Description

Attackers commonly use PowerShell because it is:

-   Built into Windows
-   Powerful and flexible
-   Trusted by systems (LOLBins)

Common malicious patterns:

-   Encoded commands (-enc)
-   Obfuscated scripts
-   Download cradles (IEX, Invoke-WebRequest)
-   Fileless malware

Example:

powershell.exe -enc
SQBFAFgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABOAGUAdwAtAE8AYgBqAGUAYwB0ACkA

------------------------------------------------------------------------

# 4. Detection Sources

-   Windows Event Logs (Event ID 4688)
-   PowerShell Logs (Event ID 4104)
-   EDR alerts
-   SIEM detection rules

------------------------------------------------------------------------

# 5. Alert Triage

Collect:

-   Hostname
-   Username
-   Command line
-   Parent process
-   Timestamp

Questions:

-   Is PowerShell expected on this system?
-   Is command encoded or obfuscated?
-   Is it linked to suspicious process?

------------------------------------------------------------------------

# 6. Investigation Procedure

## Step 1 --- Analyze Command

Check for:

-   Encoded strings (-enc)
-   Suspicious keywords:
    -   IEX
    -   DownloadString
    -   Invoke-WebRequest

Decode if needed.

------------------------------------------------------------------------

## Step 2 --- Review Parent Process

Check which process launched PowerShell:

-   explorer.exe (normal)
-   winword.exe (suspicious)
-   cmd.exe (context dependent)

------------------------------------------------------------------------

## Step 3 --- Check Execution Context

Investigate:

-   User account
-   Privileges
-   Execution time

------------------------------------------------------------------------

## Step 4 --- Check Network Activity

Look for:

-   External connections
-   File downloads
-   Suspicious domains

------------------------------------------------------------------------

# 7. Containment

-   Isolate affected endpoint
-   Kill PowerShell process
-   Block malicious IP/domain

------------------------------------------------------------------------

# 8. Eradication

-   Remove malicious scripts
-   Delete persistence mechanisms
-   Scan system with EDR

------------------------------------------------------------------------

# 9. Recovery

-   Restore system if needed
-   Patch vulnerabilities
-   Monitor for re-execution

------------------------------------------------------------------------

# 10. Post-Incident Activities

-   Improve detection rules
-   Document findings
-   Train users if needed

------------------------------------------------------------------------

# 11. MITRE ATT&CK Mapping

Execution\
T1059.001 -- PowerShell

------------------------------------------------------------------------

# 12. References

-   MITRE ATT&CK
-   NIST SP 800-61
