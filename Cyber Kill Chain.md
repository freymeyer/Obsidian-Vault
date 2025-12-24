---
tags:
  - "#cybersecurity/frameworks"
  - "#cybersecurity/blue-team/detection"
  - "#cybersecurity/red-team/methodology"
  - "#interview/concepts"
aliases:
  - CKC
  - Lockheed Martin Kill Chain
---

# Cyber Kill Chain

> **One-liner:** A 7-stage framework by Lockheed Martin that maps the phases of a cyberattack from reconnaissance to objective execution.

## 🎯 What Is It?
The Cyber Kill Chain is a cybersecurity framework introduced by **Lockheed Martin in 2011**, inspired by military kill chain concepts. It helps organizations understand how cyberattacks are conducted by breaking them into sequential stages. By understanding each stage, defenders can interrupt attacks before they reach their objectives.

## 🔬 The Seven Stages

| Stage | Name | Attacker Goal | Defender Focus |
|-------|------|---------------|----------------|
| 1 | **[[Reconnaissance (Cyber Security)\|Reconnaissance]]** | Gather information about the target | Minimize exposure, monitor for scans |
| 2 | **Weaponization** | Create/modify payload for target vulnerabilities | Disable risky features (macros), user training |
| 3 | **Delivery** | Transmit payload to target environment | Email/web filtering, [[Web Application Firewalls (WAFS)\|WAFs]] |
| 4 | **Exploitation** | Execute payload via vulnerability or weak auth | Patching, [[Multi-Factor Authentication (MFA)\|MFA]], [[Intrusion Prevention System (IPS)\|IPS]] |
| 5 | **Installation** | Establish [[Persistence (Cyber Security)\|persistence]] (backdoors, malware) | [[Endpoint detection and response (EDR)\|EDR]], application allowlisting |
| 6 | **[[Command and Control (C2)]]** | Create covert communication channel | Network monitoring, DNS analysis |
| 7 | **Actions on Objectives** | Execute goals (exfiltration, ransomware) | [[Data Loss Prevention (DLP)\|DLP]], network segmentation |

## 📊 Stage Deep Dive

### Stage 1: Reconnaissance
Information gathering about the target's vulnerabilities and weaknesses.
- **Passive:** [[Open Source Intelligence (OSINT)\|OSINT]], [[Google Dorking]], WHOIS, DNS queries
- **Active:** Port scanning, vulnerability scanning, [[Social Engineering]]

### Stage 2: Weaponization
Creating a deliverable malicious payload tailored to discovered weaknesses.
- [[Exploit Kit|Exploit Kits]] for packaging exploits
- Malicious macros in Office documents
- Obfuscation and encryption to evade detection

### Stage 3: Delivery
Transmitting the weaponized payload to the target.
- [[Phishing]] / Spear phishing emails
- [[Malvertising]]
- [[Smishing]] (SMS phishing)
- Physical delivery (USB drops)

### Stage 4: Exploitation
The payload executes and exploits a vulnerability.
- Password attacks ([[Brute-force]], credential theft)
- Software vulnerabilities
- [[Zero-Day Exploit|Zero-day exploits]]

### Stage 5: Installation
Establishing persistent access to the compromised system.
- [[Web Shell|Web shells]]
- Scheduled tasks / cron jobs
- [[Living off the Land (LOLBAS)|LOLBins]]

### Stage 6: Command and Control
Setting up covert communication for remote control.
- [[DNS Tunneling]]
- [[Domain Generation Algorithm (DGA)|DGA]]
- [[Fast Flux]]
- HTTPS encryption

### Stage 7: Actions on Objectives
Executing the attacker's ultimate goal.
- [[Data Exfiltration]]
- [[Ransomware]]
- [[Lateral Movement]]
- System destruction

## 🛡️ Breaking the Chain

The key defensive principle: **interrupt at any stage to prevent objective completion**.

```
Early Detection = Less Damage
─────────────────────────────
Recon → Weaponize → Deliver → Exploit → Install → C2 → Actions
  ↑         ↑          ↑         ↑         ↑       ↑       ↑
Block    Disable     Filter    Patch     EDR   Monitor   DLP
 info    macros      email     vulns           traffic
```

## 🎤 Interview Angles

### Common Questions
- "Walk me through the Cyber Kill Chain stages"
- "At which stage is it most effective to stop an attacker?"
- "How does the Kill Chain relate to [[MITRE ATT&CK]]?"

### Key Talking Points
- Earlier detection = less damage and lower remediation cost
- The framework is **linear**, but real attacks may skip stages
- [[MITRE ATT&CK]] provides more granular TTPs within each stage

## ❌ Limitations
- Assumes a linear attack flow (not always true)
- Focuses on external threats; less applicable to insider threats
- Doesn't cover cloud-native or supply chain attacks well
- [[MITRE ATT&CK]] is often preferred for detailed TTP mapping

## 🔗 Related Concepts
- [[MITRE ATT&CK]]
- [[Pyramid of Pain]]
- [[Reconnaissance (Cyber Security)]]
- [[Command and Control (C2)]]
- [[Persistence (Cyber Security)]]
- [[Lateral Movement]]

## 📚 References
- Lockheed Martin - Original Kill Chain Paper (2011)
- [TryHackMe - Cyber Kill Chain Room](https://tryhackme.com/room/cyberkillchain)
