---
tags:
  - "#cybersecurity/osint"
  - "#infrastructure/networking"
  - "#interview/concepts"
aliases:
  - ASN
---

# Autonomous System Number (ASN)

> **One-liner:** A globally unique identifier assigned to a collection of IP address ranges managed by a single organization.

## 🎯 What Is It?
An **Autonomous System Number (ASN)** is a unique identifier (typically 16 or 32-bit) assigned by regional internet registries (RIRs) to organizations that manage a large block of IP addresses and routing policies. Think of it as an "area code" for internet infrastructure—Google, Amazon, universities, and ISPs all have their own ASNs.

ASNs are fundamental to [[Border Gateway Protocol (BGP)|BGP]] routing, allowing networks to identify and exchange routing information with each other.

## 🤔 Why It Matters

### For Reconnaissance
During [[Passive Reconnaissance]] or penetration testing, discovering a target's ASN allows you to:
- **Map all IP addresses** owned by an organization
- **Find shadow IT** — Forgotten servers in the same network block
- **Discover infrastructure** — Servers, IoT devices, databases
- **Identify hosting providers** — Cloud vs on-prem infrastructure

### For Blue Team
- Track your organization's exposed attack surface
- Monitor all IPs in your ASN for vulnerabilities
- Detect unauthorized services on your IP ranges
- Investigate attribution during incident response

## 🔬 How It Works

### ASN Structure
```
AS[Number]
Examples:
  AS15169  → Google
  AS16509  → Amazon (AWS)
  AS14061  → DigitalOcean
  AS13335  → Cloudflare
```

### ASN Lookup Process
```
1. Get target IP address
   ├─ ping target.com
   └─ nslookup target.com

2. Query ASN lookup service
   ├─ https://asnlookup.com/
   ├─ https://bgp.he.net/
   └─ WHOIS query

3. Search Shodan/Censys for ASN
   ├─ Shodan: asn:AS14061
   └─ Discover all services on that ASN
```

### Tools for ASN Lookup

| Tool | Command | Output |
|------|---------|--------|
| **WHOIS** | `whois -h whois.cymru.com <IP>` | ASN, Country, Owner |
| **BGP Toolkit** | https://bgp.he.net/ | Routes, Peers, Prefixes |
| **ASN Lookup** | https://asnlookup.com/ | ASN details, IP ranges |
| **Shodan** | `asn:AS15169` | All services on ASN |

## 🛠️ Practical Usage

### Recon with ASN
```bash
# 1. Get IP of target
ping tryhackme.com
# Response: 104.26.11.229

# 2. Lookup ASN
whois -h whois.cymru.com 104.26.11.229
# Output: AS13335 | Cloudflare

# 3. Search Shodan for all IPs in that ASN
# Visit: https://www.shodan.io/search?query=asn:AS13335
```

### Finding Organization's Assets
```bash
# Search for MySQL databases on a specific ASN
asn:AS14061 product:MySQL

# Find web servers on company ASN
asn:AS15169 port:443

# Discover webcams on network
asn:AS15169 webcam
```

## 🛡️ Detection & Prevention

### How to Detect Reconnaissance
- ASN queries are passive—no direct detection possible
- Monitor for [[Shodan]] scans hitting your IP ranges
- Track unusual WHOIS lookups in DNS/firewall logs

### How to Prevent / Mitigate
- **Minimize exposed services** — Only publish necessary ports
- **Monitor your own ASN** — Use [[Shodan]] Monitor to track your exposure
- **Network segmentation** — Don't expose everything on the same IP block
- **Update software** — Patch vulnerable services discovered via ASN searches
- **Implement WAF/IDS** — Detect and block reconnaissance patterns

## 🎤 Interview Angles

### Common Questions
- **Q: What is an ASN and why is it useful for reconnaissance?**
  - A: An ASN is a unique identifier for an organization's IP address ranges. During recon, knowing a target's ASN lets you discover all their internet-facing infrastructure without directly scanning them—using search engines like Shodan.

- **Q: How would you find a company's ASN?**
  - A: First, resolve their domain to an IP using `ping` or `nslookup`. Then query an ASN lookup tool like whois.cymru.com or bgp.he.net to find the ASN. Finally, search Shodan with `asn:AS[number]` to map all exposed services.

- **Q: Can ASNs help with incident response?**
  - A: Yes—during IR, identifying the attacker's ASN can reveal their hosting provider and possibly other infrastructure they control. It also helps attribute attacks to specific threat actors.

### STAR Example
> **Situation:** A client wanted to understand their external attack surface but had no comprehensive asset inventory.
> **Task:** Map all internet-facing infrastructure without active scanning that might disrupt services.
> **Action:** Identified their ASN via WHOIS, then searched Shodan using `asn:AS[number]` to discover all exposed services. Cross-referenced with their IT asset list.
> **Result:** Discovered 8 forgotten servers including an unpatched Windows Server 2012 instance. Client immediately decommissioned or patched them, reducing attack surface by 30%.

## ✅ Best Practices
- Always check ASN ownership before assuming it's the target's infrastructure (e.g., Cloudflare ASN ≠ target's actual servers)
- Use ASN searches as **passive recon**—no packets sent to target
- Combine ASN data with [[Google Dorking]] and certificate transparency logs
- Document ASN findings in recon reports with timestamps

## ❌ Common Misconceptions
- **"ASN lookup is active scanning"** — No, it's passive. You're querying third-party databases, not the target.
- **"All IPs in an ASN belong to one company"** — Not always. Hosting providers (AWS, DigitalOcean) have thousands of customers on their ASN.
- **"ASN search reveals everything"** — Only shows internet-facing services. Internal infrastructure remains hidden.

## 🔗 Related Concepts
- [[Shodan]]
- [[Border Gateway Protocol (BGP)]]
- [[Passive Reconnaissance]]
- [[Open Source Intelligence (OSINT)]]
- [[WHOIS]]
- [[IP Address]]

## 📚 References
- https://bgp.he.net/ — BGP Toolkit
- https://asnlookup.com/ — ASN Lookup Tool
- https://www.shodan.io/search — Search by ASN
- ARIN, RIPE, APNIC — Regional Internet Registries
