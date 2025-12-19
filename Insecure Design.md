---
tags:
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - Design Flaws
  - Secure Design
severity: High
owasp: A06
---

# Insecure Design

> **One-liner:** Security flaws built into the architecture from the start—you can't patch a missing security control.

## 🎯 What Is It?
This is **A06** of [[OWASP]]. Insecure design occurs when applications lack security controls at the architectural level. Unlike implementation bugs, these are fundamental design flaws that require re-architecture to fix.

## 💥 Why It Matters (Impact)
- **Systemic Risk:** Can't be fixed with a patch
- **Business Logic:** Abuse cases not considered
- **Costly:** Requires redesign, not just code fixes

**Key Insight:** Insecure design ≠ insecure implementation. You can have secure code with an insecure design.

## 📊 Insecure Design vs Implementation Bugs

| Insecure Design | Implementation Bug |
|-----------------|-------------------|
| Missing rate limiting on password reset | SQL injection in login form |
| No transaction limits on transfers | XSS in comment field |
| API allows unlimited data export | Buffer overflow |
| No re-authentication for sensitive actions | Hardcoded credentials |

## 💰 Real-World Example: Clubhouse

> [Clubhouse](https://www.networkintelligence.ai/blogs/vulnerabilities-and-privacy-issues-with-clubhouse-app/) assumed users would only interact through the mobile app. The backend API had no authentication—anyone could query user data, room info, and private conversations directly. The "private conversation" design was fundamentally flawed.

## 🔬 Common Insecure Design Patterns

| Pattern | Risk | Secure Alternative |
|---------|------|-------------------|
| Trusting client-side validation | Bypass all checks | Server-side validation |
| No rate limiting | Brute force, scraping | Rate limits, CAPTCHA |
| Unlimited data export | Mass data theft | Pagination, export limits |
| No re-auth for sensitive ops | Account takeover | Step-up authentication |
| Shared secrets for all users | One leak = all compromised | Per-user secrets |
| No abuse case analysis | Business logic attacks | Threat modeling |

## 🎯 Threat Modeling (Interview Favorite!)

**STRIDE Model:**
| Threat | Description | Example |
|--------|-------------|---------|
| **S**poofing | Pretending to be someone else | Fake login page |
| **T**ampering | Modifying data/code | Changing prices in cart |
| **R**epudiation | Denying actions | No audit logs |
| **I**nformation Disclosure | Exposing data | Error messages with stack traces |
| **D**enial of Service | Making system unavailable | No rate limiting |
| **E**levation of Privilege | Gaining unauthorized access | [[Privilege Escalation]] |

## 🛡️ Secure Design Principles

| Principle | Implementation |
|-----------|---------------|
| Defense in depth | Multiple security layers |
| Least privilege | Minimal permissions by default |
| Fail securely | Deny access on error |
| Zero trust | Verify every request |
| Separation of duties | No single point of compromise |
| Secure defaults | Security out-of-the-box |

## 🤖 AI-Specific Design Risks (2025)

| Risk | Description |
|------|-------------|
| Prompt injection | User input mixed with system prompts |
| Blind trust in output | Acting on AI decisions without validation |
| Model poisoning | Backdoored models from untrusted sources |
| Data leakage | Sensitive data in prompts/training |

## 🎤 Interview STAR Example
> **Situation:** E-commerce app had no purchase limits—attackers exploited promo codes to get unlimited discounts.
> **Task:** Redesign the promotion system with abuse prevention.
> **Action:** Conducted threat modeling session with dev team. Identified 12 abuse cases. Implemented per-user promo limits, velocity checks (max 3 promos/hour), and fraud scoring. Added monitoring for unusual redemption patterns.
> **Result:** Fraudulent promo abuse dropped by 94%. System flagged and blocked a bot attack within 5 minutes of launch.

## 🔗 Related Concepts
- [[Security Misconfigurations]]
- [[Broken Access Control]]
- [[Authentication Failures]]
- [[Threat Modeling]]

## 📚 References
- OWASP Insecure Design
- OWASP Threat Modeling
- Microsoft STRIDE
