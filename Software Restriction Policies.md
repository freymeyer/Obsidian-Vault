---
dg-publish: true
tags:
  - "#infrastructure/windows/policy"
aliases:
  - SRP
---

# Software Restriction Policies

> One-liner: Windows policy framework to control which software can run, based on rules and security levels.

## 🎯 What Is It?
SRP defines trust rules (hash, path, certificate, zone) and a default security level that governs execution. Commonly used to block untrusted scripts and binaries in user-writable paths.

## 🤔 Why It Matters
- Reduces malware execution and LOLBAS abuse.
- Complements AppLocker/WDAC on legacy systems.

## 🔬 How It Works
### Core Principles
1. Rules evaluated in order; most specific wins.
2. Default security level applies to everything else.
3. Policies delivered via Local or Group Policy.

### Technical Deep-Dive
- Default Security Level: typically "Unrestricted" by default; set to "Disallowed" for stronger posture.
- Rule Types: Hash, Path, Certificate, Internet Zone.
- Scope: Apply to all users or exclude local admins.

## 🛡️ Detection & Prevention
### How to Detect
- Eventing via AppLocker logs (if used) or SRP-related registry/policy audits.

### How to Prevent / Mitigate
- Prefer AppLocker/WDAC on modern Windows; use SRP where those are unavailable.
- Block user profile execution paths (e.g., %AppData%, %Temp%).

## 🔗 Related Concepts
- [[AppLocker]]
- [[Windows Event Logs]]
- [[Living off the Land (LOLBAS)]]
