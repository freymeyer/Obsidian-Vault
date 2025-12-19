---
tags:
  - "#cybersecurity/frameworks"
  - "#interview/concepts"
aliases:
  - ATT&CK
  - MITRE
  - ATT&CK Framework
---

# MITRE ATT&CK

> **One-liner:** A globally-recognized knowledge base of adversary tactics and techniques based on real-world observations.

## 🎯 What Is It?
MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) is a framework that categorizes cyber adversary behavior. It's the de facto standard for describing HOW attackers operate, used by red teams, blue teams, and vendors worldwide.

## 📊 Framework Structure

```
Enterprise ATT&CK Matrix

TACTICS (The "Why" - Adversary Goals)
─────────────────────────────────────────────────────────────────►
│ Recon │ Resource │ Initial │ Execution │ Persistence │ Priv  │
│       │   Dev    │ Access  │           │             │ Esc   │
├───────┼──────────┼─────────┼───────────┼─────────────┼───────┤
│       │          │         │           │             │       │
│  T1595│   T1583  │  T1566  │   T1059   │    T1547    │ T1548 │
│  T1592│   T1584  │  T1190  │   T1204   │    T1053    │ T1134 │
│  ...  │   ...    │  ...    │   ...     │    ...      │ ...   │
│       │          │         │           │             │       │
└───────┴──────────┴─────────┴───────────┴─────────────┴───────┘
         TECHNIQUES (The "How" - Specific Methods)
```

## 🎯 14 Tactics (Enterprise)

| ID | Tactic | Goal |
|----|--------|------|
| TA0043 | Reconnaissance | Gather victim info |
| TA0042 | Resource Development | Acquire infrastructure |
| TA0001 | Initial Access | Get into the network |
| TA0002 | Execution | Run malicious code |
| TA0003 | Persistence | Maintain foothold |
| TA0004 | Privilege Escalation | Gain higher permissions |
| TA0005 | Defense Evasion | Avoid detection |
| TA0006 | Credential Access | Steal credentials |
| TA0007 | Discovery | Learn the environment |
| TA0008 | Lateral Movement | Move through network |
| TA0009 | Collection | Gather target data |
| TA0011 | Command and Control | Communicate with implants |
| TA0010 | Exfiltration | Steal data |
| TA0040 | Impact | Disrupt operations |

## 🔬 Technique Example

**T1566 - Phishing**
- **Sub-technique T1566.001:** Spearphishing Attachment
- **Sub-technique T1566.002:** Spearphishing Link
- **Sub-technique T1566.003:** Spearphishing via Service

Each technique includes:
- Description
- Procedure examples (real APT usage)
- Mitigations
- Detection guidance

## 💡 How to Use ATT&CK

### For Blue Team
- Map detections to techniques
- Identify coverage gaps
- Prioritize detection engineering
- Threat hunt based on TTPs

### For Red Team
- Structure attack plans
- Emulate specific threat actors
- Report findings consistently

### For Threat Intel
- Describe adversary behavior
- Compare threat actor TTPs
- Track evolving tradecraft

## 🛠️ Related Resources

| Resource | Purpose |
|----------|---------|
| ATT&CK Navigator | Visualize coverage/gaps |
| MITRE D3FEND | Defensive techniques (countermeasures) |
| ATT&CK Workbench | Customize for your org |
| Atomic Red Team | Test technique detection |

## 🎤 Interview STAR Example
> **Situation:** Security team had no standard way to measure detection coverage.
> **Task:** Implement a framework to assess and improve detection capabilities.
> **Action:** Mapped existing SIEM rules to ATT&CK techniques using Navigator. Identified 40% coverage gap in Credential Access and Lateral Movement tactics. Prioritized 10 new detections based on threat intel showing those TTPs used against our sector.
> **Result:** Increased ATT&CK coverage from 35% to 60% in 3 months. Detected simulated Kerberoasting attack during purple team exercise.

## 💡 Interview Tips
- Know at least 3-4 tactics and example techniques
- Understand difference between Tactic vs Technique vs Sub-technique
- Be able to discuss how you'd use Navigator
- Mention ATT&CK when discussing detection engineering

## 🔗 Related Concepts
- [[Kill Chain]] — Simpler linear model (ATT&CK is more detailed)
- [[Detection Engineering]]
- [[threat intelligence|Threat Intelligence]]
- [[Red Teaming]]
- [[Pyramid of Pain]]

## 📚 References
- MITRE ATT&CK: https://attack.mitre.org/
- ATT&CK Navigator: https://mitre-attack.github.io/attack-navigator/
