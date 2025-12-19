---
tags:
  - "#cybersecurity/blue-team/threat-intel"
  - "#interview/concepts"
aliases:
  - Threat Intel
  - TI
  - CTI
  - Cyber Threat Intelligence
---

# Threat Intelligence

> **One-liner:** Evidence-based knowledge about threats that informs defensive decisions and priorities.

## 🎯 What Is It?
Threat intelligence (TI) is the collection, analysis, and dissemination of information about current and potential attacks. It transforms raw data into actionable insights that help organizations understand WHO is attacking, HOW they attack, and WHAT to defend against.

## 📊 Intelligence Types

| Type | Audience | Content | Timeframe |
|------|----------|---------|-----------|
| **Strategic** | Executives, Board | Threat landscape, trends, risk | Long-term |
| **Tactical** | Security managers | TTPs, attack patterns | Medium-term |
| **Operational** | SOC, IR teams | Campaigns, threat actors | Short-term |
| **Technical** | Analysts, Engineers | IOCs, signatures, rules | Immediate |

## 🔬 The Intelligence Cycle

```
    ┌─────────────┐
    │  Planning   │ ← Define requirements
    └──────┬──────┘
           ▼
    ┌─────────────┐
    │ Collection  │ ← Gather data (OSINT, feeds, internal)
    └──────┬──────┘
           ▼
    ┌─────────────┐
    │ Processing  │ ← Normalize, deduplicate, enrich
    └──────┬──────┘
           ▼
    ┌─────────────┐
    │  Analysis   │ ← Correlate, assess, contextualize
    └──────┬──────┘
           ▼
    ┌─────────────┐
    │Dissemination│ ← Share with stakeholders
    └──────┬──────┘
           ▼
    ┌─────────────┐
    │  Feedback   │ ← Evaluate effectiveness
    └─────────────┘
```

## 🛠️ Key Frameworks

| Framework | Purpose |
|-----------|---------|
| [[MITRE ATT&CK]] | Adversary TTPs matrix |
| [[Pyramid of Pain]] | IOC value hierarchy |
| Diamond Model | Intrusion analysis |
| Kill Chain | Attack phase model |
| STIX/TAXII | TI sharing standards |

## 📡 Intelligence Sources

| Source Type | Examples |
|-------------|----------|
| **Open Source (OSINT)** | VirusTotal, AlienVault OTX, abuse.ch |
| **Commercial** | Recorded Future, Mandiant, CrowdStrike |
| **Government** | CISA, FBI Flash, NCSC |
| **ISACs** | FS-ISAC, H-ISAC (sector-specific) |
| **Internal** | Your SIEM, EDR, incident data |

## 📋 Common IOC Types

| IOC | Example | Pyramid Level |
|-----|---------|---------------|
| File hash (MD5/SHA256) | `d41d8cd98f00b204...` | Easy to change |
| IP address | `192.168.1.100` | Easy |
| Domain | `malware.evil.com` | Simple |
| URL | `http://evil.com/payload.exe` | Simple |
| Email address | `attacker@phish.com` | Simple |
| TTPs | "Uses PowerShell for execution" | Tough! |

## 🎤 Interview STAR Example
> **Situation:** Organization had no threat intelligence program; reactive security posture only.
> **Task:** Establish a threat intelligence capability to improve detection.
> **Action:** Integrated 3 free TI feeds (AlienVault OTX, abuse.ch, CISA) into SIEM. Created automated IOC matching. Built weekly threat briefing for SOC team based on sector-relevant threats.
> **Result:** Proactively blocked 47 malicious IPs/domains in first month. Reduced MTTD for known threats by 65%.

## 🔗 Related Concepts
- [[Pyramid of Pain]]
- [[Detection Engineering]]
- [[Indicator Detection]]
- [[Security Operations Center (SOC)]]

## 📚 References
- MITRE ATT&CK: https://attack.mitre.org/
- SANS CTI Summit resources
- CISA Alerts: https://www.cisa.gov/