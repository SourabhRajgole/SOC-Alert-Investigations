# Security Operations Center Playbook

## Incident Type
Password Spraying Attack

---

# 1. Objective

This playbook provides a structured investigation and response procedure for **password spraying attacks** detected within the organization's authentication infrastructure.

The objective is to:

• Detect large-scale authentication attempts using common passwords  
• Identify compromised user accounts  
• Contain unauthorized access  
• Restore secure authentication operations  

This playbook supports SOC analysts in responding quickly and consistently to credential-based attacks.

---

# 2. Scope

This playbook applies to authentication systems including:

• Active Directory environments  
• Microsoft 365 / Azure AD  
• VPN authentication portals  
• Web application authentication systems  
• Cloud identity providers  

The procedures outlined here apply to alerts triggered by SIEM, IAM monitoring systems, or user-reported suspicious login activity.

---

# 3. Threat Description

Password spraying is a credential attack technique where an attacker attempts a **small number of common passwords across many user accounts**.

Unlike brute force attacks, which repeatedly target a single account, password spraying spreads login attempts across multiple accounts to **avoid account lockout thresholds**.

Typical attack flow:

1. Attacker identifies valid usernames
2. Attacker attempts common passwords across multiple accounts
3. One or more accounts eventually authenticate successfully
4. Attacker uses compromised accounts for persistence or lateral movement

Common passwords used in spraying attacks:
