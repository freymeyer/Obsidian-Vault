---
tags:
  - "#cybersecurity/osint"
  - "#cybersecurity/red-team/recon"
  - "#interview/concepts"
aliases:
  - Shodan Hacking
---

# Shodan Dorking

> **One-liner:** Using advanced search filters and operators on Shodan.io to discover specific vulnerable devices, services, or misconfigurations across the internet.

## 🎯 What Is It?
**Shodan Dorking** (similar to [[Google Dorking]]) is the technique of crafting targeted search queries on [[Shodan]] using filters, operators, and keywords to find:
- Vulnerable systems (CVE exploits)
- Misconfigured devices (default credentials)
- Industrial control systems (SCADA, ICS)
- Exposed databases and cameras
- Specific software versions or banners

The term "dork" refers to a pre-crafted search query that reveals something specific—like all internet-connected printers, or devices vulnerable to EternalBlue.

## 🤔 Why It Matters

### For Offensive Security
- **Rapid reconnaissance** — Find vulnerable targets instantly
- **CVE exploitation** — Discover unpatched systems
- **IoT hunting** — Locate exposed cameras, routers, smart devices
- **Banner analysis** — Identify software versions for exploit matching

### For Defensive Security
- **Attack surface mapping** — See what attackers see
- **Shadow IT discovery** — Find forgotten or unauthorized devices
- **Vulnerability assessment** — Identify your exposed services before attackers do
- **Compliance validation** — Ensure nothing sensitive is internet-facing

## 🔬 How It Works

### Shodan Filters
Shodan uses **filters** to narrow down search results:

| Filter | Description | Example |
|--------|-------------|---------|
| `hostname:` | Search by hostname | `hostname:example.com` |
| `port:` | Find devices with specific port open | `port:3389` |
| `country:` | Filter by country code | `country:"US"` |
| `city:` | Filter by city | `city:"London"` |
| `os:` | Operating system | `os:"Windows 7"` |
| `product:` | Software/service name | `product:"Apache"` |
| `vuln:` | Known CVE vulnerability | `vuln:CVE-2017-0144` |
| `asn:` | Autonomous System Number | `asn:AS15169` |
| `org:` | Organization name | `org:"Google"` |
| `has_screenshot:` | Devices with web interface screenshot | `has_screenshot:true` |
| `http.favicon.hash:` | Favicon hash (fingerprinting) | `http.favicon.hash:-1776962843` |

### Combining Filters
```bash
# Basic search
apache

# Apache in US
apache country:"US"

# Apache version 2.4 on port 8080
product:"Apache" "2.4" port:8080

# MySQL databases in Germany on DigitalOcean
product:"MySQL" country:"DE" org:"DigitalOcean"

# Windows RDP servers
port:3389 os:"Windows"
```

## 🛠️ Famous Shodan Dorks

### Ransomware-Infected Machines
```bash
has_screenshot:true encrypted attention
```
Uses OCR ([[Optical Character Recognition]]) to find ransomware messages on remote desktops.

### Industrial Control Systems (ICS)
```bash
screenshot.label:ics

# Or specific ICS vendors
"Siemens" "PLC"
"Schneider Electric"
```

### Vulnerable to EternalBlue (MS17-010)
```bash
vuln:MS17-010
```
**Note:** `vuln:` filter requires a premium account.

### Exposed Webcams
```bash
webcam has_screenshot:true

# Or specific webcam brands
"IP Camera" "DVR"
```

### Default Credentials
```bash
"default password"
"admin:admin"
```

### Exposed Databases
```bash
# MongoDB
product:"MongoDB"

# Elasticsearch
port:9200 product:"Elasticsearch"

# Redis
product:"Redis"
```

### SolarWinds Supply Chain Attack Detection
```bash
http.favicon.hash:-1776962843
```
Identifies SolarWinds Orion instances by their unique favicon hash.

### Misconfigured Cloud Storage
```bash
"Index of /" "aws s3"
```

### ICS/SCADA Systems
```bash
port:502 Modbus
port:102 Siemens
```

## 🛡️ Detection & Prevention

### How to Detect
- **You cannot directly detect Shodan scans** — They're passive from your perspective
- Monitor for unusual connection patterns across many IPs
- Use threat intel feeds that track Shodan scanner IPs
- Alert on services that shouldn't be internet-facing

### How to Prevent / Mitigate
- **Search yourself first** — Use Shodan to find your own exposed assets
- **Remove unnecessary services** — Don't expose databases, ICS, or admin panels
- **Use Shodan Monitor** — Get alerts when new services appear on your IP ranges
- **Change default credentials** — Immediately upon deployment
- **Update software** — Patch CVEs that Shodan can detect
- **Implement VPN/zero trust** — Put sensitive services behind authentication
- **Hide banners** — Modify service banners to obscure version information

### Blue Team Shodan Usage
```bash
# Monitor your organization
org:"Your Company Name"

# Check your ASN
asn:AS[YourNumber]

# Look for your IP ranges
net:203.0.113.0/24

# Find exposed admin panels
org:"Your Company" "admin" "login"
```

## 🎤 Interview Angles

### Common Questions
- **Q: What is Shodan Dorking and how does it differ from Google Dorking?**
  - A: Shodan Dorking uses search filters on Shodan.io to find exposed devices and services by querying Shodan's database of internet-connected systems. Unlike Google Dorking (which searches web pages), Shodan searches service banners, ports, and device metadata.

- **Q: How would you use Shodan during a pentest?**
  - A: First, I'd search for the target's organization or ASN to map their internet-facing infrastructure. Then use filters like `vuln:`, `product:`, and `port:` to identify potentially vulnerable services. Finally, correlate findings with CVE databases for exploitation.

- **Q: What's the ethical/legal concern with Shodan Dorking?**
  - A: Viewing information on Shodan is legal (it's publicly accessible). However, **accessing** those systems without authorization is illegal. It's a reconnaissance tool—what you do after finding something determines legality.

### STAR Example
> **Situation:** A financial client was concerned about exposed internal systems after a data breach at a competitor.
> **Task:** Identify any internet-facing databases or admin panels belonging to the client.
> **Action:** Used Shodan dorks like `org:"[Client Name]" product:"MySQL"` and `port:3389 org:"[Client Name]"`. Found 3 exposed MySQL instances and 2 RDP servers with weak passwords.
> **Result:** Client immediately secured the databases and disabled RDP access. Prevented potential breach that could have exposed customer financial data.

## ✅ Best Practices
- Always have written authorization before accessing discovered systems
- Use Shodan for **passive recon only**—don't exploit findings without permission
- Document all Shodan queries and results with timestamps
- Combine with [[CVE]] databases to assess exploitability
- Use [[Shodan]] Monitor for continuous asset discovery

## ❌ Common Misconceptions
- **"Shodan scanning is hacking"** — Querying Shodan is legal. Accessing systems without permission is not.
- **"Only hackers use Shodan"** — Security teams, researchers, and companies use it for asset management and vulnerability assessment.
- **"Shodan reveals everything"** — Only shows internet-facing services. Internal networks remain hidden unless exposed.

## 🔗 Related Concepts
- [[Shodan]]
- [[Google Dorking]]
- [[Banner Grabbing]]
- [[Passive Reconnaissance]]
- [[Autonomous System Number (ASN)]]
- [[Open Source Intelligence (OSINT)]]
- [[Vulnerability Scanning]]

## 📚 References
- https://www.shodan.io/explore — Popular Shodan Dorks
- https://github.com/jakejarvis/awesome-shodan-queries — Curated Shodan Dorks
- https://help.shodan.io/the-basics/search-query-fundamentals — Official Shodan Search Guide
- TryHackMe: Shodan.io Room
