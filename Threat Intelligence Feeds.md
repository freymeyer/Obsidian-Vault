---
dg-publish: true
tags:
  - "#cybersecurity/threat-intel"
  - "#cybersecurity/blue-team/soc"
  - "#interview/concepts"
aliases:
  - TI Feeds
  - IOC Feeds
  - Threat Feeds
---

# Threat Intelligence Feeds

> **One-liner:** Continuously updated streams of [[Indicator of Compromise (IOC)|IOCs]], malicious IPs, domains, and file hashes shared by security vendors and organizations to enable proactive threat detection and blocking.

## 🎯 What Is It?
Threat Intelligence Feeds are curated, regularly updated lists of known malicious indicators (IPs, domains, URLs, file hashes, email addresses) distributed by threat intelligence providers. These feeds enable organizations to detect and prevent attacks by comparing network traffic, DNS queries, and file activity against known bad indicators.

## 🤔 Why It Matters
- **Proactive defense** — Block threats before they compromise systems
- **Faster detection** — Match known IOCs instantly vs. behavioral analysis
- **Shared knowledge** — Benefit from other organizations' incident data
- **Reduced dwell time** — Stop known threats immediately
- **Cost-effective** — Leverage community intelligence vs. building own
- **Compliance** — Many frameworks require threat intelligence integration

## 📊 Types of Threat Intelligence

| Type | Description | Audience | Use Case |
|------|-------------|----------|----------|
| **Strategic** | High-level trends, threat landscape | Executives | Risk assessment, budgeting |
| **Tactical** | TTPs, campaigns, threat actor behavior | Security managers | Detection strategy |
| **Operational** | Ongoing campaigns, attacker intent | SOC analysts | Active threat hunting |
| **Technical** | IOCs (IPs, domains, hashes) | Security engineers | **Feed-based blocking/detection** |

**This note focuses on Technical Intelligence (IOC-based feeds).**

## 🔬 How It Works

### Feed Consumption Workflow
```
1. Subscribe to threat intelligence feed
   ↓
2. Feed provider publishes new IOCs
   ↓
3. Feed consumed by security tools:
   - [[Firewall]] (IP blocking)
   - [[DNS Sinkhole]] (domain blocking)
   - [[Email Gateway]] (domain/IP blocking)
   - [[Security Information and Event Management system (SIEM)|SIEM]] (IOC matching)
   - [[Endpoint detection and response (EDR)|EDR]] (hash blocking)
   ↓
4. Tools compare traffic against IOCs
   ↓
5. Match found → Block/Alert
   ↓
6. Security team investigates alerts
```

### Common IOC Types in Feeds

| IOC Type | Example | Use Case |
|----------|---------|----------|
| **IP Address** | `192.0.2.100` | Block [[Command and Control (C2)]] connections |
| **Domain** | `evil-phish.com` | [[DNS Sinkhole]], [[Email Gateway]] blocking |
| **URL** | `http://malware.net/payload.exe` | Web proxy blocking |
| **File Hash (MD5/SHA256)** | `d41d8cd98f00b204e9800998ecf8427e` | [[Antivirus (AV)]], [[Endpoint detection and response (EDR)|EDR]] blocking |
| **Email Address** | `attacker@evil.com` | Email Gateway filtering |
| **SSL Certificate** | Cert thumbprint | TLS inspection blocking |

## 🛠️ Popular Threat Intelligence Feed Providers

### Free/Community Feeds
| Provider | Type | Focus |
|----------|------|-------|
| **abuse.ch** | Malware URLs, IPs | [[Malware]], [[Ransomware]] |
| **AlienVault OTX** | Community IOCs | General threats |
| **Emerging Threats** | Suricata/Snort rules | Network IDS/IPS |
| **Spamhaus** | IP/domain blocklists | [[Spam]], phishing |
| **PhishTank** | Phishing URLs | [[Phishing]] |
| **CISA** | Government advisories | Critical infrastructure |
| **VirusTotal** | File/URL reputation | Malware analysis |

### Commercial Feeds
| Provider | Coverage | Best For |
|----------|----------|----------|
| **Recorded Future** | Real-time, broad | Enterprise SOC |
| **Mandiant** | APT, targeted attacks | Threat hunting |
| **CrowdStrike** | Falcon Intelligence | [[Endpoint detection and response (EDR)|EDR]] integration |
| **Anomali** | ThreatStream | SIEM/SOAR integration |
| **Palo Alto Unit 42** | Network IOCs | Firewall integration |

## 🛡️ Threat Intelligence Consumers vs. Producers

### Consumers
Organizations that **use** threat intelligence feeds to improve security:
- **Prevention**: Block known bad IPs/domains at [[Firewall]]/[[DNS Sinkhole]]
- **Detection**: Alert on IOC matches in [[Security Information and Event Management system (SIEM)|SIEM]]/[[Endpoint detection and response (EDR)|EDR]]
- **Incident Response**: Validate IOCs during investigations
- **Threat Hunting**: Proactively search logs for historical IOC matches

### Producers
Organizations that **create and share** threat intelligence:
- Collect IOCs from internal incidents and honeypots
- Analyze [[Malware]] samples to extract IOCs
- Monitor threat actor infrastructure
- Publish feeds/reports for community

Most organizations are **consumers**. Becoming a producer requires:
- Large-scale monitoring infrastructure
- Malware analysis capabilities
- Threat research team
- Legal/privacy review process

## 🎤 Interview Angles

### Common Questions
- "How do threat intelligence feeds improve security?"
- "What's the difference between a consumer and producer?"
- "How would you operationalize threat intelligence in a SOC?"
- "What are the limitations of IOC-based threat intelligence?"

### Key Talking Points
- Feeds are **reactive** — Only detect known threats
- **Context matters** — Not all IP matches are malicious (shared hosting)
- **Feed quality varies** — False positives common in low-quality feeds
- **Expiration** — IOCs have limited shelf life (IPs get reassigned)
- **Integration is key** — Feeds useless without automation
- **[[Pyramid of Pain]]** — IP/domain IOCs are "easy" for attackers to change

### STAR Story
> **Situation:** SOC had no threat intelligence capability, only reactive detections after incidents occurred.
> **Task:** Integrate threat intelligence feeds to proactively block and detect known threats.
> **Action:** Subscribed to abuse.ch, AlienVault OTX, and Spamhaus feeds. Integrated feeds into firewall (IP blocking), DNS server (domain sinkholing), and [[Elastic]] [[Security Information and Event Management system (SIEM)|SIEM]] (IOC matching). Used [[Uncoder.io]] to convert IOC lists to Elastic queries. Created [[ElastAlert]] rules for IOC hits. Configured daily feed updates via cron job.
> **Result:** Blocked 47 malicious IPs/domains proactively in first month. Detected 3 compromised hosts via IOC matches before data exfiltration. Reduced MTTD for known threats by 65%.

## ✅ Best Practices
- **Use multiple feeds** — Combine commercial + community for coverage
- **Validate feed quality** — Track false positive rates
- **Automate ingestion** — Manual updates don't scale
- **Set expiration** — Remove stale IOCs (IPs reassigned)
- **Contextualize hits** — Not all matches = compromise
- **Log all matches** — Feed IOC → [[Security Information and Event Management system (SIEM)|SIEM]] for investigation
- **Test before production** — New feed may cause false positives
- **Monitor feed freshness** — Alert if feed stops updating
- **Combine with behavioral detection** — Feeds alone insufficient

## ❌ Common Misconceptions
- "Threat intel = no more breaches" — Only stops known threats
- "All IOCs are bad forever" — IPs/domains get recycled
- "Set and forget" — Requires continuous tuning and validation
- "Free feeds = good enough" — Commercial feeds provide more context
- "IOC feeds replace detection engineering" — Complement, don't replace

## 🆚 Feed Format Standards

| Format | Description | Support |
|--------|-------------|---------|
| **STIX/TAXII** | Structured format for TI sharing | Enterprise tools |
| **CSV/JSON** | Simple lists | Easy integration |
| **MISP** | Open-source TI platform | Community sharing |
| **OpenIOC** | XML-based IOC format | Legacy |

## 🔗 Related Concepts
- [[Indicator of Compromise (IOC)]]
- [[Cyber Threat Intelligence (CTI)]]
- [[Indicator Detection]]
- [[Pyramid of Pain]]
- [[DNS Sinkhole]]
- [[Firewall]]
- [[Email Gateway]]
- [[Security Information and Event Management system (SIEM)]]
- [[Uncoder.io]]
- [[ElastAlert]]

## 📚 References
- MITRE ATT&CK: Threat Intelligence
- SANS: Consuming Threat Intelligence
- CISA Cybersecurity Advisories: https://www.cisa.gov/cybersecurity-advisories
- abuse.ch: https://abuse.ch/
- AlienVault OTX: https://otx.alienvault.com/
- STIX/TAXII: https://oasis-open.github.io/cti-documentation/
