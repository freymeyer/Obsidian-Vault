---
tags:
  - "#infrastructure/windows/audit-policy"
aliases:
  - Audit logon events
---

# Audit Logon Events

> One-liner: Windows audit policy that records account logon/logoff activities and related authentication events.

## 🎯 What Is It?
Controls whether Windows logs successful and/or failed logon attempts. Events surface in the Security log (e.g., 4624, 4625) and are critical for identity-related investigations.

## 🤔 Why It Matters
- Tracks authentication abuse and brute-force attempts.
- Supports lateral movement and account compromise detection.

## 🔬 How It Works
### Core Principles
1. Configure Success and Failure auditing via Local/Group Policy.
2. Tune to reduce noise while retaining security value.
3. Forward to SIEM for correlation.

### Technical Deep-Dive
- Policy path: Security Settings → Local Policies → Audit Policy → Audit logon events.
- Key events: 4624 (Success), 4625 (Failure), 4634 (Logoff), 4647 (User-initiated logoff).

## 🛡️ Detection & Prevention
### How to Detect
- Alert on excessive 4625 failures and anomalous 4624 patterns.

### How to Prevent / Mitigate
- Enforce account lockout, MFA, and strong password policies.

## 🔗 Related Concepts
- [[Windows Event Logs]]
- [[Security Information and Event Management system (SIEM)]]
