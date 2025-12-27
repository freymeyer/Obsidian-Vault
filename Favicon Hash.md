---
tags:
  - "#cybersecurity/osint"
  - "#cybersecurity/web-security"
  - "#interview/concepts"
aliases:
  - Favicon Fingerprinting
  - mmh3 Hash
---

# Favicon Hash

> **One-liner:** A unique cryptographic hash of a website's favicon used to fingerprint web applications and identify specific software or infrastructure.

## 🎯 What Is It?
A **favicon hash** is a numerical fingerprint (typically using [[MurmurHash3]] algorithm) of a website's favicon.ico file. Since many web applications, frameworks, and platforms ship with default favicons, the hash acts as a unique signature to identify:
- Content management systems (WordPress, Joomla)
- Web frameworks (Django, Laravel)
- Infrastructure software (Jenkins, Grafana, SolarWinds)
- Custom applications

Search engines like [[Shodan]] index favicon hashes, allowing defenders and attackers to find all instances of specific software across the internet.

## 🤔 Why It Matters

### For Offensive Security
- **Software identification** — Instantly know what CMS or framework a site uses
- **Target discovery** — Find all instances of vulnerable software (e.g., SolarWinds Orion)
- **Attack surface mapping** — Identify forgotten admin panels or staging servers
- **Evasion detection** — Verify if a site is what it claims to be

### For Defensive Security
- **Shadow IT discovery** — Find unauthorized instances of corporate software
- **Vulnerability management** — Locate all deployments of a vulnerable application
- **Incident response** — Track malicious infrastructure using known favicon hashes
- **Asset inventory** — Discover forgotten or unmanaged web applications

### Real-World Example: SolarWinds Hack (2020)
After the SolarWinds supply chain attack, researchers used the SolarWinds Orion favicon hash to find all vulnerable instances:
```bash
http.favicon.hash:-1776962843
```

## 🔬 How It Works

### Favicon Hash Generation
```
1. Download favicon.ico from target
2. Encode to Base64
3. Calculate MurmurHash3 (32-bit)
4. Result: A signed integer hash
```

### Calculation Example
```python
import mmh3
import requests
import codecs

# Download favicon
response = requests.get('https://example.com/favicon.ico')
favicon = codecs.encode(response.content, "base64")

# Calculate hash
hash_value = mmh3.hash(favicon)
print(f"Favicon hash: {hash_value}")
```

### Shodan Usage
```bash
# Search by favicon hash
http.favicon.hash:-1776962843

# Find all SolarWinds Orion instances
http.favicon.hash:-1776962843

# Find all Jenkins instances
http.favicon.hash:81586312

# Find all Grafana dashboards
http.favicon.hash:-1272756243
```

## 🛠️ Tools & Techniques

### Calculate Favicon Hash Manually
```bash
# Using Python
python3 -c "import mmh3, requests, codecs; print(mmh3.hash(codecs.encode(requests.get('https://target.com/favicon.ico').content,'base64')))"

# Using FavFreak (GitHub tool)
python3 favfreak.py -u https://target.com
```

### Shodan Favicon Searches
```bash
# Find specific software
http.favicon.hash:81586312 # Jenkins
http.favicon.hash:-1272756243 # Grafana
http.favicon.hash:116323821 # Fortinet VPN
http.favicon.hash:-1233880039 # Microsoft Exchange

# Combine with other filters
http.favicon.hash:81586312 country:"US"
http.favicon.hash:-1776962843 org:"Target Company"
```

### Censys Favicon Search
```bash
# Censys also indexes favicons
services.http.response.favicons.md5_hash:<hash>
```

## 📊 Common Favicon Hashes

| Hash | Software | Risk Level |
|------|----------|------------|
| `-1776962843` | SolarWinds Orion | High (post-breach) |
| `81586312` | Jenkins | High (often misconfigured) |
| `-1272756243` | Grafana | Medium |
| `116323821` | Fortinet VPN | High (if unpatched) |
| `-1233880039` | Microsoft Exchange | High (ProxyLogon, etc.) |
| `999357577` | Cisco ASA | Medium |
| `-1153089216` | F5 BIG-IP | Medium |

## 🛡️ Detection & Prevention

### How to Detect Reconnaissance
- **Monitor for favicon requests** from scanner IPs
- Track unusual User-Agent strings accessing `/favicon.ico`
- Alert on bulk favicon downloads across your network
- **Note:** Shodan/Censys scanning is passive from your perspective

### How to Prevent / Mitigate

#### Change Default Favicons
```html
<!-- Replace default favicon with custom one -->
<link rel="icon" type="image/png" href="/custom-favicon.png">
```

#### Hide Favicons Behind Authentication
```nginx
# Nginx example
location = /favicon.ico {
    auth_basic "Restricted";
    auth_basic_user_file /etc/nginx/.htpasswd;
}
```

#### Use Generic Favicons
- Don't use software-default favicons on production systems
- Create custom favicons for internal/admin tools
- Rotate favicons periodically for high-value assets

#### Blue Team Strategies
```bash
# Monitor your own favicon hashes on Shodan
http.favicon.hash:<your_hash> org:"Your Company"

# Set up alerts for new instances
# Use Shodan Monitor to track your favicon hashes
```

## 🎤 Interview Angles

### Common Questions
- **Q: What is a favicon hash and why is it useful for reconnaissance?**
  - A: It's a MurmurHash3 fingerprint of a website's favicon. Since many apps ship with default favicons, the hash identifies specific software. Shodan indexes these, letting you find all instances of a vulnerable application globally.

- **Q: How was favicon hashing used in the SolarWinds incident?**
  - A: After the SolarWinds Orion breach, researchers calculated the Orion favicon hash (`-1776962843`) and searched Shodan to find all vulnerable instances worldwide, helping organizations identify exposure.

- **Q: How would you prevent favicon-based fingerprinting?**
  - A: Use custom favicons instead of defaults, place favicons behind authentication for sensitive apps, and regularly audit what's exposed on Shodan using your organization's favicon hashes.

### STAR Example
> **Situation:** A security team needed to identify all exposed Jenkins instances across their global infrastructure to patch a critical RCE vulnerability.
> **Task:** Quickly discover all Jenkins servers without doing an internal network scan that might disrupt services.
> **Action:** Calculated the Jenkins favicon hash (`81586312`) and searched Shodan with `http.favicon.hash:81586312 org:"[Company]"`. Found 12 exposed Jenkins instances, 5 of which were not in the asset inventory.
> **Result:** Patched all instances within 24 hours. Prevented potential exploitation of vulnerable CI/CD infrastructure.

## ✅ Best Practices
- **Search yourself before attackers do** — Find your exposed apps on Shodan
- Change default favicons on admin panels and internal tools
- Use [[Shodan]] Monitor to track your favicon hashes
- Document favicon hashes for critical applications in your asset inventory
- Combine with [[Shodan Dorking]] for comprehensive recon

## ❌ Common Misconceptions
- **"Changing favicon makes me invisible"** — No, attackers use many fingerprinting methods (headers, content, TLS certs)
- **"Only attackers use favicon hashes"** — Security teams use them for asset discovery and vulnerability management
- **"Favicon hashing is active scanning"** — No, querying Shodan is passive; the scanning already happened

## 🔗 Related Concepts
- [[Shodan]]
- [[Shodan Dorking]]
- [[Banner Grabbing]]
- [[Passive Reconnaissance]]
- [[Open Source Intelligence (OSINT)]]
- [[Fingerprinting]]
- [[MurmurHash3]]

## 📚 References
- https://github.com/devanshbatham/FavFreak — Favicon Hash Tool
- https://www.shodan.io/search?query=http.favicon.hash — Shodan Favicon Search
- https://medium.com/@Phocean/shodan-favicon-search-9b5c3e2b8e40 — Favicon Search Tutorial
- SolarWinds Favicon Hash: `-1776962843`
