---
tags:
  - "#cybersecurity/red-team/reconnaissance"
  - "#cybersecurity/blue-team/detection"
  - "#interview/concepts"
aliases:
  - Recon
  - Information Gathering
---

# Reconnaissance (Cyber Security)

> **One-liner:** The first phase of an attack where adversaries gather information about a target to identify vulnerabilities and entry points.

## 🎯 What Is It?
Reconnaissance is the initial stage of the [[Cyber Kill Chain]] where attackers collect information about their target's systems, infrastructure, personnel, and security posture. The goal is to discover potential weaknesses that can be exploited in later stages.

## 📊 Types of Reconnaissance

| Type | Description | Detection Risk | Examples |
|------|-------------|----------------|----------|
| **Passive** | No direct interaction with target | Very Low | WHOIS, DNS queries, [[Open Source Intelligence (OSINT)\|OSINT]] |
| **Active** | Direct interaction with target systems | Higher | Port scanning, vulnerability scanning |

## 🔬 How It Works

### Passive Reconnaissance
Gathering information without alerting the target:

```
Public Sources
├── WHOIS databases (domain ownership, contacts)
├── DNS records (subdomains, mail servers, IPs)
├── Job postings (tech stack, security tools)
├── Social media (employee info, org structure)
├── [[Google Dorking]] (exposed files, misconfigs)
└── Certificate Transparency logs
```

**Tools:** Shodan, Censys, theHarvester, Maltego, Recon-ng

### Active Reconnaissance
Direct interaction that may trigger detection:

```bash
# Port Scanning
nmap -sV -sC target.com

# Subdomain enumeration
gobuster dns -d target.com -w wordlist.txt

# Web directory bruteforce
ffuf -u https://target.com/FUZZ -w wordlist.txt
```

**Tools:** [[Nmap]], Masscan, Nikto, dirb, gobuster

## 🛡️ Detection & Prevention

### How to Detect
- Monitor for port scans via firewall/IDS logs
- Analyze web server logs for enumeration patterns
- Watch for unusual DNS query volumes
- Alert on failed authentication attempts (credential stuffing recon)

### How to Prevent / Mitigate
- Minimize public information exposure
- Use WHOIS privacy protection
- Implement rate limiting on public services
- Remove sensitive files from public directories
- Use honeypots to detect reconnaissance

## 🎤 Interview Angles

### Common Questions
- "What's the difference between passive and active reconnaissance?"
- "What tools would you use for reconnaissance in a pentest?"
- "How would you detect reconnaissance activity as a defender?"

### STAR Story
> **Situation:** During a security assessment, needed to map attack surface without alerting the client prematurely.
> **Task:** Perform comprehensive passive reconnaissance first.
> **Action:** Used OSINT techniques—WHOIS, certificate transparency, job postings, and Google dorking—before any active scanning.
> **Result:** Discovered an exposed admin panel and outdated Jenkins server through passive means alone, demonstrating attack surface risk without triggering any alarms.

## ✅ Best Practices
- **Red Team:** Start passive, go active only when necessary
- **Blue Team:** Assume reconnaissance is ongoing; minimize exposure
- Regular external attack surface assessments
- Monitor threat intelligence for mentions of your org

## 🔗 Related Concepts
- [[Cyber Kill Chain]]
- [[Open Source Intelligence (OSINT)]]
- [[Google Dorking]]
- [[Nmap]]
- [[Social Engineering]]

## 📚 References
- MITRE ATT&CK - Reconnaissance Techniques
- PTES - Penetration Testing Execution Standard
