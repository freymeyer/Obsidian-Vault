---
dg-publish: true
tags:
  - "#cybersecurity/red-team/recon"
  - "#interview/concepts"
aliases:
  - Active Recon
---

# Active Reconnaissance

> **One-liner:** Gathering information about a target by directly interacting with their systems, which risks detection.

## 🎯 What Is It?
Active reconnaissance involves directly probing and interacting with a target's systems to gather information. Unlike [[Passive Reconnaissance]], active recon leaves traces and can trigger security alerts because you're actively connecting to the target.

Think of it as walking up to a building and testing the doors and windows — you might be seen.

## 🔍 How It Works

### Common Techniques
| Technique | Tool Examples | Data Retrieved |
|-----------|---------------|----------------|
| **Port Scanning** | nmap, masscan | Open ports, services |
| **Service Enumeration** | nmap -sV, banner grabbing | Software versions |
| **Vulnerability Scanning** | Nessus, OpenVAS | Known vulnerabilities |
| **Web Crawling** | Burp Spider, dirb | Hidden directories, pages |
| **OS Fingerprinting** | nmap -O | Operating system info |

### Key Tools
```bash
# Port scan
nmap -sS -p- target.com

# Service version detection
nmap -sV -p 80,443 target.com

# OS fingerprinting
nmap -O target.com

# Directory brute-forcing
gobuster dir -u http://target.com -w /usr/share/wordlists/common.txt
```

## ⚔️ Active vs Passive Reconnaissance

| Aspect | Active | Passive |
|--------|--------|---------|
| **Detection Risk** | High | None |
| **Target Interaction** | Direct | None |
| **Legal Risk** | Requires authorization | Generally safe |
| **Examples** | Port scan, ping sweep | WHOIS lookup, Google search |
| **Data Quality** | Current, detailed | May be outdated |

## 🛡️ Detection & Prevention (Blue Team)

### Detection Methods
- **IDS/IPS Alerts** — Detect port scans, vulnerability probes
- **Firewall Logs** — Monitor connection attempts
- **Web Application Firewalls** — Detect directory brute-forcing
- **Honeypots** — Trap attackers during enumeration
- **Rate Limiting** — Slow down scanning attempts

### Prevention
- Minimize exposed services
- Use non-standard ports where appropriate
- Implement fail2ban for repeated connection attempts
- Deploy honeypots to detect and study attackers
- Regular security audits to identify what's exposed

## 🎤 Interview Angles

### Common Questions
- **Q: What's the legal consideration for active reconnaissance?**
  - A: You must have explicit written authorization. Unauthorized scanning can violate computer crime laws.

- **Q: How can you make active recon stealthier?**
  - A: Slow down scans, use fragmented packets, scan from multiple IPs, avoid signature-based detection patterns.

- **Q: What's the difference between a SYN scan and a connect scan?**
  - A: SYN scan (-sS) sends SYN, receives SYN-ACK, sends RST — never completes handshake, stealthier. Connect scan (-sT) completes full TCP handshake, more detectable but works without root.

### STAR Example
> **Situation:** During an authorized pentest, I needed to map the client's external attack surface.
> **Task:** Perform active reconnaissance while minimizing detection by their SOC team.
> **Action:** Used slow nmap scans with timing option -T2, randomized target order, and scanned from multiple source IPs over several days.
> **Result:** Mapped 47 open services across 12 hosts without triggering a single SOC alert, demonstrating gaps in their detection capabilities.

## 🔗 Related Concepts
- [[Passive Reconnaissance]]
- [[Nmap]]
- [[Port Scanning]]
- [[Vulnerability Scanning]]
- [[Cyber Kill Chain]]
- [[Social Engineering]]

## 📚 References
- TryHackMe: Active Reconnaissance Room
- PTES - Vulnerability Analysis Phase
