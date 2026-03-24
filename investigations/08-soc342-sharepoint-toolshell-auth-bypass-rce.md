# SOC Investigation 320 — SharePoint ToolShell Auth Bypass and RCE Detected

## Alert Information

| Field | Value |
|---|---|
| Event ID | 320 |
| Rule Name | SOC342 – CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE |
| Severity | Critical |
| Event Type | Web Attack |
| Detection Platform | LetsDefend SIEM |
| Hostname | SharePoint01 |
| Host IP | 172.16.20.17 |
| Attacker IP | 107.191.58[.]76 |
| Classification | True Positive |
| Status | Contained |

## Alert Description

The SIEM generated an alert triggered by the rule **SOC342 – CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE**. The alert indicates a suspicious unauthenticated **HTTP POST** request targeting the SharePoint endpoint `/_layouts/15/ToolPane.aspx?DisplayMode=Edit&a=/ToolPane.aspx`, which is associated with the **ToolShell** exploit chain affecting on-premises Microsoft SharePoint servers.

CVE-2025-53770 is a critical SharePoint vulnerability tied to **unsafe deserialization and remote code execution**, and public guidance documented exploitation using crafted POST requests to the ToolPane endpoint. Microsoft and Rapid7 both described this activity as affecting **on-premises SharePoint** and noted that attackers use it to achieve code execution, deploy malicious ASPX payloads, and steal cryptographic machine keys for persistence.

Firewall and web logs showed that traffic from the external source IP **107.191.58[.]76** was **allowed** to reach the destination host **SharePoint01 (172.16.20.17)**. The request included a suspicious referer value of `/_layouts/SignOut.aspx` and a large payload size of **7699 bytes**, which aligns with the exploitation pattern documented in the community walkthrough and public vendor reporting.

## Detection

### Enrichment & Context

The first step of the investigation was determining whether the attacker IP address was internal or external.

| Source IP | Destination Host |
|---|---|
| 107.191.58[.]76 | SharePoint01 (172.16.20.17) |

The source IP address is external and therefore suspicious. The destination system is a SharePoint server, which makes the alert especially critical because Microsoft confirmed active exploitation of internet-facing **on-premises SharePoint** servers in July 2025.

### IP Reputation Control

Threat intelligence checks were performed using:

- OnlyHunt Threat Intel
- Community walkthrough reference
- Vendor guidance and public reporting

OnlyHunt linked **107.191.58[.]76** to **CVE-2025-53770** activity. The community walkthrough also associated this IP with the same exploit scenario in LetsDefend. Microsoft and Rapid7 both documented that active campaigns exploited this SharePoint vulnerability to gain code execution and steal machine keys.

## Analysis

### Traffic Analysis

The HTTP request observed was:

- **Method:** POST
- **URL:** `/_layouts/15/ToolPane.aspx?DisplayMode=Edit&a=/ToolPane.aspx`
- **Referer:** `/_layouts/SignOut.aspx`
- **Content-Length:** 7699
- **User-Agent:** Firefox 120.0

This request is malicious for several reasons:

- It targets the **ToolPane.aspx** endpoint associated with ToolShell exploitation.
- It uses **POST**, which is consistent with exploit payload delivery.
- It includes a **spoofed referer** value of `/_layouts/SignOut.aspx`.
- It carries a **large body**, suggesting encoded exploit content instead of normal browsing activity.

Microsoft reported that observed attacks began with crafted POST requests to the ToolPane endpoint, and the community walkthrough specifically identified the same URL, referer, and payload characteristics in this LetsDefend scenario.

### Endpoint Analysis

Endpoint telemetry from **SharePoint01** was reviewed to determine whether the attack was successful.

A suspicious PowerShell command was executed on the host:

```text
"C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -nop -w hidden -e <base64>
```

This command used:
- `-nop` for no profile
- `-w hidden` to hide the execution window
- `-e` to pass an encoded command

These flags are strongly suspicious and commonly observed in malicious execution chains.

After decoding the command, the content revealed ASPX server-side code that loads `System.Web`, accesses `System.Web.Configuration.MachineKeySection`, and prints out the following values:

- `ValidationKey`
- `Validation`
- `DecryptionKey`
- `Decryption`
- `CompatibilityMode`

This confirms that the attacker’s objective was to extract **ASP.NET machine keys** from the SharePoint server. Microsoft explicitly warned that attackers exploiting this SharePoint campaign stole machine key material, and both Microsoft and Rapid7 recommended rotating those keys after compromise.

### Post-Compromise Activity

The decoded payload shows clear **post-exploitation activity**. Rather than only probing the server, the attacker successfully executed code that accessed sensitive cryptographic configuration. Microsoft’s threat intelligence blog described post-exploitation in these attacks as including deployment of files such as `spinstall0.aspx` and theft of machine key material. The community walkthrough also documented machine-key extraction, `spinstall0.aspx`, and follow-on execution activity as part of the same incident chain.

The ability to read machine keys is highly dangerous because these keys may allow an attacker to:
- forge authentication tokens
- decrypt protected data
- maintain unauthorized access
- support persistence and later lateral movement

This means the attack was **successful**, not just attempted.

## Indicators of Compromise (IOCs)

| Type | Indicator |
|---|---|
| IPv4 | 107.191.58[.]76 |
| Hostname | SharePoint01 |
| IPv4 | 172.16.20.17 |
| URL Path | `/_layouts/15/ToolPane.aspx?DisplayMode=Edit&a=/ToolPane.aspx` |
| Referer | `/_layouts/SignOut.aspx` |
| CVE | CVE-2025-53770 |
| Command | `powershell.exe -nop -w hidden -e` |
| Script Behavior | ASP.NET MachineKey extraction |

## MITRE ATT&CK Mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Execution | T1059.001 – PowerShell |
| Persistence | T1505.003 – Web Shell |
| Credential Access | T1552 – Unsecured Credentials / Sensitive Configuration Exposure |
| Defense Evasion | T1027 – Obfuscated/Compressed Files and Information |

## Containment

Based on the investigation results, the system was confirmed to be compromised.

The affected host was immediately isolated from the network using endpoint security controls to prevent lateral movement and further attacker activity.

| Hostname | IP |
|---|---|
| SharePoint01 | 172.16.20.17 |

After containment, the case was escalated because the investigation confirmed successful exploitation and post-compromise access to sensitive server cryptographic material.

## Summary

The alert detected malicious external web traffic targeting the SharePoint server **SharePoint01 (172.16.20.17)** through the ToolPane endpoint associated with **CVE-2025-53770**.

Investigation confirmed:

- A crafted external **POST** request from **107.191.58[.]76**
- A suspicious **spoofed referer** and large payload
- Threat intelligence association between the source IP and CVE-2025-53770
- Encoded hidden **PowerShell** execution on the target server
- Decoded ASPX code that extracted **ASP.NET MachineKey** values
- Successful post-exploitation activity and server compromise

This attack demonstrates a successful exploitation of a public-facing SharePoint service through the ToolShell vulnerability chain, followed by cryptographic key theft activity.

## Lessons Learned

- Monitoring suspicious web requests to sensitive application endpoints is critical for early detection of exploit activity.
- Public-facing SharePoint servers require urgent patching and hardening when critical vulnerabilities are disclosed.
- PowerShell activity on SharePoint servers should be closely monitored, especially when spawned in suspicious execution chains.
- Machine key theft significantly increases post-compromise risk because it can enable persistence and token forgery.
- Community walkthroughs can be useful for training, but findings should always be validated against official vendor guidance.

## Remediation Actions

Recommended remediation actions include:

- Apply the latest Microsoft security updates for supported SharePoint versions immediately.
- Rotate all SharePoint **ASP.NET machine keys** and restart IIS after patching.
- Review the server for unauthorized ASPX files such as `spinstall0.aspx` or similar variants.
- Investigate for abnormal process chains such as `w3wp.exe` spawning `cmd.exe` or `powershell.exe`.
- Restrict external exposure to SharePoint wherever possible by using VPN, reverse proxy, or authenticated gateways if immediate isolation is not possible.
- Deploy or validate endpoint detection and AMSI protections on SharePoint servers.

## References

- Community walkthrough: https://medium.com/@Gavin.M.Fernandes/letsdefend-soc342-cve-2025-53770-sharepoint-toolshell-auth-bypass-and-rce-e3837374f122
- Microsoft Security Blog: https://www.microsoft.com/en-us/security/blog/2025/07/22/disrupting-active-exploitation-of-on-premises-sharepoint-vulnerabilities/
- Microsoft MSRC Guidance: https://www.microsoft.com/en-us/msrc/blog/2025/07/customer-guidance-for-sharepoint-vulnerability-cve-2025-53770/
- Rapid7 Analysis: https://www.rapid7.com/blog/post/etr-zero-day-exploitation-of-microsoft-sharepoint-servers-cve-2025-53770/
