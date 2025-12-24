---
tags:
  - "#tools/blue-team/network-analysis"
  - "#cybersecurity/blue-team/threat-hunting"
  - cybersecurity/blue-team/detection-engineering
aliases:
  - Real Intelligence Threat Analytics
category: Threat Hunting / Network Analysis
platform: Linux
---

# RITA

> **One-liner:** Open-source framework for detecting C2 communication by analyzing network traffic via [[Zeek]] logs.

## 🎯 What Is It?
RITA (Real Intelligence Threat Analytics) is an open-source framework created by Active Countermeasures. Its core functionality is to detect [[Command and Control (C2)]] communication by analyzing network traffic captures and logs. RITA correlates captured fields including IP addresses, ports, timestamps, and connection durations to identify suspicious behavior patterns.

## 🔬 How It Works
RITA only accepts network traffic input as [[Zeek]] logs. It runs several analysis modules collecting information like:
- Periodic connection intervals (beaconing)
- Excessive number of DNS queries
- Long FQDNs and random subdomains
- Volume of data over time over HTTPS, DNS, or non-standard ports
- Self-signed or short-lived certificates
- Known malicious IPs via threat intel feeds

## ⚡ Quick Reference

### Installation
```bash
# RITA is typically run via Docker or native install
# See: https://github.com/activecm/rita
```

### Convert PCAP to Zeek Logs
```bash
zeek readpcap <pcapfile> <outputdirectory>
# Example:
zeek readpcap pcaps/suspicious.pcap zeek_logs/suspicious
```

### Import Zeek Logs to RITA
```bash
rita import --logs ~/zeek_logs/suspicious/ --database suspicious
```

### View Analysis Results
```bash
rita view <database-name>
```

## 📖 Detection Features

| Feature | Description |
|---------|-------------|
| **C2 Beacon Detection** | Identifies periodic "check-in" connections |
| **[[DNS Tunneling]] Detection** | Flags excessive queries, long subdomains |
| **Long Connection Detection** | Unusual persistent connections |
| **[[Data Exfiltration]] Detection** | Large outbound data volumes |
| **Threat Intel Feeds** | Cross-references known malicious IPs |
| **Severity Scoring** | Ranks connections by risk level |

## 📊 RITA Results Columns

| Column | What It Shows |
|--------|---------------|
| **Severity** | Risk score based on threat modifiers |
| **Source/Destination** | IP addresses or FQDNs |
| **Beacon** | Likelihood of beaconing behavior (%) |
| **Duration** | Connection length (long = suspicious) |
| **Subdomains** | Number of subdomains (high = suspicious) |
| **Threat Intel** | Matches on threat intel feeds |

## 🔧 Threat Modifiers

| Modifier | Detection Purpose |
|----------|------------------|
| **MIME type/URI mismatch** | HTTP header doesn't match URI |
| **Rare signature** | Unusual TLS handshake patterns |
| **Prevalence** | Low % of hosts connecting to destination |
| **First Seen** | New external host on network |
| **Missing host header** | HTTP connections without host header |
| **Large outgoing data** | Potential exfiltration |
| **No direct connections** | Hidden/complex C2 patterns |

## 🔍 Search Syntax

```bash
# Enter search mode with /
/

# Search fields
dst:<destination>           # Filter by destination
src:<source>                # Filter by source
beacon:>=<percentage>       # Beacon score threshold
sort:duration-desc          # Sort by duration descending

# Example: Find all high-beacon connections to a domain
dst:rabbithole.malhare.net beacon:>=70 sort:duration-desc
```

## 💡 Pro Tips
- Larger datasets provide more accurate results; small captures are prone to false positives
- The `First Seen` modifier is less reliable with short capture windows
- Cross-reference RITA findings with [[VirusTotal]] for IOC validation
- Pivot from RITA results back into [[Zeek]] logs and PCAP for deeper analysis

## 📝 My Use Cases
- Used in [[C2 Detection - Command & Carol]] to detect C2 beacons from captured network traffic
- Threat hunting for persistent connections and DNS anomalies

## 🎤 Interview Angles

### Common Questions
- "What tools would you use to detect C2 traffic?"
- "How do you analyze network traffic for beaconing behavior?"
- "What indicators suggest C2 communication?"

### STAR Story
> **Situation:** Large volume of network traffic needed analysis for potential C2 activity.
> **Task:** Identify any command and control communication without manually reviewing PCAPs.
> **Action:** Converted PCAP to Zeek logs, imported into RITA, analyzed beacon scores and threat modifiers. Identified high-severity connections with regular intervals and long FQDNs.
> **Result:** Detected AsyncRAT C2 beacon to a malicious Cloudflare tunnel. Blocked the C2 domain and initiated incident response.

## 🔗 Related Tools
- [[Zeek]] — Generates the logs RITA analyzes
- [[Wireshark]] — Deep packet inspection
- [[VirusTotal]] — IOC verification
- [[Suricata]] — IDS for signature-based detection

## 📚 References
- [RITA GitHub - Active Countermeasures](https://github.com/activecm/rita)
- [Active Countermeasures Blog](https://www.activecountermeasures.com/blog/)
- [Zeek Log Documentation](https://docs.zeek.org/en/master/logs/index.html)
