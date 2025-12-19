---
tags:
  - "#cybersecurity/vuln/auth-bypass"
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - Broken Authentication
severity: High
owasp: A07
---

# Authentication Failures

> **One-liner:** Application can't reliably verify a user's identity, allowing attackers to compromise accounts.

## 🎯 What Is It?
This is **A07** of [[OWASP]], part of failures in [[Identification, Authentication, Authorization, and Accountability (IAAA)]]. Authentication failures occur when login mechanisms can be bypassed, brute-forced, or manipulated.

## 💥 Why It Matters (Impact)
- **Confidentiality:** Account takeover, access to private data
- **Integrity:** Attacker can act as the victim
- **Availability:** Mass account lockouts via abuse

## 📊 Common Vulnerability Patterns

| Vulnerability | Description | Attack |
|--------------|-------------|--------|
| Username enumeration | Different responses for valid/invalid users | Harvest valid usernames |
| Weak passwords | No complexity requirements | [[Dictionary Attacks]] |
| No rate limiting | Unlimited login attempts | [[Brute-force]] |
| Credential stuffing | Accepts known breached creds | Automated attacks |
| Session fixation | Session not rotated on login | Hijack pre-auth session |
| Insecure "Remember Me" | Predictable tokens | Token prediction |
| Weak recovery | Security questions, email-only | Account takeover |

## 🔬 Vulnerable Code Examples

```python
# ❌ Username Enumeration
if not user_exists(username):
    return "User not found"  # Reveals valid usernames!
if not check_password(password):
    return "Wrong password"

# ✅ Generic Error Message
if not authenticate(username, password):
    return "Invalid credentials"  # Same message for both cases
```

```python
# ❌ No Rate Limiting
@app.route('/login', methods=['POST'])
def login():
    return check_credentials(request.form)  # Unlimited attempts!

# ✅ With Rate Limiting
@app.route('/login', methods=['POST'])
@limiter.limit("5 per minute")
def login():
    return check_credentials(request.form)
```

## 🔍 How to Test

1. **Enumeration:** Try valid vs invalid usernames, compare responses
2. **Brute Force:** Use [[Hydra]] or Burp Intruder, check for lockout
3. **Session Testing:** Check if session ID changes after login
4. **Password Reset:** Test for account enumeration, token predictability

## 🛡️ Prevention

| Control | Implementation |
|---------|---------------|
| [[Multi-Factor Authentication (MFA)]] | TOTP, hardware keys |
| Rate limiting | 5 attempts, then lockout/CAPTCHA |
| Generic errors | Same message for all auth failures |
| Secure sessions | Rotate session ID on auth state change |
| Password policies | Length > complexity, breach checking |
| Secure recovery | Time-limited tokens, MFA verification |

## 🎤 Interview STAR Example
> **Situation:** Application had no rate limiting; attackers were brute-forcing accounts.
> **Task:** Stop ongoing attacks and implement proper authentication controls.
> **Action:** Deployed immediate IP-based rate limiting via WAF. Implemented application-level rate limiting (5 attempts/15 min). Added account lockout with email notification. Rolled out MFA for all users.
> **Result:** Brute-force attacks dropped to zero. Account takeovers reduced by 99%.

## 🔗 Related Concepts
- [[Authentication]]
- [[Brute-force]]
- [[Dictionary Attacks]]
- [[Multi-Factor Authentication (MFA)]]
- [[Session]]

## 📚 References
- OWASP Authentication Cheat Sheet
- NIST 800-63B Digital Identity Guidelines