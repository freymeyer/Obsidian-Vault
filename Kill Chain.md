---
dg-publish: true
tags:
  - "#cybersecurity/frameworks"
  - "#interview/concepts"
aliases:
  - Cyber Kill Chain
  - Lockheed Martin Kill Chain
  - LM Kill Chain
---

# Kill Chain

> **One-liner:** A 7-phase model describing the stages of a cyberattack from reconnaissance to objective completion.

## 🎯 What Is It?
The Cyber Kill Chain, developed by Lockheed Martin, breaks down cyberattacks into sequential phases. Understanding each phase helps defenders identify where to detect and disrupt attacks. Breaking any link in the chain stops the attack.

## 📊 The 7 Phases

```
┌─────────────────────────────────────────────────────────────────┐
│                     CYBER KILL CHAIN                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. RECONNAISSANCE ──► 2. WEAPONIZATION ──► 3. DELIVERY        │
│         │                     │                  │              │
│    Gather info            Build payload      Send to target     │
│                                                  │              │
│  ◄──────────────────────────────────────────────┘              │
│                                                                 │
│  4. EXPLOITATION ──► 5. INSTALLATION ──► 6. COMMAND & CONTROL  │
│         │                   │                    │              │
│    Trigger vuln        Install malware      Establish C2        │
│                                                  │              │
│                              7. ACTIONS ON OBJECTIVES ◄─────────┘
│                                      │                          │
│                               Achieve goal                      │
│                         (exfil, destroy, encrypt)               │
└─────────────────────────────────────────────────────────────────┘
```

## 🔬 Phase Details

| Phase | Attacker Action | Defender Detection | Defender Prevention |
|-------|-----------------|-------------------|---------------------|
| **1. Reconnaissance** | OSINT, scanning, social media research | Monitor for port scans, unusual queries | Limit public exposure, employee training |
| **2. Weaponization** | Create malicious payload (PDF, Office doc) | Threat intel on new malware | N/A (happens off-network) |
| **3. Delivery** | [[Phishing]], watering hole, USB drop | Email gateway, web proxy, [[EDR]] | Email filtering, web filtering, training |
| **4. Exploitation** | Trigger vulnerability (CVE, 0-day) | [[EDR]], exploit detection | Patching, hardening, least privilege |
| **5. Installation** | Malware persists on system | [[EDR]], file integrity, [[Sysmon]] | Application whitelisting, [[EDR]] |
| **6. C2** | Beacon to [[Command and Control (C2)|C2 server]] | Network monitoring, DNS analysis | Egress filtering, SSL inspection |
| **7. Actions** | Exfil data, ransomware, destruction | DLP, behavioral analytics | Data classification, backups |

## 💡 Key Insight: Left of Boom vs Right of Boom

```
        ◄── LEFT OF BOOM ──►  💥  ◄── RIGHT OF BOOM ──►
        
Recon → Weaponization → Delivery → Exploitation → Install → C2 → Actions
                                        │
                                      BOOM
                                   (Compromise)
```

- **Left of Boom:** Prevention-focused (stop before breach)
- **Right of Boom:** Detection & response-focused (limit damage)

## 🆚 Kill Chain vs ATT&CK

| Aspect | Kill Chain | [[MITRE ATT&CK]] |
|--------|------------|------------------|
| Phases | 7 linear stages | 14 non-linear tactics |
| Detail | High-level | Granular techniques |
| Focus | Attack progression | Adversary behavior |
| Best For | Explaining attacks to executives | Detection engineering |

## 🎤 Interview STAR Example
> **Situation:** Asked to explain a recent breach to the executive team.
> **Task:** Communicate technical attack in business terms.
> **Action:** Used Kill Chain to walk through the attack: Recon via LinkedIn (found IT admin), Delivery via spearphishing, Exploitation of unpatched Outlook, Installation of Cobalt Strike, C2 over HTTPS, Actions = data exfiltration.
> **Result:** Executives understood the attack flow and approved budget for email security and patch management improvements.

## 💡 Interview Tips
- Know all 7 phases in order
- Be able to give an example for each phase
- Understand it's about "breaking the chain"
- Compare/contrast with ATT&CK when asked

## 🔗 Related Concepts
- [[MITRE ATT&CK]] — More detailed framework
- [[Unified Kill Chain]] — Extended version
- [[Phishing]] — Common delivery mechanism
- [[Command and Control (C2)]]
- [[Detection Engineering]]

## 📚 References
- Lockheed Martin Cyber Kill Chain: https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html
- SANS: Applying the Kill Chain
