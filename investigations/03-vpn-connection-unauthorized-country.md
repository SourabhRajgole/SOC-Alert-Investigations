
# SOC Investigation 03 — VPN Connection Detected from Unauthorized Country

## Alert Information

| Field | Value |
|------|------|
| Event ID | 225 |
| Rule Name | SOC257 – VPN Connection Detected from Unauthorized Country |
| Severity | Low |
| Event Type | Unauthorized Access |
| Detection Platform | LetsDefend SIEM |
| Username | monica@letsdefend.io |
| Destination Host | vpn-letsdefend.io |
| Source IP | 113.161.158.12 |
| Classification | True Positive |

---

## Alert Description

The SIEM generated an alert indicating that a VPN connection attempt originated from a country not normally associated with the user's activity.

The alert was triggered when the user account **monica@letsdefend.io** attempted to access the corporate VPN service from the IP address **113.161.158.12**.

This IP address was identified as originating from **Vietnam**, which triggered the detection rule **SOC257 – VPN Connection Detected from Unauthorized Country**.

---

## Detection and Initial Triage

During the initial investigation, the alert metadata was reviewed to identify the source IP, user account, and service involved.

Key details observed:

- Source IP Address: 113.161.158.12
- Target Service: vpn-letsdefend.io
- Username Used: monica@letsdefend.io

Log management systems were queried to identify firewall and proxy events associated with the source IP address.

The logs confirmed that the attacker attempted to access the VPN login portal and submitted authentication requests.

---

## Log Analysis

Proxy and firewall logs revealed the following activity:

Source IP: 113.161.158.12  
Destination: vpn-letsdefend.io  
Request Method: POST  
URI: /logon.html  
HTTP Response Code: 200  
Username Attempted: monica@letsdefend.io  

The HTTP response code **200** indicates that the VPN login portal was accessible and responded successfully.

However, this does not confirm successful authentication.

Further authentication logs were analyzed to determine whether the login attempt succeeded.

---

## IP Reputation Analysis

The source IP address **113.161.158.12** was investigated using threat intelligence sources.

WHOIS records indicated that the IP belongs to:

Vietnam Posts and Telecommunications Group  
Location: Hanoi, Vietnam

Threat intelligence platforms such as AbuseIPDB and VirusTotal showed multiple historical reports related to this IP address, including:

- Brute force attacks
- Web exploitation attempts
- Malicious scanning activity

This reputation data supports the likelihood that the connection attempt was malicious.

---

## Authentication and MFA Analysis

Further firewall logs revealed that the attacker attempted to authenticate using the correct password for the user account **monica@letsdefend.io**.

However, the authentication system requires **Multi-Factor Authentication (MFA)**.

Firewall logs recorded the following action:

Action: Incorrect OTP Code

This indicates:

- The attacker entered the correct password
- A One-Time Password (OTP) was sent to the legitimate user
- The attacker failed to provide the correct OTP

Multiple OTP emails were generated during the attack attempt, indicating repeated login attempts.

---

## Additional Activity Observed

Additional firewall logs showed connection attempts from multiple source ports originating from the same attacker IP.

This behavior suggests that the attacker performed **port scanning activity** against the internal server IP **33.33.33.33**.

Such activity is often used by attackers to discover accessible services or potential vulnerabilities.

---

## Indicators of Compromise (IOCs)

| Type | Indicator |
|-----|-----------|
| Attacker IP | 113.161.158.12 |
| Target VPN | vpn-letsdefend.io |
| Username | monica@letsdefend.io |
| Target Host | 33.33.33.33 |
| Activity | Unauthorized VPN login attempt |

---

## MITRE ATT&CK Mapping

| Technique | Description |
|----------|-------------|
| T1133 | External Remote Services |
| T1110 | Brute Force (Password Guessing) |
| T1046 | Network Service Scanning |

---

## Investigation Conclusion

The alert was determined to be a **True Positive**.

The attacker successfully obtained the correct password for the user account **monica@letsdefend.io** and attempted to authenticate to the VPN service.

However, the attacker was unable to bypass the **Multi-Factor Authentication (MFA)** protection because the One-Time Password was not available to them.

As a result, the VPN login attempt failed and no unauthorized access was achieved.

---

## Recommended Response Actions

If this activity occurred in a real production environment, the following response actions would be recommended:

- Reset the password for the affected user account
- Investigate potential credential exposure
- Block or monitor the attacker IP address
- Implement geolocation-based access restrictions
- Review VPN authentication logs for additional suspicious activity

---

## Lessons Learned

Even though the attacker failed to bypass MFA protection, the fact that the correct password was used indicates that the user's credentials may have been compromised.

Organizations should:

- Enforce strong password policies
- Implement account lockout mechanisms
- Monitor abnormal login locations
- Continue using MFA to prevent unauthorized access

MFA played a critical role in preventing a successful compromise in this case.
