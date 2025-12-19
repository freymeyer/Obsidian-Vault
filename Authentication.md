---
tags:
  - "#cybersecurity/iam/access-control"
  - "#interview/concepts"
aliases:
  - AuthN
---

# Authentication

> **One-liner:** The process of verifying *who you are* (proving your identity).

## 🎯 What Is It?
Authentication (AuthN) is the security process that validates a user's identity before granting access to a system. It answers the question: **"Are you who you claim to be?"**

## 🔐 Authentication Factors

| Factor | Type | Examples |
|--------|------|----------|
| **Something you know** | Knowledge | Password, PIN, security questions |
| **Something you have** | Possession | Phone, smart card, hardware token |
| **Something you are** | Inherence | Fingerprint, face ID, retina scan |
| **Somewhere you are** | Location | GPS, IP-based geolocation |
| **Something you do** | Behavior | Typing patterns, gait analysis |

## 🛡️ Multi-Factor Authentication (MFA)
Combining **2+ factors** from different categories significantly reduces risk:
- Password + SMS code (2FA)
- Badge + fingerprint + PIN (3FA)

## ❗ Authentication vs Authorization

| Authentication | Authorization |
|----------------|---------------|
| WHO are you? | WHAT can you do? |
| Happens first | Happens after AuthN |
| Validates identity | Validates permissions |
| Example: Login | Example: Access admin panel |

## 🚨 Common Authentication Attacks
- [[Brute-force]] — Trying all password combinations
- [[Dictionary Attacks]] — Using common password lists
- [[Credential Stuffing]] — Reusing leaked credentials
- [[Phishing]] — Tricking users to reveal credentials
- Session hijacking — Stealing session tokens

## 🎤 Interview STAR Example
> **Situation:** User accounts were being compromised despite password requirements.
> **Task:** Investigate the root cause and implement stronger authentication.
> **Action:** Analyzed logs, found credential stuffing attacks. Implemented MFA, added rate limiting, deployed breach password checking.
> **Result:** Account takeovers dropped by 95% within 30 days.

## 🔗 Related Concepts
- [[Authorization]]
- [[Identification, Authentication, Authorization, and Accountability (IAAA)]]
- [[Authentication Failures]]
- [[Broken Access Control]]
- [[OAuth]]
- [[SAML]]

## 📚 References
- OWASP Authentication Cheat Sheet
- NIST SP 800-63B Digital Identity Guidelines