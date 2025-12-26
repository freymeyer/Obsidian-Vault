---
tags:
  - "#cybersecurity/red-team/recon"
  - "#cybersecurity/osint"
  - "#interview/concepts"
aliases:
  - Shodan.io
---

# Shodan

> **One-liner:** A search engine for internet-connected devices that indexes banners, services, and metadata from exposed systems worldwide.

## 🎯 What Is It?
Shodan is often called "the search engine for hackers." Unlike Google which indexes web pages, Shodan crawls the internet scanning ports and collecting **service banners** from every device it can reach. This creates a searchable database of exposed servers, IoT devices, industrial control systems, databases, and more.

**URL:** https://www.shodan.io/

## 🔍 How It Works

### Data Collection
```
Shodan Scanners → Port Scan → Banner Grab → Index in Database
```

Shodan continuously scans the entire IPv4 space, connecting to common ports and storing:
- Service banners (version strings, headers)
- SSL certificates
- Screenshot of web interfaces
- Geolocation data
- ASN and hosting provider
- Metadata and vulnerabilities

### Information Retrieved
| Data Type | Examples |
|-----------|----------|
| **IP Address** | Target server locations |
| **Open Ports** | 22, 80, 443, 3389, etc. |
| **Services** | Apache 2.4.41, OpenSSH 7.9 |
| **Organization** | Hosting company, ISP |
| **Geolocation** | Country, city, coordinates |
| **Vulnerabilities** | Known CVEs for detected versions |
| **SSL/TLS** | Certificate details, expiration |

### Search Syntax
```bash
# Find Apache servers
apache

# Find Apache in a specific country
apache country:"US"

# Search by organization
org:"Microsoft"

# Find specific port open
port:3389

# Search by hostname
hostname:example.com

# Find servers with specific vulnerability
vuln:CVE-2021-44228

# Combine filters
apache port:8080 country:"DE"
```

## ⚔️ Offensive Use Cases

### Reconnaissance
- Map target's internet-facing infrastructure
- Identify software versions for exploit matching
- Find forgotten or misconfigured services
- Discover IoT devices and weak points

### Common Searches
```bash
# Exposed webcams
webcam has_screenshot:true

# Default credentials on routers
"default password"

# Exposed MongoDB databases
mongodb port:27017

# Industrial control systems
"Schneider Electric"

# Exposed Elasticsearch
port:9200 elasticsearch
```

## 🛡️ Detection & Prevention (Blue Team)

### Detection
- **You cannot detect Shodan scanning specifically** — it's passive recon from your perspective
- Monitor for service enumeration patterns in logs
- Use threat intel feeds that track Shodan scanner IPs

### Prevention Strategies
- **Minimize Exposure** — Only expose necessary services
- **Update Software** — Patch to remove known vulnerabilities
- **Change Default Banners** — Obscure version information
- **Use Shodan Monitor** — Track your own exposed assets
- **Firewall** — Block unnecessary inbound connections
- **VPN/Zero Trust** — Put services behind authentication

### Blue Team Usage
```
1. Search your organization on Shodan
2. Identify exposed services you weren't aware of
3. Verify nothing sensitive is internet-facing
4. Set up Shodan Monitor alerts for your IP ranges
```

## 🎤 Interview Angles

### Common Questions
- **Q: What makes Shodan different from Google?**
  - A: Google indexes web pages; Shodan indexes internet-connected devices by scanning ports and collecting service banners.

- **Q: How would you use Shodan during a penetration test?**
  - A: Search for the target's IP ranges or hostname to discover exposed services, software versions, and potential vulnerabilities without touching their systems.

- **Q: As a defender, how would you use Shodan?**
  - A: Search for your organization to discover what's exposed, set up monitoring alerts, identify shadow IT and misconfigured services.

### STAR Example
> **Situation:** During a security assessment, the client believed they had minimal internet exposure.
> **Task:** Verify their external attack surface using passive reconnaissance.
> **Action:** Searched Shodan for their IP range and organization name.
> **Result:** Discovered 12 exposed services including an unpatched Jenkins server and an open MongoDB instance — neither on their asset inventory. Led to immediate remediation.

## 🔗 Related Concepts
- [[Passive Reconnaissance]]
- [[Open Source Intelligence (OSINT)]]
- [[DNSDumpster]]
- [[Port Scanning]]
- [[Vulnerability Scanning]]

## 📚 References
- https://www.shodan.io/
- https://help.shodan.io/the-basics/search-query-fundamentals
- TryHackMe: Shodan.io Room
