---
tags:
  - "#infrastructure/windows/policy/security-options"
aliases:
  - Interactive logon: Display user information when the session is locked
---

# Interactive logon — Display user information when the session is locked

> One-liner: Windows security option controlling whether user identity details are shown on the lock screen of an active session.

## 🎯 What Is It?
A Local/Group Policy setting that determines if the last signed-in user's name, domain, and email are displayed when the session is locked.

## 🤔 Why It Matters
- Hiding identity details reduces reconnaissance by shoulder-surfers or insiders.
- Recommended to suppress on sensitive systems (Finance/HR) and shared devices.

## 🔬 How It Works
### Core Principles
1. Applied via Security Options in Local/Group Policy.
2. Options include: display user information or do not display.
3. Supports privacy and least information disclosure.

### Technical Deep-Dive
- Path: Security Settings → Local Policies → Security Options.
- Policy: "Interactive logon: Display user information when the session is locked".
- Recommended: Do not display user information on lock screen for sensitive roles.

## 🔗 Related Concepts
- [[Audit Logon Events]]
- [[Windows Event Logs]]
