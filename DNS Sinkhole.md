---
tags:
  - "#cybersecurity/blue-team/prevention"
  - "#cybersecurity/networking/dns"
  - "#interview/concepts"
aliases:
  - Sinkholing
  - DNS Blackholing
---

# DNS Sinkhole

> **One-liner:** A defensive technique that redirects DNS queries for malicious domains to a controlled IP address, preventing connections to attacker infrastructure.

## 🎯 What Is It?
DNS Sinkhole is a security mechanism that intercepts and redirects DNS requests for known malicious domains to a safe IP address (typically `0.0.0.0`, `127.0.0.1`, or a dedicated sinkhole server). When a user or infected system tries to resolve a malicious domain, the DNS server returns the sinkhole IP instead of the actual malicious server's address, effectively blocking the connection.

## 🤔 Why It Matters
- **Prevents [[Command and Control (C2)]]** — Stops malware from communicating with attacker servers
- **Blocks malware downloads** — Prevents initial infection or additional payload retrieval
- **Protects against [[Phishing]]** — Blocks access to phishing websites
- **Enables detection** — Sinkhole queries provide IOCs for investigation
- **Zero-day protection** — Works even against unknown malware using known bad domains

## 🔬 How It Works

### Basic Flow
```
1. User/Malware queries: malicious-c2.evil.com
2. Internal DNS server checks blocklist
3. Domain matches blocklist entry
4. DNS returns: 0.0.0.0 (sinkhole IP)
5. Connection attempt fails or goes to sinkhole server
6. Event logged for investigation
```

### Common Sinkhole IPs
| IP Address | Purpose |
|------------|---------|
| `0.0.0.0` | Null route (connection fails immediately) |
| `127.0.0.1` | Localhost (connection loops back) |
| `192.168.x.x` | Internal sinkhole server (logs connections) |
| Dedicated IP | Sinkhole server for analysis |

### Implementation Methods

#### 1. DNS Server Configuration
- **Bind/Unbound**: RPZ (Response Policy Zones)
- **Windows DNS**: Conditional forwarding or zone redirect
- **Pi-hole**: Domain blocklist

#### 2. DNS Firewall / RPZ
```
; Response Policy Zone example
evil-c2.com CNAME .  ; Null response
malware.net A 0.0.0.0  ; Sinkhole IP
```

#### 3. Enterprise DNS Security
- Cisco Umbrella
- Infoblox DNS Firewall
- Akamai ETP

## 🛡️ Detection & Prevention

### How to Detect Sinkholed Connections
Using [[Security Information and Event Management system (SIEM)|SIEM]] or log analysis:

```kql
# Kibana/Elastic Query
dns.resolved_ip: "0.0.0.0" OR dns.resolved_ip: "127.0.0.1"

# Splunk Query
index=dns dns_answer="0.0.0.0" OR dns_answer="127.0.0.1"
| stats count by src_ip, query
| sort - count
```

### Indicators to Monitor
- High volume of queries to sinkhole IP from single host → Infected system
- Repeated queries to same sinkholed domain → Persistent malware
- Queries to newly sinkholed domains → Recent compromise

### [[Sigma]] Rule Example
```yaml
title: DNS Query to Sinkholed Domain
logsource:
  category: dns
detection:
  selection:
    dns.resolved_ip:
      - '0.0.0.0'
      - '127.0.0.1'
  condition: selection
falsepositives:
  - Legitimate services using localhost
level: medium
```

## ⚔️ Offensive Evasion Techniques
Attackers try to bypass DNS sinkholing:
- **Hard-coded IPs** — Bypass DNS entirely
- **DNS over HTTPS (DoH)** — Encrypted DNS to public resolvers
- **[[Domain Generation Algorithm (DGA)]]** — Generate thousands of domains (only few need to work)
- **[[Fast Flux]]** — Rapidly change DNS responses
- **Custom DNS resolvers** — Use attacker-controlled DNS
- **IP-based C2** — Direct IP connections

## 🎤 Interview Angles

### Common Questions
- "What is a DNS sinkhole and how does it work?"
- "How would you detect a sinkholed connection in logs?"
- "What are the limitations of DNS sinkholing?"
- "How do attackers bypass DNS sinkholing?"

### Key Talking Points
- Sinkholing is **reactive** — requires known bad domains
- Works best with **[[Cyber Threat Intelligence (CTI)|threat intelligence]]** feeds
- Combine with **DNS logging** for investigation
- **DoH/DoT** bypass traditional sinkholing (need TLS inspection)
- Effective first layer of defense, not complete solution

### STAR Story
> **Situation:** [[Security Information and Event Management system (SIEM)|SIEM]] showed 115 hits to domains resolving to `0.0.0.0` from 12 unique hosts.
> **Task:** Investigate potential malware infections indicated by DNS sinkhole.
> **Action:** Queried DNS logs for all queries resolving to sinkhole IP. Identified 12 malicious domains and 7 infected hosts. Cross-referenced with [[Cyber Threat Intelligence (CTI)|threat intelligence]] feeds—domains linked to known [[Malware]] campaign. Isolated affected hosts, ran [[Endpoint detection and response (EDR)|EDR]] scans, found trojan banking malware. Deployed updated IOCs to firewall.
> **Result:** Contained infection to 7 hosts before data exfiltration. Blocked malware C2 for entire network via DNS sinkhole. Updated incident response playbook with sinkhole monitoring procedures.

## ✅ Best Practices
- Maintain up-to-date malicious domain feeds
- Use dedicated sinkhole server (not `0.0.0.0`) for better logging
- Alert on sinkhole hits for immediate investigation
- Combine with [[Firewall]] IP blocking for defense-in-depth
- Monitor for DoH/DoT traffic bypassing internal DNS
- Log all DNS queries for forensics
- Regularly review and update blocklists

## ❌ Common Misconceptions
- "Sinkholing stops all malware" — Only works if malware uses DNS
- "Sinkhole = blocking" — Sinkholing redirects; blocking would be DNS filtering
- "`0.0.0.0` is the only sinkhole IP" — Can use any IP; `0.0.0.0` is just common
- "Set and forget" — Requires continuous feed updates and monitoring

## 🔗 Related Concepts
- [[Domain Name System (DNS)]]
- [[DNS Tunneling]]
- [[Domain Generation Algorithm (DGA)]]
- [[Fast Flux]]
- [[Command and Control (C2)]]
- [[Cyber Threat Intelligence (CTI)]]
- [[Indicator of Compromise (IOC)]]
- [[Security Information and Event Management system (SIEM)]]

## 📚 References
- SANS: DNS Sinkhole as a Defensive Weapon
- MITRE ATT&CK: T1071.004 (Application Layer Protocol: DNS)
- Spamhaus Domain Block List (DBL)
- abuse.ch URLhaus feed
