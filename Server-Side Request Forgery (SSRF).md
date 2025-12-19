---
tags:
  - "#cybersecurity/web-sec/server-side"
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - SSRF
severity: High
owasp: A10
---

# Server-Side Request Forgery (SSRF)

> **One-liner:** Attacker tricks the server into making requests to unintended locations, accessing internal resources or external systems.

## 🎯 What Is It?
This is **A10** of [[OWASP]]. SSRF occurs when an attacker can control URLs that the server-side application fetches. The server becomes a proxy for attacking internal systems.

## 💥 Why It Matters (Impact)
- **Confidentiality:** Read internal files, cloud metadata, internal APIs
- **Integrity:** Modify internal systems via internal APIs
- **Availability:** DoS internal services, port scanning

**Famous Breach:** Capital One (2019) — SSRF to AWS metadata endpoint led to 100M+ records exposed.

## 📊 SSRF Attack Types

| Type | Target | Example |
|------|--------|--------|
| Basic | Internal services | `http://localhost:8080/admin` |
| Cloud Metadata | AWS/GCP/Azure metadata | `http://169.254.169.254/latest/meta-data/` |
| File Access | Local files | `file:///etc/passwd` |
| Port Scanning | Internal network | Loop through ports |
| Protocol Smuggling | Other protocols | `gopher://`, `dict://` |

## 🔬 Vulnerable Code Example

```python
# ❌ VULNERABLE: User controls the URL
@app.route('/fetch')
def fetch_url():
    url = request.args.get('url')
    response = requests.get(url)  # Server fetches attacker's URL!
    return response.text

# Attack: /fetch?url=http://169.254.169.254/latest/meta-data/iam/security-credentials/
```

```python
# ✅ SECURE: Allowlist and validation
import ipaddress
from urllib.parse import urlparse

ALLOWED_HOSTS = ['api.trusted.com', 'cdn.trusted.com']

@app.route('/fetch')
def fetch_url():
    url = request.args.get('url')
    parsed = urlparse(url)
    
    # Validate scheme
    if parsed.scheme not in ['http', 'https']:
        abort(400)
    
    # Validate host against allowlist
    if parsed.hostname not in ALLOWED_HOSTS:
        abort(400)
    
    # Resolve and check for internal IPs
    ip = socket.gethostbyname(parsed.hostname)
    if ipaddress.ip_address(ip).is_private:
        abort(400)
    
    return requests.get(url).text
```

## 🎯 Common Targets

```bash
# AWS Metadata (IMDSv1)
http://169.254.169.254/latest/meta-data/
http://169.254.169.254/latest/meta-data/iam/security-credentials/

# GCP Metadata
http://metadata.google.internal/computeMetadata/v1/

# Azure Metadata
http://169.254.169.254/metadata/instance?api-version=2021-02-01

# Internal Services
http://localhost:8080/admin
http://127.0.0.1:6379/ (Redis)
http://internal-api.corp:8080/
```

## 🔍 Bypass Techniques (Know for Defense)

| Bypass | Example |
|--------|--------|
| Decimal IP | `http://2130706433/` (127.0.0.1) |
| IPv6 | `http://[::1]/` |
| URL encoding | `http://127.0.0.1%2f@evil.com` |
| DNS rebinding | Domain resolves to internal IP |
| Redirects | External URL redirects to internal |

## 🛡️ Prevention

| Control | Implementation |
|---------|---------------|
| Allowlist | Only permit known-good hosts |
| Block internal ranges | 127.0.0.0/8, 10.0.0.0/8, 169.254.0.0/16 |
| Disable redirects | Don't follow HTTP redirects |
| IMDSv2 | Require token for AWS metadata |
| Network segmentation | App servers can't reach sensitive internal |
| WAF rules | Block metadata IPs in requests |

## 🎤 Interview STAR Example
> **Situation:** Vulnerability scan found SSRF in our PDF generation feature that fetched URLs.
> **Task:** Remediate the vulnerability and prevent similar issues.
> **Action:** Implemented URL allowlisting for known CDN domains only. Added IP validation blocking all private ranges. Migrated AWS instances to IMDSv2 with required tokens. Added WAF rule blocking 169.254.x.x in request parameters.
> **Result:** Eliminated SSRF risk. Even if bypassed, IMDSv2 prevents metadata access without token.

## 🔗 Related Concepts
- [[Command Injection]]
- [[Injection]]
- [[Security Misconfigurations]]

## 📚 References
- OWASP SSRF Prevention Cheat Sheet
- PortSwigger SSRF: https://portswigger.net/web-security/ssrf
