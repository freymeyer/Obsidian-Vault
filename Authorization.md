---
dg-publish: true
tags:
  - "#cybersecurity/iam/access-control"
  - "#interview/concepts"
aliases:
  - AuthZ
---

# Authorization

> **One-liner:** The process of verifying *what you can do* after your identity is confirmed.

## 🎯 What Is It?
Authorization (AuthZ) is the security process that determines what resources and actions an authenticated user can access. It answers the question: **"What are you allowed to do?"**

## 📊 Authorization Models

| Model | Description | Use Case |
|-------|-------------|----------|
| **DAC** (Discretionary) | Owner controls access | File systems (chmod) |
| **MAC** (Mandatory) | System enforces labels | Military, government |
| **RBAC** (Role-Based) | Access via roles | Enterprise apps |
| **ABAC** (Attribute-Based) | Access via attributes | Cloud, dynamic policies |
| **ReBAC** (Relationship) | Access via relationships | Social networks, graphs |

## 🛡️ RBAC Deep Dive
```
User → Role → Permission → Resource

Example:
Alice → "Editor" → [read, write] → /documents/*
Bob   → "Viewer" → [read]        → /documents/*
```

## ❗ Authentication vs Authorization

| Authentication | Authorization |
|----------------|---------------|
| WHO are you? | WHAT can you do? |
| Happens first | Happens after AuthN |
| Validates identity | Validates permissions |
| Example: Login | Example: Access admin panel |

## 🚨 Common Authorization Vulnerabilities
- [[Broken Access Control]] — OWASP #1 (2021)
- [[Insecure Direct Object Reference (IDOR)]] — Accessing others' data
- [[Privilege Escalation]] — Gaining higher permissions
- [[Authorization Bypass]] — Skipping permission checks
- Missing function-level access control

## 🎤 Interview STAR Example
> **Situation:** Users could access other users' invoices by changing the ID in the URL.
> **Task:** Identify the vulnerability and implement proper access controls.
> **Action:** Identified IDOR vulnerability. Implemented server-side authorization checks verifying user ownership before returning data. Added access control tests.
> **Result:** Eliminated unauthorized data access. Passed subsequent pentest with no authorization findings.

## ✅ Best Practices
- Always enforce authorization **server-side**
- Use [[Principle of Least Privilege]]
- Implement deny-by-default
- Log all authorization failures
- Regularly audit permissions

## 🔗 Related Concepts
- [[Authentication]]
- [[Attribute-Based Access Control (ABAC)]]
- [[Broken Access Control]]
- [[Insecure Direct Object Reference (IDOR)]]
- [[Privilege Escalation]]

## 📚 References
- OWASP Authorization Cheat Sheet
- NIST Access Control Guidelines