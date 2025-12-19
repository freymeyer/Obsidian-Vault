---
tags:
  - "#cybersecurity/vuln/crypto-failure"
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - Sensitive Data Exposure
  - Crypto Failures
severity: High
owasp: A04
---

# Cryptographic Failure

> **One-liner:** Sensitive data exposed due to weak/missing encryption, poor key management, or using broken algorithms.

## 🎯 What Is It?
This is **A04** of [[OWASP]]. Cryptographic failures occur when sensitive data isn't properly protected—either not encrypted, using weak algorithms, or with poor key management.

## 💥 Why It Matters (Impact)
- **Confidentiality:** Passwords, credit cards, PII exposed
- **Integrity:** Data can be modified without detection
- **Compliance:** PCI DSS, GDPR, HIPAA violations

## 📊 Common Failure Patterns

| Failure | Risk | Fix |
|---------|------|-----|
| No encryption at rest | Database dump = full breach | AES-256-GCM |
| No encryption in transit | MITM attacks | TLS 1.3 |
| Weak hashing (MD5, SHA1) | Password cracking | bcrypt, Argon2 |
| Hardcoded secrets | Secrets in Git history | Vault, KMS |
| Weak algorithms (DES, RC4) | Cryptanalysis attacks | AES, ChaCha20 |
| Poor key management | Keys never rotated | [[Key Management Lifecycle (KML)]] |
| Rolling own crypto | Guaranteed vulnerabilities | Use proven libraries |

## 🔬 Vulnerable vs Secure Examples

```python
# ❌ VULNERABLE: MD5 for passwords
import hashlib
hashed = hashlib.md5(password.encode()).hexdigest()  # Crackable in seconds!

# ✅ SECURE: bcrypt with salt
import bcrypt
hashed = bcrypt.hashpw(password.encode(), bcrypt.gensalt(rounds=12))
```

```python
# ❌ VULNERABLE: Hardcoded API key
API_KEY = "sk-1234567890abcdef"  # In source code!

# ✅ SECURE: Environment variable or secrets manager
import os
API_KEY = os.environ.get('API_KEY')  # Or use HashiCorp Vault, AWS Secrets Manager
```

## 🔐 Encryption Standards (2025)

| Use Case | Recommended | Avoid |
|----------|-------------|-------|
| Symmetric encryption | AES-256-GCM, ChaCha20-Poly1305 | DES, 3DES, RC4, ECB mode |
| Password hashing | Argon2id, bcrypt, scrypt | MD5, SHA1, SHA256 (plain) |
| TLS | TLS 1.3, TLS 1.2 (strong ciphers) | SSL, TLS 1.0, TLS 1.1 |
| Key exchange | ECDH (Curve25519), RSA-2048+ | RSA-1024, DH-1024 |
| Signing | Ed25519, ECDSA, RSA-PSS | RSA-PKCS1v1.5, DSA |

## 🛡️ Prevention Checklist

| Control | Implementation |
|---------|---------------|
| Classify data | Know what's sensitive, encrypt appropriately |
| Encrypt at rest | Database, backups, logs |
| Encrypt in transit | TLS everywhere, HSTS |
| Strong algorithms | Follow NIST/OWASP recommendations |
| Key management | [[Hashicorp Vault]], AWS KMS, Azure Key Vault |
| Regular rotation | [[Key Rotation]], [[Cryptoperiod]] |
| No hardcoded secrets | Environment vars, secrets managers |

## 🎤 Interview STAR Example
> **Situation:** Security audit found passwords stored as unsalted MD5 hashes in production database.
> **Task:** Migrate to secure password storage without disrupting 50,000 users.
> **Action:** Implemented bcrypt with cost factor 12. Created migration that re-hashed passwords on next login. Forced password reset for inactive accounts after 90 days. Added password breach checking against HaveIBeenPwned.
> **Result:** 100% migration to bcrypt within 60 days. No user-facing incidents during transition.

## 🔗 Related Concepts
- [[Key Management Lifecycle (KML)]]
- [[Hashicorp Vault]]
- [[Public Key Infrastructure]]
- [[Asymmetric key distribution]]
- [[Symmetric key distribution]]

## 📚 References
- OWASP Cryptographic Failures
- OWASP Cryptographic Storage Cheat Sheet
- NIST Cryptographic Standards
