# Incident Response Playbook

## Incident Type
Brute Force Authentication Attack

---

# Introduction

Brute force attacks involve repeated login attempts to guess a user’s password.

These attacks commonly target:

• RDP servers  
• SSH services  
• VPN portals  
• Cloud authentication services

---

# Summary

This playbook outlines procedures to detect and respond to brute force login attempts.

---

# Incident Description

Indicators of brute force attacks include:

• Multiple login failures  
• Rapid authentication attempts  
• Login attempts targeting a single account

---

# Incident Response Process

## Part 1 — Acquire, Preserve Evidence

Collect:

Source IP address  
Target username  
Authentication service  
Login timestamps

Analyze authentication logs.

Relevant Windows logs:

Event ID 4625 — Failed login  
Event ID 4624 — Successful login

---

## Part 2 — Containment

Block attacker IP addresses.

Disable targeted accounts if necessary.

---

## Part 3 — Eradication

Reset compromised passwords.

Remove malicious processes.

---

## Part 4 — Recovery

Enable MFA.

Implement account lockout policies.

---

## Part 5 — Post-Incident Activity

Update brute force detection rules.

Review firewall policies.

---

# MITRE ATT&CK Mapping

T1110 Brute Force  
T1078 Valid Accounts

---

# References

NIST SP 800-61  
SANS Incident Handler Handbook
