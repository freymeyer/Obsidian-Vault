---
tags:
  - "#cybersecurity/web-sec/access-control"
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - BAC
  - Access Control Vulnerabilities
severity: Critical
owasp: A01
---

# Broken Access Control

> **One-liner:** Users can act outside their intended permissions—accessing, modifying, or deleting data they shouldn't.

## 🎯 What Is It?
This is **A01** of [[OWASP]] — the #1 most critical web vulnerability. It occurs when the server doesn't properly enforce **who can access what** on every request, part of failures in [[Identification, Authentication, Authorization, and Accountability (IAAA)]].

## 💥 Why It Matters (Impact)
- **Confidentiality:** Access other users' private data
- **Integrity:** Modify or delete others' records
- **Availability:** Delete critical resources

**Stats:** Found in 94% of applications tested (OWASP 2021)

## 📊 Types of Access Control Failures

| Type | Description | Example |
|------|-------------|--------|
| [[Privilege Escalation#Vertical privilege escalation\|Vertical]] | User → Admin access | Accessing `/admin` panel as regular user |
| [[Privilege Escalation#Horizontal privilege escalation\|Horizontal]] | User A → User B's data | Changing `?user_id=123` to `?user_id=456` |
| [[Insecure Direct Object Reference (IDOR)]] | Direct object manipulation | `/api/invoice/7` → `/api/invoice/8` |
| Missing Function Level | No authZ on sensitive endpoints | POST to `/api/deleteUser` works for anyone |

## 🔬 Vulnerable Code Example

```python
# ❌ VULNERABLE: No authorization check
@app.route('/api/user/<user_id>/profile')
def get_profile(user_id):
    return db.get_user(user_id)  # Anyone can access any profile!

# ✅ SECURE: Server-side authorization
@app.route('/api/user/<user_id>/profile')
@login_required
def get_profile(user_id):
    if current_user.id != user_id and not current_user.is_admin:
        abort(403)  # Forbidden
    return db.get_user(user_id)
```

## 🔍 How to Test

1. **IDOR Testing:** Change IDs in URLs, request bodies, headers
2. **Forced Browsing:** Try accessing admin URLs directly
3. **Method Tampering:** Change GET to POST/PUT/DELETE
4. **Parameter Pollution:** Add duplicate params `?role=user&role=admin`

## 🛡️ Prevention

| Control | Implementation |
|---------|---------------|
| Deny by default | Require explicit grants for all resources |
| Server-side checks | NEVER trust client-side authorization |
| Centralized authZ | Use middleware/interceptors |
| Audit logging | Log all access control failures |
| Automated testing | Include authZ tests in CI/CD |

## 🎤 Interview STAR Example
> **Situation:** Pentest revealed users could access other users' invoices by changing the invoice ID in the URL.
> **Task:** Fix the IDOR vulnerability and prevent similar issues.
> **Action:** Implemented server-side ownership checks verifying `invoice.owner_id == current_user.id`. Created authorization middleware for all API endpoints. Added automated IDOR tests to CI pipeline.
> **Result:** Fixed vulnerability within 24 hours. No access control findings in subsequent pentests.

## 🔗 Related Concepts
- [[Insecure Direct Object Reference (IDOR)]]
- [[Privilege Escalation]]
- [[Authorization]]
- [[Authentication]]

## 📚 References
- OWASP Broken Access Control: https://owasp.org/Top10/A01_2021-Broken_Access_Control/
- OWASP Access Control Cheat Sheet