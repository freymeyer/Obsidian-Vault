---
tags:
  - "#cybersecurity/red-team/c2"
  - "#interview/concepts"
aliases:
  - C2
  - C&C
  - Command and Control
---

# Command and Control (C2)

> **One-liner:** Infrastructure used by attackers to maintain communication with compromised systems and issue commands remotely.

## 🎯 What Is It?
Command and Control (C2) infrastructure consists of servers, protocols, and tools that allow attackers to remotely control compromised machines. Unlike simple reverse shells, C2 frameworks provide advanced features like persistence, lateral movement, and data exfiltration.

## 💥 Why It Matters
- **For Red Teams:** Essential for maintaining access during engagements
- **For Blue Teams:** C2 detection is a primary SOC function
- **MITRE ATT&CK:** Tactic TA0011 — one of the most important to detect

## 📊 C2 Communication Channels

| Protocol | Stealth Level | Detection Difficulty |
|----------|---------------|---------------------|
| HTTP/HTTPS | High | Hard (blends with web traffic) |
| DNS | Very High | Medium (DNS tunneling detection) |
| ICMP | Medium | Easy (unusual for regular traffic) |
| SMB | Medium | Medium (internal networks) |
| Custom TCP/UDP | Low | Easy (unusual ports) |

## 🔬 How C2 Works

```
┌─────────────┐         ┌─────────────┐         ┌─────────────┐
│   Attacker  │ ──────► │  C2 Server  │ ◄────── │   Victim    │
│  (Operator) │         │  (Teamserver)│         │  (Beacon)   │
└─────────────┘         └─────────────┘         └─────────────┘
       │                       │                       │
       │   Issue Commands      │    Beacon Check-in    │
       │──────────────────────►│◄──────────────────────│
       │                       │    Execute & Report   │
       │◄──────────────────────│──────────────────────►│
```

### Beaconing
Infected hosts "check in" at regular intervals (sleep time) to:
- Receive new commands
- Report task results
- Exfiltrate data

## 🛠️ Common C2 Frameworks

| Framework | Type | Use |
|-----------|------|-----|
| Cobalt Strike | Commercial | Red team standard |
| [[Metasploit]] | Open source | Pentesting, learning |
| Sliver | Open source | Modern alternative to CS |
| Havoc | Open source | Newer framework |
| Mythic | Open source | Multi-agent, extensible |
| Empire | Open source | PowerShell-based |

## 🔍 Blue Team Detection

### Indicators to Monitor
- **Beaconing patterns** — Regular interval connections
- **DNS anomalies** — High volume TXT queries, long subdomains
- **User-agent strings** — Default/unusual agents
- **JA3/JA3S fingerprints** — TLS client fingerprinting
- **Process behavior** — Unusual parent-child relationships

### Detection Rules
```yaml
# Sigma rule example - Cobalt Strike default pipe
title: CobaltStrike Named Pipe
logsource:
  product: windows
  service: sysmon
detection:
  selection:
    EventID: 17
    PipeName|startswith: '\MSSE-'
  condition: selection
```

## 🛡️ Prevention

| Control | Implementation |
|---------|---------------|
| Egress filtering | Block unnecessary outbound ports |
| DNS monitoring | Detect DNS tunneling |
| SSL inspection | Decrypt and inspect HTTPS |
| EDR | Behavioral detection of beacons |

## 🎤 Interview STAR Example
> **Situation:** SOC detected unusual DNS query patterns from a workstation.
> **Task:** Investigate potential C2 communication.
> **Action:** Analyzed DNS logs, found base64-encoded subdomains querying at 60-second intervals. Isolated host, extracted malware sample, identified Cobalt Strike beacon. Blocked C2 domain at firewall.
> **Result:** Contained breach within 2 hours. Added DNS tunneling detection rules preventing future similar attacks.

## 🔗 Related Concepts
- [[Red Teaming]]
- [[Metasploit]]
- [[Data Exfiltration]]
- [[Phishing]] (common initial access for C2)

## 📚 References
- MITRE ATT&CK T1071 (Application Layer Protocol)
- The C2 Matrix: https://www.thec2matrix.com/
