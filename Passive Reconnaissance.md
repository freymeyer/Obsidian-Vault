---
tags:
  - "#cybersecurity/red-team/recon"
  - "#cybersecurity/osint"
  - "#interview/concepts"
aliases:
  - Passive Recon
  - OSINT Recon
---

# Passive Reconnaissance

> **One-liner:** Gathering information about a target using publicly available resources without directly interacting with the target's systems.

## 🎯 What Is It?
Passive reconnaissance is the first phase of the [[Cyber Kill Chain]] where an attacker collects information about a target without alerting them. Unlike [[Active Reconnaissance]], passive recon leaves no traces on the target's systems because you never directly connect to them.

Think of it as observing a building from across the street rather than walking up and testing the locks.

## 🔍 How It Works

### Information Sources
| Source Type | Examples | Data Retrieved |
|-------------|----------|----------------|
| **DNS Records** | A, AAAA, MX, TXT, CNAME | IP addresses, mail servers, SPF records |
| **WHOIS** | Domain registrar databases | Registrant info, creation dates, name servers |
| **Search Engines** | Google, Bing, DuckDuckGo | Exposed documents, employee info |
| **Social Media** | LinkedIn, Twitter, Facebook | Employee names, tech stack hints |
| **Public Databases** | [[Shodan]], Censys, [[DNSDumpster]] | Open ports, services, subdomains |

### Key Tools

| Tool | Purpose | Example Command |
|------|---------|-----------------|
| `whois` | Query domain registration info | `whois example.com` |
| `nslookup` | Query DNS records | `nslookup -type=MX example.com` |
| `dig` | Advanced DNS queries | `dig example.com A +short` |
| [[DNSDumpster]] | Subdomain enumeration | Web-based |
| [[Shodan]] | Internet-connected device search | Web-based / API |

## ⚔️ Passive vs Active Reconnaissance

| Aspect | Passive | Active |
|--------|---------|--------|
| **Detection Risk** | None | High |
| **Target Interaction** | None | Direct |
| **Legal Risk** | Generally safe | May be illegal |
| **Examples** | WHOIS lookup, Google search | Port scan, vulnerability scan |
| **Data Quality** | Publicly available | Detailed, current |

## 🛡️ Detection & Prevention (Blue Team)
- **You can't detect passive recon** — the attacker never touches your systems
- Monitor for your organization's data in paste sites and breached databases
- Use privacy protection services for domain registration
- Limit information exposed on social media and job postings
- Review and minimize public DNS records

## 🛠️ Passive Recon Methodology
```
1. WHOIS Lookup → Domain ownership, registrar, expiration
2. DNS Enumeration → A, MX, TXT records, name servers
3. Subdomain Discovery → DNSDumpster, crt.sh, Amass
4. Search Engine Recon → Google dorks, cached pages
5. Shodan/Censys → Exposed services, banners
6. Social Media OSINT → Employee names, tech stack
7. Document Metadata → Author names, software versions
```

## 🎤 Interview Angles

### Common Questions
- **Q: What's the difference between passive and active reconnaissance?**
  - A: Passive uses public info without touching target systems; active directly probes the target and can be detected.

- **Q: Name five sources for passive reconnaissance.**
  - A: WHOIS records, DNS lookups, Shodan, social media, job postings.

- **Q: Why is passive recon important before active recon?**
  - A: It reduces the attack surface you need to probe, identifies potential targets, and minimizes detection risk.

### STAR Example
> **Situation:** During a penetration test engagement, I needed to map the target's external attack surface before active testing.
> **Task:** Perform comprehensive passive reconnaissance to identify all publicly exposed assets.
> **Action:** Used WHOIS for domain info, dig/nslookup for DNS records, DNSDumpster for subdomains, and Shodan for exposed services. Discovered an unpatched mail server and a forgotten dev subdomain.
> **Result:** Identified 3 additional entry points not in the original scope discussion, leading to discovery of a critical vulnerability in the dev environment.

## 🔗 Related Concepts
- [[Active Reconnaissance]]
- [[Open Source Intelligence (OSINT)]]
- [[WHOIS]]
- [[nslookup]]
- [[Domain Information Groper (dig)]]
- [[DNSDumpster]]
- [[Shodan]]
- [[Cyber Kill Chain]]
- [[Google Dorking]]

## 📚 References
- TryHackMe: Passive Reconnaissance Room
- PTES (Penetration Testing Execution Standard) - Intelligence Gathering
