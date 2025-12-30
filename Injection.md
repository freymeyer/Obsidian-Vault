---
dg-publish: true
tags:
  - "#cybersecurity/web-sec/injection"
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - Injection Attacks
severity: Critical
owasp: A05
---

# Injection

> **One-liner:** Untrusted data sent to an interpreter as part of a command or query, allowing attackers to execute unintended commands.

## 🎯 What Is It?
This is **A05** of [[OWASP]]. Injection occurs when user input is passed directly to an interpreter (SQL, OS, LDAP, etc.) without proper sanitization, allowing attackers to alter the intended command.

## 💥 Why It Matters (Impact)
- **Confidentiality:** Extract entire databases
- **Integrity:** Modify/delete data, create admin accounts
- **Availability:** Drop tables, crash systems

## 📊 Types of Injection

| Type | Target | Example Payload |
|------|--------|-----------------|
| [[SQL Injection]] | Databases | `' OR 1=1 --` |
| [[Command Injection]] | OS Shell | `; cat /etc/passwd` |
| [[Server Side Template Injection (SSTI)]] | Template engines | `{{7*7}}` |
| LDAP Injection | Directory services | `*)(&` |
| XPath Injection | XML queries | `' or '1'='1` |
| NoSQL Injection | MongoDB, etc. | `{"$gt": ""}` |
| AI Prompt Injection | LLMs | `Ignore previous instructions...` |

## 🔬 SQL Injection Example

```python
# ❌ VULNERABLE: String concatenation
query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"
# Attack: username = "admin'--" → Bypasses password check!

# ✅ SECURE: Parameterized query
cursor.execute(
    "SELECT * FROM users WHERE username = ? AND password = ?",
    (username, password)
)
```

## 🔬 Command Injection Example

```python
# ❌ VULNERABLE: Direct shell execution
import os
os.system(f"ping {user_input}")  
# Attack: user_input = "8.8.8.8; cat /etc/passwd"

# ✅ SECURE: Use subprocess with list arguments
import subprocess
subprocess.run(["ping", "-c", "4", user_input], shell=False)
```

## 🔍 How to Test

| Injection | Test Payloads |
|-----------|---------------|
| SQL | `'`, `"`, `' OR 1=1--`, `1; DROP TABLE--` |
| Command | `;`, `\|`, `&&`, `$(command)` |
| SSTI | `{{7*7}}`, `${7*7}`, `<%= 7*7 %>` |
| NoSQL | `{"$ne": null}`, `{"$gt": ""}` |

## 🛡️ Prevention

| Control | Implementation |
|---------|---------------|
| Parameterized queries | NEVER concatenate user input |
| Input validation | Allowlist expected patterns |
| Escape special chars | Context-aware encoding |
| Least privilege | DB accounts with minimal perms |
| WAF | Block common injection patterns |
| Prepared statements | Use ORM/query builders |

## 🎤 Interview STAR Example
> **Situation:** Pentest found SQL injection in login form allowing authentication bypass.
> **Task:** Fix the vulnerability and prevent future injection issues.
> **Action:** Converted all raw SQL queries to parameterized statements using ORM. Implemented input validation middleware. Added WAF rules for common injection patterns. Created secure coding training for dev team.
> **Result:** Zero injection findings in subsequent pentests. Reduced overall vulnerability count by 60%.

## 📈 Notable Injection Breaches

| Year | Target | Impact |
|------|--------|--------|
| 2023 | MOVEit | 2,700+ orgs via SQLi |
| 2017 | Equifax | 147M records |
| 2011 | Sony | 77M accounts |

## 🔗 Related Concepts
- [[SQL Injection]]
- [[Command Injection]]
- [[Server Side Template Injection (SSTI)]]
- [[Cross-Site Scripting (XSS)]]

## 📚 References
- OWASP Injection Prevention Cheat Sheet
- PortSwigger SQL Injection: https://portswigger.net/web-security/sql-injection
