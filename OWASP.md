---
tags:
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - Open Web Application Security Project
  - OWASP Top 10
---

# OWASP

> **One-liner:** A nonprofit foundation providing free resources, tools, and standards to improve software security worldwide.

## 🎯 What Is It?
The **Open Web Application Security Project (OWASP)** is a community-driven organization focused on improving software security. Their most famous resource is the **OWASP Top 10** — a list of the most critical web application security risks.

## 🔟 OWASP Top 10 (2025)

| Rank | Vulnerability                           | One-Liner (Memorize These!)                          |
| ---- | --------------------------------------- | ---------------------------------------------------- |
| A01  | [[Broken Access Control]]               | Users act outside their intended permissions         |
| A02  | [[Security Misconfigurations]]          | Insecure default configs, open cloud storage         |
| A03  | [[Software Supply Chain Failures]]      | Compromised dependencies, malicious packages         |
| A04  | [[Cryptographic Failure]]               | Sensitive data exposed via weak/missing encryption   |
| A05  | [[Injection]]                           | Untrusted data sent to interpreter (SQLi, XSS, etc.) |
| A06  | [[Insecure Design]]                     | Missing security controls at design phase            |
| A07  | [[Authentication Failures]]             | Broken auth, weak passwords, session issues          |
| A08  | [[Software or Data Integrity Failures]] | Code/data integrity not verified (CI/CD, updates)    |
| A09  | [[Logging & Alerting Failures]]         | Insufficient logging for breach detection            |
| A10  | [[Server-Side Request Forgery (SSRF)]]  | Server fetches attacker-controlled URLs              |

## 📊 Quick Reference by Attack Type

| Category | Vulnerabilities |
|----------|----------------|
| **Access Issues** | A01 Broken Access Control, A07 Auth Failures |
| **Injection** | A05 Injection (SQLi, XSS, SSTI, Command) |
| **Data Protection** | A04 Cryptographic Failure |
| **Design/Config** | A02 Misconfigs, A06 Insecure Design |
| **Supply Chain** | A03 Supply Chain, A08 Integrity Failures |
| **Ops/Detection** | A09 Logging Failures |
| **Server-Side** | A10 SSRF |

## 🎤 Interview Quick Hits

**"What is OWASP?"**
> A nonprofit providing free security resources. Their Top 10 is the industry standard for web app risks.

**"Walk me through the Top 10"**
> Start with: "A01 is Broken Access Control — the #1 risk because it's the most common..."

**"Which is most critical?"**
> A01 Broken Access Control — found in 94% of applications tested. Users accessing data/functions they shouldn't.

## 🛠️ Other OWASP Resources

| Resource | Purpose |
|----------|---------|
| OWASP Testing Guide | Pentest methodology |
| OWASP ASVS | Application Security Verification Standard |
| OWASP Cheat Sheets | Quick reference for secure coding |
| OWASP ZAP | Free web app scanner |
| OWASP Dependency-Check | Find vulnerable libraries |
| OWASP SAMM | Security maturity model |

## 🎤 Interview STAR Example
> **Situation:** Development team had no security standards for code reviews.
> **Task:** Implement a security review process.
> **Action:** Introduced OWASP Top 10 training for developers. Created a checklist based on OWASP Testing Guide. Integrated OWASP ZAP into CI/CD pipeline for automated scanning.
> **Result:** Reduced critical vulnerabilities in production by 70% over 6 months. Team could identify and fix issues before deployment.

## 🔗 Related Concepts
- [[Cross-Site Scripting (XSS)]]
- [[SQL Injection]]
- [[Insecure Direct Object Reference (IDOR)]]
- [[Server-Side Request Forgery (SSRF)]]
- [[Cross-site request forgery (CSRF)]]

## 📚 References
- OWASP Top 10: https://owasp.org/Top10/
- OWASP Cheat Sheets: https://cheatsheetseries.owasp.org/
- OWASP Testing Guide: https://owasp.org/www-project-web-security-testing-guide/


