---
tags:
  - "#cybersecurity/red-team/recon"
  - "#infrastructure/networking/dns"
  - "#interview/concepts"
aliases:
  - Name Server Lookup
---

# nslookup

> **One-liner:** A command-line tool for querying DNS servers to retrieve domain name records.

## 🎯 What Is It?
nslookup (Name Server Lookup) is a network administration tool for querying the Domain Name System to obtain domain-to-IP mapping and other DNS records. It's available on Windows, Linux, and macOS, making it a universal tool for DNS troubleshooting and [[Passive Reconnaissance]].

## 🔍 How It Works

### Basic Syntax
```bash
nslookup [OPTIONS] DOMAIN_NAME [SERVER]
```

### DNS Record Types
| Query Type | Result | Use Case |
|------------|--------|----------|
| **A** | IPv4 addresses | Find web server IPs |
| **AAAA** | IPv6 addresses | Find IPv6 addresses |
| **MX** | Mail servers | Identify email infrastructure |
| **TXT** | Text records | SPF, DKIM, verification tokens |
| **CNAME** | Canonical name | Find aliases |
| **SOA** | Start of Authority | Zone transfer info |
| **NS** | Name servers | Identify authoritative DNS |

### Command Examples
```bash
# Basic lookup (A record)
nslookup example.com

# Query specific record type
nslookup -type=MX example.com

# Query specific DNS server
nslookup example.com 8.8.8.8

# Get all records
nslookup -type=ANY example.com

# Query TXT records (useful for finding flags, SPF, etc.)
nslookup -type=TXT example.com

# Interactive mode
nslookup
> set type=MX
> example.com
```

### Example Output
```
$ nslookup -type=A tryhackme.com 1.1.1.1
Server:     1.1.1.1
Address:    1.1.1.1#53

Non-authoritative answer:
Name:   tryhackme.com
Address: 172.67.69.208
Name:   tryhackme.com
Address: 104.26.11.229
```

## ⚔️ Offensive Use Cases
- **IP Discovery** — Map domains to IP addresses
- **Mail Server Enumeration** — Find MX records for phishing targets
- **Subdomain Hints** — TXT records may reveal subdomains
- **Cloud Provider Identification** — IP ranges indicate AWS, Azure, etc.
- **SPF Record Analysis** — Identify authorized mail senders

## 🛡️ Detection & Prevention (Blue Team)
- DNS queries to public servers are **not detectable** by the target
- Monitor internal DNS for excessive queries (potential enumeration)
- Use DNS firewalls to block known malicious domains
- Review TXT records for sensitive information leakage

## 🆚 nslookup vs dig

| Feature | nslookup | dig |
|---------|----------|-----|
| **Output** | Simple, human-readable | Detailed, technical |
| **TTL Display** | Not by default | Always shown |
| **Scripting** | Less suitable | Better for scripts |
| **Availability** | Windows, Linux, macOS | Primarily Linux/macOS |
| **Deprecation** | Considered legacy | Preferred tool |

## 🎤 Interview Angles

### Common Questions
- **Q: How would you find the mail servers for a domain?**
  - A: `nslookup -type=MX domain.com`

- **Q: What's the difference between nslookup and dig?**
  - A: dig provides more detailed output including TTL by default and is better for scripting. nslookup is simpler and works natively on Windows.

- **Q: What does "non-authoritative answer" mean?**
  - A: The response came from a DNS cache, not directly from the authoritative name server for that domain.

## 🔗 Related Concepts
- [[Domain Information Groper (dig)]]
- [[Domain Name System (DNS)]]
- [[Passive Reconnaissance]]
- [[WHOIS]]
- [[DNSDumpster]]

## 📚 References
- TryHackMe: Passive Reconnaissance Room
- Microsoft nslookup Documentation
