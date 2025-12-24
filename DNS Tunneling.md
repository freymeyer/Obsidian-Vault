---
tags:
  - "#cybersecurity/red-team/evasion"
  - "#cybersecurity/blue-team/detection"
  - "#interview/concepts"
aliases:
  - DNS Exfiltration
  - DNS Covert Channel
---

# DNS Tunneling

> **One-liner:** Technique that encodes data within DNS queries and responses to bypass security controls and establish covert communication channels.

## 🎯 What Is It?
DNS Tunneling abuses the [[Domain Name System (DNS)]] protocol to tunnel data that wouldn't normally be carried by DNS. Attackers encode data (commands, exfiltrated files, or C2 traffic) into DNS queries and responses, typically using TXT, CNAME, or A record queries to attacker-controlled domains.

Because DNS is essential for network operations, it's rarely blocked at the firewall, making it an attractive evasion technique.

## 🤔 Why It Matters
- **Evasion:** DNS often bypasses firewalls and security controls
- **[[Command and Control (C2)]]:** Stealthy channel for C2 beacons
- **[[Data Exfiltration]]:** Extract sensitive data through DNS queries
- **MITRE ATT&CK:** T1071.004 (Application Layer Protocol: DNS)

## 🔬 How It Works

### Basic Mechanism
```
┌──────────────┐                    ┌──────────────┐
│    Victim    │                    │  Attacker's  │
│   Machine    │                    │  DNS Server  │
└──────┬───────┘                    └──────┬───────┘
       │                                   │
       │  Query: base64data.evil.com       │
       │──────────────────────────────────►│
       │                                   │
       │  Response: TXT "command_encoded"  │
       │◄──────────────────────────────────│
       │                                   │
```

### Encoding Methods
| Method | Description | Example |
|--------|-------------|---------|
| **Subdomain** | Data encoded in subdomain | `dGVzdA.evil.com` |
| **TXT Records** | Data in TXT response | `"VGhpcyBpcyBzZWNyZXQ="` |
| **CNAME** | Data in CNAME responses | Chained lookups |
| **NULL/PRIVATE** | Less common record types | Harder to detect |

### Data Exfiltration Example
```
# Attacker encodes stolen file as base64, chunks it into subdomains
SGVsbG8gV29ybGQ.evil.com  → Chunk 1
VGhpcyBpcyBzZW.evil.com   → Chunk 2
Y3JldCBkYXRh.evil.com     → Chunk 3
```

## 🔍 Detection Methods

### Indicators of DNS Tunneling
| Indicator | Why It's Suspicious |
|-----------|---------------------|
| **High query volume** | Normal users don't make hundreds of DNS queries/min |
| **Long subdomains** | Legitimate domains rarely exceed 20-30 chars |
| **High entropy strings** | Base64/hex encoded data has high entropy |
| **TXT record queries** | Unusual volume of TXT queries |
| **Uncommon record types** | NULL, PRIVATE, or other rare types |
| **Single domain, many subdomains** | All queries to one attacker domain |

### Detection Tools
| Tool | Capability |
|------|------------|
| [[RITA]] | Analyzes [[Zeek]] logs for DNS anomalies |
| [[Zeek]] | Logs all DNS queries for analysis |
| PassiveDNS | Historical DNS query analysis |
| [[Splunk]] / [[Elastic]] | SIEM queries for DNS patterns |

### Example Detection Query (Splunk)
```spl
index=dns sourcetype=dns
| eval subdomain_length=len(query)-len(replace(query, ".", ""))
| where subdomain_length > 50 OR len(query) > 75
| stats count by query, src_ip
| where count > 100
```

### Sigma Rule
```yaml
title: Potential DNS Tunneling
logsource:
  category: dns
detection:
  selection:
    query|re: '^[a-zA-Z0-9]{30,}\.'
  condition: selection
```

## 🛡️ Prevention & Mitigation

| Control | Implementation |
|---------|---------------|
| **DNS Monitoring** | Log and analyze all DNS queries |
| **DNS Filtering** | Block known malicious domains |
| **Query Length Limits** | Alert on unusually long queries |
| **TXT Record Restrictions** | Limit or monitor TXT queries |
| **Internal DNS Servers** | Force all DNS through monitored resolvers |
| **DNS over HTTPS Blocking** | Prevent encrypted DNS bypass |

## 📊 Common DNS Tunneling Tools

| Tool | Type | Notes |
|------|------|-------|
| **dnscat2** | C2 | Popular open-source tool |
| **iodine** | VPN over DNS | Tunnels IP traffic |
| **DNSExfiltrator** | Exfil | Specifically for data theft |
| **Cobalt Strike** | C2 | DNS beacon capability |

## 🎤 Interview Angles

### Common Questions
- "How would you detect DNS tunneling?"
- "What makes DNS attractive for C2 communication?"
- "What indicators would you look for in DNS logs?"

### STAR Story
> **Situation:** [[RITA]] flagged a host with excessive DNS queries to a single domain with high-entropy subdomains.
> **Task:** Investigate potential DNS tunneling or data exfiltration.
> **Action:** Analyzed [[Zeek]] DNS logs, found thousands of queries with base64-encoded subdomains at regular intervals. Decoded samples revealed file chunks. Identified dnscat2 traffic pattern.
> **Result:** Isolated compromised host, blocked attacker domain, quantified 200MB data exfiltration. Added DNS entropy monitoring to detection stack.

## ❌ Common Misconceptions
- "Blocking DNS will stop tunneling" — Attackers can use DNS over HTTPS (DoH)
- "Small queries aren't a threat" — Even small payloads can exfil credentials
- "Only TXT records are used" — A records, CNAME, and others work too

## 🔗 Related Concepts
- [[Command and Control (C2)]]
- [[Data Exfiltration]]
- [[Domain Name System (DNS)]]
- [[RITA]]
- [[Zeek]]

## 📚 References
- MITRE ATT&CK T1071.004
- [SANS - Detecting DNS Tunneling](https://www.sans.org/white-papers/)
- [dnscat2 GitHub](https://github.com/iagox86/dnscat2)
