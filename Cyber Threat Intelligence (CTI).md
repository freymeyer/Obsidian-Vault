---
tags:
  - "#cybersecurity/blue-team/threat-intelligence"
  - "#interview/concepts"
aliases:
  - CTI
  - Threat Intelligence
---

# Cyber Threat Intelligence (CTI)

> **One-liner:** Analyzed information about adversaries and attack patterns that enables proactive, context-driven security decisions.

## 🎯 What Is It?

Cyber Threat Intelligence (CTI) is the process of collecting, analyzing, and applying information about threat actors, their tactics, and indicators to answer three critical questions:

1. **Who or what is behind this indicator?**
2. **What was their behavior in the past?**
3. **How should my organization respond right now?**

CTI transforms raw data into actionable intelligence that helps SOC analysts distinguish genuine threats from noise and make confident triage decisions within minutes.

## 🔬 Intelligence Pyramid

| Layer | Definition | Example | SOC L1 Action |
|-------|------------|---------|---------------|
| **Data** | Unprocessed observable | `45.155.205.3:443` | Capture the artifact |
| **Information** | Data + factual annotation | IP registered to Hetzner, first seen 2023-07-14 | Record attributes |
| **Intelligence** | Analyzed information answering "so what?" | IP belongs to current BumbleBee C2; block immediately | Escalate or suppress |

CTI is about climbing this pyramid—enriching artifacts until they qualify as actionable intelligence.

## 📊 CTI Classifications

| Type | Focus | Audience | Example |
|------|-------|----------|---------|
| **Strategic** | High-level trends & business risk | Executive leadership | Annual ransomware report predicting healthcare shift |
| **Tactical** | Adversary TTPs & behaviors | SOC analysts, hunters | Advisory on new T1059.005 abuse in malspam |
| **Operational** | Campaign-specific motives & intent | IR teams, security ops | Details on specific APT targeting our industry |
| **Technical** | Atomic indicators (IPs, hashes, domains) | SOC L1 analysts | Daily IOC feed for firewall blocks |

**SOC L1 analysts** primarily work with **Technical Intel**, escalate **Tactical IOAs**, and identify patterns feeding **Operational reporting**.

## 🔄 Threat Intelligence Lifecycle

### 1. **Direction** — Define the Mission
- Identify critical assets and business risks
- Formulate intelligence requirements (IRs)
- Ask: *"What threats target our environment?"*

### 2. **Collection** — Gather Raw Material
- Internal telemetry (SIEM, EDR)
- Commercial threat feeds
- Open-source intelligence (OSINT)
- Community & ISAC sharing

### 3. **Processing** — Normalize & Correlate
- Standardize formats (IPv6 compression, lowercase domains)
- Deduplicate against existing indicators
- Tag with source, date, [[Traffic Light Protocol (TLP)]]
- Convert to actionable formats (CSV, YARA rules)

### 4. **Analysis** — Turn Information into Judgment
- Grade confidence: High / Medium / Low
- Cross-check against local telemetry
- Map to [[MITRE ATT&CK]] TTPs
- Assess relevance to organization

### 5. **Dissemination** — Deliver to Stakeholders
- Firewall team: CSV blocklists
- EDR team: YARA rules
- CTI platform: Full tagged indicators
- Management: Executive summary

### 6. **Feedback** — Measure & Refine
- Track KPIs (dwell time, false positive rate)
- Update direction based on outcomes
- Close the loop for continuous improvement

## 🎯 Key Intelligence Artifacts

### [[Indicator of Compromise (IOC)]]
Evidence that a breach occurred (e.g., C2 IP in logs, malware hash on disk).

### [[Indicator of Attack (IOA)]]
Malicious action in progress (e.g., PowerShell launching unknown service).

### [[Tactics, Techniques, and Procedures (TTP)]]
Adversary's detailed methodologies mapped to [[MITRE ATT&CK]] IDs.

## 🔍 Common Indicator Types for L1 Triage

| Indicator Type | Example | Enrichment Sources |
|----------------|---------|-------------------|
| **IPv4/IPv6** | `45.155.205.3` | WHOIS, VirusTotal, Shodan |
| **Domain/FQDN** | `malicious-updates[.]net` | WHOIS age, passive DNS, urlscan.io |
| **URL** | `hxxp://evil[.]net/login` | URLhaus, urlscan.io, Any.Run |
| **File Hash** | `e99a18c428cb38d5...` | VirusTotal, Hybrid-Analysis |
| **Email** | `billing@evil-corp.com` | MXToolbox, HaveIBeenPwned |
| **Registry Key** | `HKCU\Software\Run\updater.exe` | Sigma rules, EDR prevalence |

## 🛡️ Detection & Prevention

### How SOC Analysts Use CTI
- **Pre-emptive Blocking:** Feed high-confidence IOCs to NGFW/EDR
- **Alert Enrichment:** Contextualize SIEM alerts with threat actor profiles
- **Threat Hunting:** Search logs for IOCs before they trigger alerts
- **Incident Response:** Map observed TTPs to known campaigns

### CTI Sources

| Source Type | Examples | Pros | Cons |
|-------------|----------|------|------|
| **Internal** | SIEM logs, EDR detections | Highest relevance | Limited to your view |
| **Commercial** | Vendor feeds, paid sandboxes | High fidelity | Licensing restrictions |
| **OSINT** | AbuseIPDB, URLhaus | Free, community-driven | Requires validation |
| **ISACs** | FS-ISAC, H-ISAC | Sector-specific, rich context | Membership required |

## 🎤 Interview Angles

### Common Questions
- **"What is Cyber Threat Intelligence?"**
  - *"CTI is analyzed information about threats that helps defenders make proactive decisions—like knowing an IP is a known C2 server before it hits your network."*

- **"How does CTI help SOC analysts?"**
  - *"It provides context. Instead of investigating 200 alerts blindly, CTI tells me which IPs are known malicious, which TTPs match active campaigns, so I can prioritize and escalate accurately."*

- **"What's the difference between data, information, and intelligence?"**
  - *"Data is raw (an IP address). Information is data with context (IP belongs to Hetzner, seen in 2023). Intelligence answers 'so what?' (This IP is active BumbleBee C2—block it now)."*

### STAR Story
> **Situation:** Morning shift started with 200 SIEM alerts—mix of benign scans and potential C2 beacons.
> **Task:** Triage alerts using CTI to identify genuine threats within 30 minutes.
> **Action:** Enriched IPs using VirusTotal and internal threat platform. Found one IP matched known BumbleBee C2 infrastructure with high confidence from two commercial feeds. Immediately escalated with full context: IOC, TTP (T1071.001 Web C2), and detection timeline.
> **Result:** IR team contained lateral movement within 15 minutes. Post-incident review showed CTI enrichment saved 2+ hours of investigation time and prevented data exfiltration.

## ✅ Best Practices

- **Enrich every indicator** — Don't just collect IOCs, understand their context
- **Respect [[Traffic Light Protocol (TLP)]]** — Honor sharing boundaries to maintain trust
- **Gradual feed adoption** — Start with 1-2 high-fidelity feeds, expand based on actionability
- **Maintain platform hygiene** — Deduplicate, age out stale IOCs, track confidence scores
- **Close the feedback loop** — Measure false positives, dwell time, and update direction accordingly
- **Document attribution** — Always note the source of intelligence for legal/audit purposes

## ❌ Common Misconceptions

- **"CTI is just IOC lists"** — Technical IOCs are one layer; TTPs, context, and analysis are equally critical
- **"More feeds = better defense"** — Over-ingesting feeds creates alert fatigue; quality > quantity
- **"CTI is only for large enterprises"** — Free OSINT and community feeds make CTI accessible to any SOC
- **"Intelligence sharing is always safe"** — Violating TLP labels can breach NDAs and tip off adversaries

## 🔗 Related Concepts

- [[Indicator of Compromise (IOC)]]
- [[Indicator of Attack (IOA)]]
- [[Tactics, Techniques, and Procedures (TTP)]]
- [[Traffic Light Protocol (TLP)]]
- [[MITRE ATT&CK]]
- [[MITRE D3FEND]]
- [[Cyber Kill Chain]]
- [[STIX]]
- [[TAXII]]
- [[Detection Engineering]]
- [[SIEM]]
- [[Threat Hunting]]

## 📚 References

- TryHackMe - Intro to Cyber Threat Intel Room
- FIRST.org - TLP Definitions
- MITRE ATT&CK Framework
- NIST SP 800-150: Guide to Cyber Threat Information Sharing
