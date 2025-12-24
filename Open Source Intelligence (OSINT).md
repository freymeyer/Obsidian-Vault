---
tags:
  - "#cybersecurity/red-team/reconnaissance"
  - "#cybersecurity/blue-team/threat-intel"
  - "#interview/concepts"
aliases:
  - OSINT
---

# Open Source Intelligence (OSINT)

> **One-liner:** The collection and analysis of publicly available information to produce actionable intelligence.

## 🎯 What Is It?
Open Source Intelligence (OSINT) is the practice of gathering information from publicly accessible sources. In cybersecurity, OSINT is used by both attackers (for [[Reconnaissance (Cyber Security)|reconnaissance]]) and defenders (for threat intelligence and investigations).

## 🔬 How It Works

### Common OSINT Sources

| Category | Sources | Information Gained |
|----------|---------|-------------------|
| **Domain** | WHOIS, DNS, crt.sh | Ownership, subdomains, IPs |
| **Social** | LinkedIn, Twitter/X, GitHub | Employees, tech stack, credentials |
| **Technical** | Shodan, Censys, VirusTotal | Exposed services, malware hashes |
| **Documents** | Google, Pastebin, S3 buckets | Leaked creds, internal docs |
| **Dark Web** | Forums, markets | Breached data, threat intel |

### OSINT Process
```
1. Define → What information do you need?
2. Collect → Gather from multiple sources
3. Process → Clean and organize data
4. Analyze → Extract actionable insights
5. Disseminate → Report findings
```

## 🛠️ Common Tools

```bash
# theHarvester - Email/subdomain gathering
theHarvester -d target.com -b google,linkedin

# Maltego - Visual link analysis (GUI)

# Recon-ng - Modular reconnaissance framework
recon-cli
use recon/domains-hosts/hackertarget
set SOURCE target.com
run

# SpiderFoot - Automated OSINT
spiderfoot -s target.com -m all
```

| Tool | Purpose |
|------|---------|
| **theHarvester** | Email, subdomain enumeration |
| **Maltego** | Visual relationship mapping |
| **Shodan** | Internet-connected device search |
| **Censys** | Certificate and host discovery |
| **Recon-ng** | Modular OSINT framework |
| **SpiderFoot** | Automated OSINT scanning |
| **Have I Been Pwned** | Breach checking |

## 🛡️ Detection & Prevention

### How to Detect (as a Target)
- Most passive OSINT is **undetectable**
- Monitor for mentions of your org in paste sites/dark web
- Track certificate transparency logs for unauthorized certs

### How to Prevent / Mitigate
- Minimize public footprint (WHOIS privacy, limited social media)
- Regular attack surface assessments
- Employee security awareness training
- Remove sensitive documents from public access
- Use canary tokens in sensitive locations

## 🎤 Interview Angles

### Common Questions
- "Walk me through your OSINT methodology"
- "What tools would you use to gather intel on a target organization?"
- "How can organizations reduce their OSINT exposure?"

### Key Talking Points
- OSINT is legal (uses only public info)
- Distinguish between OSINT and active reconnaissance
- Blue team uses OSINT for threat intel and brand monitoring

## ✅ Best Practices
- Document all sources for attribution
- Verify information from multiple sources
- Use VPN/Tor for operational security during collection
- Respect privacy boundaries and legal limits
- Automate recurring checks with tools like SpiderFoot

## 🔗 Related Concepts
- [[Reconnaissance (Cyber Security)]]
- [[Google Dorking]]
- [[Social Engineering]]
- [[Cyber Kill Chain]]

## 📚 References
- OSINT Framework (osintframework.com)
- SANS OSINT Resources
