---
tags:
  - "#tools/threat-hunting"
  - "#cybersecurity/blue-team/detection-engineering"
  - "#interview/tools"
aliases:
  - Uncoder
  - SOC Prime Uncoder
---

# Uncoder.io

> **One-liner:** An online platform that translates detection rules and IOC lists between different SIEM, EDR, and XDR query formats.

## 🎯 What Is It?
Uncoder.io is a free web-based tool developed by SOC Prime that converts security detection content between different formats. It supports translation of:
- **[[Sigma]] rules** → SIEM queries (Splunk, Elastic, QRadar, etc.)
- **IOC lists** → Hunting queries
- **SIEM queries** → Other SIEM formats

This enables security teams to reuse detection logic across different security platforms without manually rewriting queries.

## 🤔 Why It Matters
- **Platform portability** — Move detections between SIEMs
- **Time-saving** — Automatic conversion vs. manual rewriting
- **Reduced errors** — Programmatic translation more accurate than manual
- **Community sharing** — Share Sigma rules, not vendor-specific queries
- **IOC operationalization** — Convert [[Threat intelligence]] IOCs to executable queries
- **MITRE ATT&CK integration** — Maps rules to tactics/techniques

## 🔬 How It Works

### Supported Conversions

#### Source Formats (Input)
- [[Sigma]] (universal detection format)
- IOC Lists (IPs, domains, URLs, hashes, emails, files)
- Splunk SPL
- Elastic Query DSL / KQL
- QRadar AQL
- Microsoft Sentinel KQL
- And 40+ other formats

#### Target Formats (Output)
- Elastic (Query DSL, KQL, [[ElastAlert]])
- Splunk (SPL)
- Microsoft Sentinel / Defender XDR (KQL)
- Chronicle / Google SecOps (YARA-L)
- IBM QRadar (AQL)
- CrowdStrike (Falcon Query Language)
- Carbon Black (CB Query)
- And 40+ other platforms

### IOC Translation Example

**Input:** IOC List (IPs)
```
192.168.1.100
10.0.0.50
172.16.5.20
```

**Output:** Elastic KQL
```kql
destination.ip:(192.168.1.100 OR 10.0.0.50 OR 172.16.5.20)
```

**Output:** Splunk SPL
```spl
dest_ip IN ("192.168.1.100", "10.0.0.50", "172.16.5.20")
```

### Sigma Translation Example

**Input:** [[Sigma]] Rule
```yaml
title: Suspicious PowerShell Execution
logsource:
  product: windows
detection:
  selection:
    CommandLine|contains: 
      - 'Invoke-Mimikatz'
      - '-EncodedCommand'
  condition: selection
```

**Output:** Elastic KQL
```kql
process.command_line:(*Invoke-Mimikatz* OR *-EncodedCommand*)
```

**Output:** [[ElastAlert]] Rule
```yaml
filter:
- query_string:
    query: 'process.command_line:(*Invoke-Mimikatz* OR *-EncodedCommand*)'
```

## 🛠️ Practical Use Cases

### 1. [[Threat intelligence]] IOC Hunting
**Scenario:** Receive IOC list from threat intel feed
**Steps:**
1. Paste IPs/domains into Uncoder.io
2. Select "IOCs" as source
3. Select target platform (e.g., Elastic Query)
4. Copy generated query
5. Execute in Kibana to hunt for IOCs

### 2. [[Sigma]] Rule Deployment
**Scenario:** Found community Sigma rule for [[Malware]] detection
**Steps:**
1. Copy Sigma rule YAML
2. Paste into Uncoder.io
3. Select "Sigma" → "ElastAlert"
4. Deploy generated ElastAlert rule
5. Start alerting on technique

### 3. SIEM Migration
**Scenario:** Migrating from Splunk to Elastic
**Steps:**
1. Export Splunk detection rules
2. Convert each rule via Uncoder.io
3. Review/tune generated Elastic queries
4. Deploy in new Elastic environment

## 🎤 Interview Angles

### Common Questions
- "What is Uncoder.io used for?"
- "How does Uncoder.io support Sigma rules?"
- "What are the limitations of automated query translation?"

### Key Talking Points
- **Not a detection platform** — Just a translator
- **Sigma-first approach** — Sigma rules are vendor-agnostic
- **Requires validation** — Always test translated queries
- **SOC Prime Threat Detection Marketplace** — Access to 100k+ Sigma rules
- **Defanging support** — Automatically cleans defanged IOCs (`1[.]2[.]3[.]4` → `1.2.3.4`)

### STAR Story
> **Situation:** Received threat intel report with 15 malicious IPs related to [[Ransomware]] campaign. Needed to hunt for them in Elasticsearch logs immediately.
> **Task:** Convert IOC list to Elastic query for threat hunting.
> **Action:** Used Uncoder.io to paste IOC list, selected "IOCs" as source and "Elastic Query" as target. Tool automatically defanged IPs and generated KQL query. Ran query in Kibana against `filebeat-*` index for past 30 days.
> **Result:** Identified 48 connections to 7 of the malicious IPs from a compromised host. Isolated host, blocked IPs at firewall, prevented ransomware deployment. Total time from intel to detection: 5 minutes.

## ✅ Best Practices
- **Always test** translated queries before deploying to production
- **Review field mappings** — Ensure source fields match your data
- **Use Sigma as source** — Most reliable conversion path
- **Validate IOC defanging** — Check brackets removed correctly
- **Check for duplicates** — IOC lists may have redundant entries
- **Document conversions** — Track which rules came from Uncoder
- **Leverage SOC Prime platform** — Access curated Sigma rules

## ❌ Common Misconceptions
- "Perfect translation" — May require manual tuning for edge cases
- "One-click deployment" — Still need to understand the query logic
- "Replaces detection engineering" — Tool assists, doesn't replace expertise
- "Free forever" — Basic translation free; advanced features may require account

## 🆚 Comparison with Similar Tools

| Tool | Type | Formats | Cost |
|------|------|---------|------|
| **Uncoder.io** | Web-based converter | 50+ formats | Free (account required) |
| **sigmac** | CLI tool | 20+ formats | Free (deprecated) |
| **pySigma** | Python library | Extensible | Free |
| **Sentinel Converter** | Microsoft tool | KQL only | Free |

## 🔗 Related Concepts
- [[Sigma]]
- [[ElastAlert]]
- [[Detection Engineering]]
- [[Indicator of Compromise (IOC)]]
- [[Threat intelligence]]
- [[Security Information and Event Management system (SIEM)]]
- [[Elastic]]
- [[Splunk]]

## 📚 References
- Uncoder.io: https://uncoder.io/
- SOC Prime Threat Detection Marketplace: https://tdm.socprime.com/
- Sigma GitHub: https://github.com/SigmaHQ/sigma
- pySigma Documentation: https://sigmahq-pysigma.readthedocs.io/
