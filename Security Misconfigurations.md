---
dg-publish: true
tags:
  - "#cybersecurity/web-sec/server-side"
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - Security Misconfiguration
  - Misconfigs
severity: High
owasp: A02
---

# Security Misconfigurations

> **One-liner:** Insecure defaults, incomplete configurations, or exposed services that create easy entry points for attackers.

## 🎯 What Is It?
This is **A02** of [[OWASP]]. Security misconfigurations happen when systems are deployed with unsafe defaults, missing patches, or exposed services. These aren't code bugs—they're deployment mistakes.

## 💥 Why It Matters (Impact)
- **Confidentiality:** Exposed admin panels, open S3 buckets
- **Integrity:** Default creds allow system modification
- **Availability:** Unpatched services get exploited

## 📊 Common Misconfigurations

| Category | Misconfiguration | Risk |
|----------|-----------------|------|
| **Defaults** | Admin/admin credentials | Full system access |
| **Cloud** | Public S3/Blob storage | Data exposure |
| **Services** | Exposed management ports | Direct system access |
| **Errors** | Stack traces in responses | Info disclosure |
| **Headers** | Missing security headers | XSS, clickjacking |
| **Patches** | Outdated software | Known CVE exploitation |
| **Permissions** | Overly permissive IAM | Privilege escalation |

## 💰 Real-World Example: Uber 2017

> In 2017, [Uber](https://www.huntress.com/threat-library/data-breach/uber-data-breach) exposed an AWS S3 bucket with sensitive user data because it was publicly accessible. Attackers downloaded data directly without credentials.

## 🔬 Misconfigurations to Check

```bash
# Common exposed services
nmap -sV --script=default target.com

# Check security headers
curl -I https://target.com | grep -iE "x-frame|x-xss|x-content|strict-transport|content-security"

# AWS S3 bucket check
aws s3 ls s3://bucket-name --no-sign-request

# Default creds to test (with permission!)
admin:admin, admin:password, root:root, test:test
```

## 🔐 Security Headers Checklist

| Header | Purpose |
|--------|---------|
| `Content-Security-Policy` | Prevent XSS, injection |
| `X-Frame-Options` | Prevent clickjacking |
| `X-Content-Type-Options` | Prevent MIME sniffing |
| `Strict-Transport-Security` | Force HTTPS |
| `X-XSS-Protection` | Legacy XSS filter |

## 🛡️ Prevention Checklist

| Control | Implementation |
|---------|---------------|
| ☐ Harden defaults | Remove default accounts, change default ports |
| ☐ Minimal install | Remove unused features, services, frameworks |
| ☐ Patch management | Automated updates, vulnerability scanning |
| ☐ Cloud audit | Regular review of IAM, storage permissions |
| ☐ Security headers | CSP, X-Frame-Options, HSTS |
| ☐ Error handling | Generic errors to users, detailed logs internally |
| ☐ Automated scanning | Infrastructure-as-code scanning, CSPM |

## 🎤 Interview STAR Example
> **Situation:** Cloud security audit found 47 S3 buckets with public read access.
> **Task:** Identify exposed data and remediate all misconfigurations.
> **Action:** Used AWS Config rules to identify all public buckets. Reviewed contents—found 3 with PII. Removed public access, implemented bucket policies requiring encryption. Set up automated alerts for public bucket creation.
> **Result:** Achieved 100% private buckets within 48 hours. Prevented future exposure with preventive controls.

## 🔗 Related Concepts
- [[Insecure Design]]
- [[Cryptographic Failure]]
- [[Privilege Escalation]]

## 📚 References
- OWASP Security Misconfiguration
- CIS Benchmarks: https://www.cisecurity.org/cis-benchmarks
