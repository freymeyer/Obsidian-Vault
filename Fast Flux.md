---
tags:
  - "#cybersecurity/attacks/c2"
  - "#cybersecurity/networking"
  - "#interview/concepts"
aliases:
  - Fast-Flux
  - Fast Flux Networks
---

# Fast Flux

> **One-liner:** A DNS technique that rapidly rotates IP addresses associated with a domain to hide malicious infrastructure and resist takedown.

## 🎯 What Is It?
Fast Flux is an evasion technique used in the **Command and Control** stage of the [[Cyber Kill Chain]] where a domain's DNS records are rapidly changed to point to different IP addresses (often compromised machines). This makes it extremely difficult to block C2 infrastructure or identify the true location of malicious servers.

## 🔬 How It Works

### Normal DNS vs Fast Flux

```
Normal DNS:
┌─────────────────────────────────────────┐
│  evil.com → 192.168.1.100              │
│  (Static, easy to block)                │
└─────────────────────────────────────────┘

Fast Flux DNS:
┌─────────────────────────────────────────┐
│  evil.com → Changes every 3-5 minutes   │
│                                         │
│  Time 0:   → 1.2.3.4   (Bot in USA)    │
│  Time 5:   → 5.6.7.8   (Bot in Germany)│
│  Time 10:  → 9.10.11.12 (Bot in Brazil)│
│  ...                                    │
│  (1000s of rotating IPs)                │
└─────────────────────────────────────────┘
```

### Architecture Types

| Type | Description | Complexity |
|------|-------------|------------|
| **Single Flux** | IP addresses rotate; nameserver static | Low |
| **Double Flux** | Both IPs and nameservers rotate | High |

### Single Flux vs Double Flux

```
Single Flux:
─────────────
DNS Server (static) ──► Domain ──► Rotating IPs
     ns1.evil.com        evil.com   1.2.3.4, 5.6.7.8...

Double Flux:
─────────────
Rotating NS ──► Domain ──► Rotating IPs
ns1.evil.com    evil.com   1.2.3.4, 5.6.7.8...
ns2.evil.com
ns3.evil.com
(all rotate)
```

### How Fast Flux Works

```
1. Attacker controls domain with short TTL (3-5 min)
2. Compromised bots act as proxy nodes
3. DNS returns multiple A records (round-robin)
4. Records change frequently

Victim → DNS Query → Bot1 → Backend C2
                  ↓
         (5 min later)
                  ↓
Victim → DNS Query → Bot47 → Backend C2
```

## 📊 Characteristics

| Indicator | Normal Domain | Fast Flux Domain |
|-----------|---------------|------------------|
| **TTL** | Hours to days | 3-5 minutes |
| **IP Count** | 1-5 IPs | 10-1000+ IPs |
| **IP Diversity** | Same ASN/region | Multiple countries |
| **IP Churn** | Rare changes | Constant rotation |

## 🛡️ Detection & Prevention

### How to Detect
- **Low TTL values** - TTL < 300 seconds is suspicious
- **High A record count** - Many IPs for one domain
- **IP diversity** - IPs across many ASNs/countries
- **Passive DNS analysis** - Track historical IP associations
- **Anomaly detection** - Compare against baseline DNS behavior

### Detection Query Example
```sql
-- Detect fast flux domains
SELECT domain, COUNT(DISTINCT ip) as ip_count, AVG(ttl) as avg_ttl
FROM dns_logs
WHERE timestamp > NOW() - INTERVAL 1 HOUR
GROUP BY domain
HAVING ip_count > 10 AND avg_ttl < 300
```

### How to Prevent / Mitigate
- Block domains with fast flux characteristics
- Use DNS sinkholing for known malicious domains
- Implement DNS Response Policy Zones (RPZ)
- Monitor for newly registered domains (NRDs)
- Combine with [[Domain Generation Algorithm (DGA)|DGA]] detection

## 🎤 Interview Angles

### Common Questions
- "What is Fast Flux and why do attackers use it?"
- "How would you detect Fast Flux activity?"
- "What's the difference between single and double flux?"

### Key Talking Points
- Makes IP-based blocking ineffective
- Often combined with [[Domain Generation Algorithm (DGA)|DGA]] for maximum resilience
- Proxy layer hides true C2 server location
- Detection requires DNS traffic analysis

## ✅ Best Practices
- Log and analyze DNS queries
- Alert on domains with very low TTLs
- Correlate with threat intelligence
- Use machine learning for flux detection
- Block access to known fast flux domains

## 🔗 Related Concepts
- [[Command and Control (C2)]]
- [[Domain Generation Algorithm (DGA)]]
- [[DNS Tunneling]]
- [[Cyber Kill Chain]]
- [[Domain Name System (DNS)]]

## 📚 References
- MITRE ATT&CK - Dynamic Resolution (T1568)
- Shadowserver Fast Flux Reports
