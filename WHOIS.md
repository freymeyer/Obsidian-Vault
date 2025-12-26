---
tags:
  - "#cybersecurity/red-team/recon"
  - "#infrastructure/networking/dns"
  - "#interview/concepts"
aliases:
  - whois
  - WHOIS Protocol
---

# WHOIS

> **One-liner:** A query protocol for retrieving registration information about domain names, IP addresses, and autonomous systems.

## 🎯 What Is It?
WHOIS is a request-response protocol defined in RFC 3912 that queries databases containing information about registered domain names and IP addresses. WHOIS servers listen on **TCP port 43** and return details about domain ownership, registration dates, and name servers.

## 🔍 How It Works

### Query Flow
```
Client → WHOIS Server (TCP 43) → Response with registration data
```

### Information Retrieved
| Field | Description |
|-------|-------------|
| **Registrar** | Company where domain was registered (e.g., Namecheap, GoDaddy) |
| **Registrant** | Domain owner's contact information |
| **Creation Date** | When domain was first registered |
| **Expiration Date** | When registration expires |
| **Updated Date** | Last modification date |
| **Name Servers** | DNS servers authoritative for the domain |
| **Status Codes** | Domain lock status, transfer restrictions |

### Command Usage
```bash
# Basic query
whois example.com

# Query specific WHOIS server
whois -h whois.verisign-grs.com example.com

# Query for IP address
whois 8.8.8.8

# Verbose output
whois -v example.com
```

### Example Output
```
Domain Name: EXAMPLE.COM
Registry Domain ID: 2336799_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.iana.org
Updated Date: 2023-08-14T07:01:38Z
Creation Date: 1995-08-14T04:00:00Z
Registrar Registration Expiration Date: 2024-08-13T04:00:00Z
Registrar: IANA
Name Server: A.IANA-SERVERS.NET
Name Server: B.IANA-SERVERS.NET
```

## ⚔️ Offensive Use Cases
- **Reconnaissance** — Identify domain owner, tech/admin contacts
- **Social Engineering** — Use contact info for phishing
- **Attack Surface Mapping** — Find related domains by same registrant
- **Expiration Monitoring** — Hijack domains that expire
- **Infrastructure Analysis** — Identify name server providers

## 🛡️ Detection & Prevention (Blue Team)

### Privacy Protections
- **WHOIS Privacy Services** — Mask registrant information
- **GDPR Impact** — Many registrars redact personal data
- **Domain Locking** — Prevent unauthorized transfers

### Blue Team Considerations
- Monitor your domains' WHOIS for unauthorized changes
- Use privacy protection to limit OSINT exposure
- Set up domain expiration alerts
- Review what information is publicly exposed

## 🎤 Interview Angles

### Common Questions
- **Q: What port does WHOIS use?**
  - A: TCP port 43.

- **Q: What can you learn from a WHOIS query during a pentest?**
  - A: Registrar, contact info (unless privacy-protected), creation/expiration dates, name servers, and related domains.

- **Q: How has GDPR affected WHOIS?**
  - A: Many registrars now redact personal information from WHOIS responses to comply with data protection regulations.

## 🔗 Related Concepts
- [[Passive Reconnaissance]]
- [[Domain Name System (DNS)]]
- [[nslookup]]
- [[Domain Information Groper (dig)]]
- [[Open Source Intelligence (OSINT)]]

## 📚 References
- RFC 3912 - WHOIS Protocol Specification
- ICANN WHOIS Primer
