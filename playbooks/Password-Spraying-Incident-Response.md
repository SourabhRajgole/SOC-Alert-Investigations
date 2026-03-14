# Incident Response Playbook

## Incident Type
Password Spraying Attack

---

# Introduction

Password spraying is a credential attack where attackers attempt a small number of passwords across many accounts.

This technique avoids account lockout policies.

---

# Summary

This playbook outlines response procedures for password spraying incidents.

---

# Incident Description

Common indicators include:

• Authentication attempts across many accounts  
• Repeated login attempts using the same password  
• Login attempts from suspicious IP addresses

---

# Incident Response Process

## Part 1 — Acquire Evidence

Collect:

Source IP  
Target accounts  
Authentication logs

Identify login patterns.

---

## Part 2 — Containment

Block attacker IPs.

Disable compromised accounts.

---

## Part 3 — Eradication

Force password resets.

Remove unauthorized accounts.

---

## Part 4 — Recovery

Enable MFA.

Monitor authentication logs.

---

## Part 5 — Post-Incident Activity

Improve password policies.

Update detection rules.

---

# MITRE ATT&CK Mapping

T1110.003 Password Spraying  
T1078 Valid Accounts

---

# References

NIST SP 800-61  
Microsoft Security Operations Guide
