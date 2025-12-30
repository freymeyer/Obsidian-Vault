---
dg-publish: true
tags:
  - "#cybersecurity/attacks/c2"
  - "#cybersecurity/malware"
  - "#interview/concepts"
aliases:
  - DGA
  - Domain Flux
---

# Domain Generation Algorithm (DGA)

> **One-liner:** An algorithm that dynamically generates large numbers of domain names for [[Command and Control (C2)|C2]] communication, making it difficult to block or take down attacker infrastructure.

## 🎯 What Is It?
A Domain Generation Algorithm (DGA) is a technique used by malware to evade domain blocking and C2 takedowns. Instead of hardcoding a single C2 domain, the malware generates thousands of pseudo-random domain names using a shared algorithm. The attacker only needs to register a few of these domains to maintain communication.

## 🔬 How It Works

```
┌──────────────────┐                    ┌──────────────────┐
│    Malware       │                    │    Attacker      │
│                  │                    │                  │
│  ┌────────────┐  │                    │  ┌────────────┐  │
│  │    DGA     │  │     Same           │  │    DGA     │  │
│  │ Algorithm  │  │   Algorithm        │  │ Algorithm  │  │
│  └─────┬──────┘  │                    │  └─────┬──────┘  │
│        │         │                    │        │         │
│        ▼         │                    │        ▼         │
│  abc123.com      │                    │  abc123.com ◄──Register 1-2%
│  xyz789.net      │                    │  xyz789.net      │
│  qwe456.org      │    Matches!        │  qwe456.org      │
│  ...             │                    │  ...             │
│  (50,000/day)    │                    │  (50,000/day)    │
└──────────────────┘                    └──────────────────┘
         │                                       │
         └───────────Connects to registered──────┘
                     domains for C2
```

### DGA Types

| Type | Description | Example |
|------|-------------|---------|
| **Time-based** | Uses current date/time as seed | Conficker |
| **Dictionary-based** | Combines real words | Suppobox |
| **Hash-based** | Cryptographic hash of seed | Necurs |
| **Arithmetic** | Mathematical operations | CryptoLocker |

### Simple DGA Example (Python)
```python
import datetime
import hashlib

def generate_domains(date, count=10):
    domains = []
    for i in range(count):
        seed = f"{date.year}{date.month}{date.day}{i}"
        domain = hashlib.md5(seed.encode()).hexdigest()[:12]
        domains.append(f"{domain}.com")
    return domains

# Both malware and attacker generate same domains for today
today = datetime.date.today()
print(generate_domains(today))
```

## 📊 Notable DGA Malware

| Malware | Domains/Day | Algorithm Type |
|---------|-------------|----------------|
| **Conficker** | 50,000 | Time-based |
| **CryptoLocker** | 1,000 | Time-based |
| **Necurs** | 2,048 | Hash-based |
| **Dyre** | 1,000 | Dictionary-based |
| **Emotet** | Variable | Multiple seeds |

## 🛡️ Detection & Prevention

### How to Detect
- **Entropy analysis** - DGA domains have high randomness
- **N-gram analysis** - Detect non-linguistic patterns
- **Machine learning** - Train classifiers on known DGA patterns
- **DNS query volume** - High NXDOMAIN responses (most domains unregistered)
- **Reputation checks** - Query against threat intelligence

### Detection Indicators
```
Legitimate domain: google.com (low entropy, known)
DGA domain: a3f8k2m9p1q4.com (high entropy, unknown)

Red flags:
- Long, random-looking domain names
- High NXDOMAIN rate from single host
- Queries to many TLDs (.com, .net, .org, .info)
- Regular timing patterns in DNS queries
```

### How to Prevent / Mitigate
- DNS sinkholing of known DGA domains
- Block newly registered domains (NRDs)
- DNS response policy zones (RPZ)
- Machine learning-based DNS filtering
- Network monitoring for DGA patterns

## 🎤 Interview Angles

### Common Questions
- "What is a DGA and why do attackers use them?"
- "How would you detect DGA activity in network traffic?"
- "What are the limitations of blocking DGA-based C2?"

### Key Talking Points
- DGAs make C2 takedowns extremely difficult
- Security vendors must preemptively register/monitor domains
- Combine with [[Fast Flux]] for even more resilience
- Machine learning is increasingly effective at detection

## ✅ Best Practices
- Implement DNS logging and analysis
- Use threat intelligence for known DGA patterns
- Monitor for high NXDOMAIN rates
- Block or alert on newly registered domains
- Combine with other C2 detection techniques

## 🔗 Related Concepts
- [[Command and Control (C2)]]
- [[Fast Flux]]
- [[DNS Tunneling]]
- [[Cyber Kill Chain]]
- [[Domain Name System (DNS)]]

## 📚 References
- MITRE ATT&CK - Dynamic Resolution: DGA (T1568.002)
- DGArchive - Academic DGA research
