---
dg-publish: true
tags:
  - "#cybersecurity/blue-team/detection"
  - "#cybersecurity/defense"
  - "#interview/concepts"
aliases:
  - Honeypots
  - Honey Pot
---

# Honeypot

> **One-liner:** A decoy system designed to attract attackers, detect intrusion attempts, and gather intelligence on attacker tactics and tools.

## 🎯 What Is It?
A Honeypot is a deliberately vulnerable or enticing system deployed to detect unauthorized access attempts. Honeypots serve as early warning systems and intelligence gathering tools. Any interaction with a honeypot is suspicious by definition, since legitimate users have no reason to access it.

## 🔬 How It Works

```
Normal Network:                    With Honeypots:
┌─────────────────────┐           ┌─────────────────────┐
│   Real Servers      │           │   Real Servers      │
│                     │           │                     │
│  ┌───┐ ┌───┐ ┌───┐ │           │  ┌───┐ ┌───┐ ┌───┐ │
│  │Web│ │DB │ │App│ │           │  │Web│ │DB │ │App│ │
│  └───┘ └───┘ └───┘ │           │  └───┘ └───┘ └───┘ │
│                     │           │                     │
│  Attacker scans     │           │  ┌───┐ ← Honeypot  │
│  real systems       │           │  │🍯│    (Decoy)   │
│                     │           │  └───┘              │
│                     │           │    │                │
│                     │           │  Alert! Attacker   │
│                     │           │  touched honeypot  │
└─────────────────────┘           └─────────────────────┘
```

## 📊 Types of Honeypots

### By Interaction Level

| Type | Description | Risk | Intelligence |
|------|-------------|------|--------------|
| **Low Interaction** | Emulates services, limited functionality | Low | Basic IOCs |
| **Medium Interaction** | Partial service emulation | Medium | TTPs, tools |
| **High Interaction** | Full systems, real OS/services | High | Complete intel |

### By Purpose

| Type | Purpose | Examples |
|------|---------|----------|
| **Production** | Detect attacks on real network | Network honeypots |
| **Research** | Study attacker behavior | University honeynets |
| **Deception** | Waste attacker time/resources | Fake credentials, files |

### Common Honeypot Tools

| Tool | Type | Emulates |
|------|------|----------|
| **Cowrie** | Medium | SSH/Telnet |
| **Dionaea** | Low | Multiple protocols |
| **HoneyD** | Low | Network hosts |
| **Kippo** | Medium | SSH |
| **T-Pot** | Platform | Multiple honeypots |
| **Conpot** | Low | ICS/SCADA |

## 🍯 Honeytokens
Honeytokens are data-based traps (non-system honeypots):

| Honeytoken | Purpose |
|------------|---------|
| Fake credentials | Detect credential theft/use |
| Canary files | Detect data exfiltration |
| Database records | Detect unauthorized access |
| AWS keys | Detect cloud credential theft |
| DNS canaries | Detect internal recon |

## 🛡️ Deployment Considerations

### Benefits
- Zero false positives (any access is suspicious)
- Early warning system for attacks
- Intelligence on attacker TTPs
- Detect [[Lateral Movement]] and internal threats
- Waste attacker time and resources

### Risks
- Can be used as pivot point if compromised
- Requires monitoring and maintenance
- May attract unwanted attention
- Legal considerations for data collection

## 🎤 Interview Angles

### Common Questions
- "What is a honeypot and why would you deploy one?"
- "What's the difference between low and high interaction honeypots?"
- "How would you detect if someone interacted with a honeypot?"

### STAR Story
> **Situation:** Needed to detect lateral movement within the internal network.
> **Task:** Deploy detection mechanisms that had zero false positives.
> **Action:** Deployed internal honeypots emulating file servers and domain controllers. Created honeytoken credentials in LSASS.
> **Result:** Detected an attacker using stolen credentials to access the honeypot, enabling early containment before production systems were compromised.

## ✅ Best Practices
- Make honeypots look realistic and attractive
- Isolate from production to prevent pivot attacks
- Log everything—every packet, every command
- Alert immediately on any interaction
- Regularly update to remain convincing
- Legal review for compliance

## 🔗 Related Concepts
- [[Cyber Kill Chain]]
- [[Command and Control (C2)]]
- [[Lateral Movement]]
- [[Detection Engineering]]
- [[Deception Technology]]

## 📚 References
- MITRE Shield - Honeypots
- The Honeynet Project
