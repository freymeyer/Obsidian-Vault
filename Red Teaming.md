---
dg-publish: true
tags:
  - "#cybersecurity/red-team/methodology"
  - "#interview/concepts"
aliases:
  - Red Team
  - Adversary Simulation
---

# Red Teaming

> **One-liner:** Simulating real-world adversaries to test an organization's detection and response capabilities.

## 🎯 What Is It?
Red teaming is an adversarial security assessment where a team emulates threat actors' tactics, techniques, and procedures (TTPs) to identify security gaps that traditional testing might miss.

## 🔴 Red Team vs Penetration Testing

| Aspect | Red Team | Penetration Test |
|--------|----------|------------------|
| **Goal** | Test detection & response | Find vulnerabilities |
| **Scope** | Full organization | Defined targets |
| **Duration** | Weeks to months | Days to weeks |
| **Stealth** | Evade detection | Not a priority |
| **Knowledge** | Minimal (black box) | Often documented |
| **Blue Team Aware?** | Usually not | Usually yes |

## 🚩 Red Team Phases

1. **Reconnaissance** — OSINT, social engineering research
2. **Initial Access** — [[Phishing]], exploits, physical access
3. **Execution** — Running malicious code
4. **Persistence** — Maintaining access
5. **Privilege Escalation** — Gaining higher access
6. **Defense Evasion** — Avoiding detection
7. **Lateral Movement** — Moving through network
8. **Collection** — Gathering target data
9. **Exfiltration** — [[Data Exfiltration|Extracting data]]
10. **Impact** — Achieving objectives

## 🛠️ Common Red Team Tools

| Tool | Purpose |
|------|---------|
| Cobalt Strike | C2 framework |
| [[Metasploit]] | Exploitation |
| BloodHound | AD enumeration |
| Mimikatz | Credential dumping |
| [[Phishing]] frameworks | Initial access |

## 🤝 Purple Teaming
When red and [[Blue Teaming|blue teams]] collaborate:
- Red executes TTPs openly
- Blue observes and tunes detections
- Both improve together

## 🎤 Interview STAR Example
> **Situation:** Organization wanted to test SOC detection capabilities before a compliance audit.
> **Task:** Conduct a red team engagement simulating an APT actor.
> **Action:** Performed spear phishing to gain initial access, escalated privileges using Kerberoasting, moved laterally via RDP, and exfiltrated simulated sensitive data.
> **Result:** Identified 12 detection gaps. SOC implemented new alerts. Follow-up engagement showed 80% improvement in detection time.

## 🎯 Frameworks
- [[MITRE ATT&CK]] — Adversary tactics & techniques
- [[Kill Chain]] — Lockheed Martin attack phases
- PTES — Penetration Testing Execution Standard

## 🔗 Related Concepts
- [[Blue Teaming]]
- [[Phishing]]
- [[Command and Control (C2)]]
- [[Privilege Escalation]]
- [[MITRE ATT&CK]]

## 📚 References
- MITRE ATT&CK Framework
- Red Team Development and Operations Guide