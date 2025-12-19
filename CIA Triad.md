---
tags:
  - "#cybersecurity/frameworks"
  - "#interview/concepts"
aliases:
  - CIA
  - Confidentiality Integrity Availability
---

# CIA Triad

> **One-liner:** The three core principles of information security: Confidentiality, Integrity, and Availability.

## 🎯 What Is It?
The CIA Triad is the foundational model for information security. Every security control, vulnerability, and attack can be understood through its impact on one or more of these three pillars.

## 📊 The Three Pillars

```
                    ┌─────────────────┐
                    │ CONFIDENTIALITY │
                    │   "Need to know"│
                    └────────┬────────┘
                             │
                             ▼
              ┌──────────────────────────────┐
              │                              │
              │         CIA TRIAD            │
              │                              │
              └──────────────────────────────┘
                    ▲                ▲
                    │                │
         ┌─────────┴───┐      ┌─────┴─────────┐
         │  INTEGRITY  │      │ AVAILABILITY  │
         │ "Trustworthy"│      │ "Accessible"  │
         └─────────────┘      └───────────────┘
```

## 🔒 Confidentiality
**Definition:** Ensuring information is only accessible to authorized parties.

| Threat | Control |
|--------|---------|
| Data breach | Encryption at rest |
| Eavesdropping | Encryption in transit (TLS) |
| Unauthorized access | Access controls, [[Authentication]] |
| Social engineering | Security awareness training |
| Shoulder surfing | Privacy screens |

**Example Attack:** [[SQL Injection]] exposing customer database

---

## ✅ Integrity
**Definition:** Ensuring information is accurate, complete, and unmodified by unauthorized parties.

| Threat | Control |
|--------|---------|
| Data tampering | Hashing, digital signatures |
| Man-in-the-middle | TLS, certificate pinning |
| Unauthorized changes | Change management, audit logs |
| Malware | File integrity monitoring |
| Injection attacks | Input validation |

**Example Attack:** Attacker modifying wire transfer amounts

---

## ⚡ Availability
**Definition:** Ensuring systems and data are accessible when needed.

| Threat | Control |
|--------|---------|
| DDoS | CDN, rate limiting, scrubbing |
| Hardware failure | Redundancy, clustering |
| Ransomware | Backups, incident response |
| Natural disaster | Disaster recovery, geo-redundancy |
| Power outage | UPS, generators |

**Example Attack:** [[Ransomware]] encrypting critical files

---

## 🎯 Applying CIA to Scenarios

| Scenario | Primary Impact |
|----------|---------------|
| Customer PII leaked | **Confidentiality** |
| Database records altered | **Integrity** |
| Website taken offline by DDoS | **Availability** |
| Ransomware attack | **Availability** (+ Confidentiality if exfil) |
| MITM modifying transactions | **Integrity** |
| Phishing stealing credentials | **Confidentiality** |

## 🔄 Extended Models

| Model | Additions |
|-------|-----------|
| **Parkerian Hexad** | + Possession, Authenticity, Utility |
| **DAD Triad** | Disclosure, Alteration, Destruction (opposite) |

## 🎤 Interview STAR Example
> **Situation:** Company needed to classify security risks for a new payment system.
> **Task:** Create a risk assessment framework.
> **Action:** Used CIA Triad to categorize each risk. Identified that payment integrity was highest priority (fraudulent transactions), followed by confidentiality (PCI compliance), then availability.
> **Result:** Prioritized controls: transaction signing for integrity, encryption for confidentiality, redundancy for availability. Passed PCI audit on first attempt.

## 💡 Interview Tips
- Always mention CIA when discussing security risks
- For any vulnerability, identify which pillar is affected
- Know examples of attacks and controls for each
- Understand tradeoffs (e.g., high availability may reduce confidentiality)

## 🔗 Related Concepts
- [[Authentication]] — Supports confidentiality
- [[Authorization]] — Supports all three
- [[Cryptographic Failure]] — Impacts confidentiality/integrity
- [[Ransomware]] — Primarily impacts availability

## 📚 References
- NIST SP 800-12: Introduction to Computer Security
- ISO 27001 Information Security
