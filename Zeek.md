---
tags:
  - "#tools/blue-team/network-analysis"
  - "#cybersecurity/blue-team/monitoring"
aliases:
  - Bro
  - Zeek IDS
category: Network Security Monitoring
platform: Linux, macOS
---

# Zeek

> **One-liner:** Open-source network security monitoring tool that passively observes traffic and generates structured logs for analysis.

## 🎯 What Is It?
Zeek (formerly known as Bro) is an open-source network security monitoring (NSM) tool. Unlike firewalls or IDS/IPS systems, Zeek does not use signatures or rules to block traffic. Instead, it passively observes network traffic and converts it into structured, enriched logs for security analysis.

Zeek can monitor traffic via:
- Configured SPAN ports (mirror ports)
- Physical network taps
- Imported packet captures (PCAP files)

## 🤔 Why It Matters
- **Incident Response:** Rich connection metadata for investigations
- **Threat Hunting:** Feed logs to tools like [[RITA]] for C2 detection
- **Forensics:** Historical record of all network activity
- **Visibility:** See what traditional security tools miss

## 📊 NSM Data Types Zeek Covers

| Data Type | Description | Zeek Coverage |
|-----------|-------------|---------------|
| **Transaction Data** | Summarized application-layer records | ✅ Yes |
| **Extracted Content** | Files and artifacts extracted from traffic | ✅ Yes |
| **Full Packet Capture** | Complete packet payloads | ❌ Use tcpdump/Wireshark |
| **Session Data** | Connection metadata | ✅ Yes |

## ⚡ Quick Reference

### Convert PCAP to Zeek Logs
```bash
zeek readpcap <pcapfile> <outputdirectory>

# Example
zeek readpcap ~/pcaps/capture.pcap ~/zeek_logs/capture
```

### Read Logs with zeek-cut
```bash
# Extract specific fields from conn.log
cat conn.log | zeek-cut id.orig_h id.resp_h id.resp_p proto service duration

# Filter DNS queries
cat dns.log | zeek-cut query answers
```

## 📖 Common Log Files

| Log File | Contents |
|----------|----------|
| `conn.log` | Connection records (source, dest, ports, duration, bytes) |
| `dns.log` | DNS queries and responses |
| `http.log` | HTTP requests and responses |
| `ssl.log` | SSL/TLS handshake details |
| `files.log` | File transfers and extracted content |
| `x509.log` | Certificate details |
| `notice.log` | Zeek-generated alerts |
| `weird.log` | Unusual/malformed traffic |

## 🔬 Log Field Examples

### conn.log Key Fields
| Field | Description |
|-------|-------------|
| `id.orig_h` | Source IP |
| `id.resp_h` | Destination IP |
| `id.orig_p` | Source port |
| `id.resp_p` | Destination port |
| `proto` | Protocol (TCP, UDP, ICMP) |
| `service` | Detected service (http, dns, ssl) |
| `duration` | Connection length |
| `orig_bytes` | Bytes sent by source |
| `resp_bytes` | Bytes sent by destination |

## 🔧 Practical Examples

### Find Long Connections (Potential C2)
```bash
cat conn.log | zeek-cut id.orig_h id.resp_h duration | awk '$3 > 3600' | sort -t$'\t' -k3 -rn | head
```

### Extract All DNS Queries
```bash
cat dns.log | zeek-cut query | sort | uniq -c | sort -rn | head -20
```

### Find Large Data Transfers
```bash
cat conn.log | zeek-cut id.orig_h id.resp_h orig_bytes resp_bytes | awk '$3 > 1000000 || $4 > 1000000'
```

## 💡 Pro Tips
- Zeek logs are tab-delimited; use `zeek-cut` for easy field extraction
- Timestamps are in Unix epoch format by default
- Combine with [[RITA]] for automated C2 beacon detection
- Use `files.log` to track extracted files and their hashes
- `weird.log` often contains early indicators of attacks

## 🛠️ Integration with Other Tools

| Tool | Integration |
|------|-------------|
| [[RITA]] | Import Zeek logs for C2 analysis |
| [[Splunk]] / [[Elastic]] | Ingest logs for SIEM correlation |
| [[Suricata]] | Combine with signature-based detection |
| [[Wireshark]] | Deep dive into specific packets |

## 🎤 Interview Angles

### Common Questions
- "What is Zeek and how is it different from an IDS?"
- "What types of logs does Zeek generate?"
- "How would you use Zeek for threat hunting?"

### STAR Story
> **Situation:** Needed to investigate potential [[Data Exfiltration]] after an alert on unusual outbound traffic.
> **Task:** Analyze network traffic to identify the exfiltration method and scope.
> **Action:** Converted PCAP to Zeek logs, analyzed `conn.log` for large outbound transfers, correlated with `dns.log` for suspicious queries. Found base64-encoded data in DNS TXT queries.
> **Result:** Identified [[DNS Tunneling]] exfiltration to attacker-controlled domain. Blocked domain and quantified data loss at ~50MB.

## 📝 My Use Cases
- Used in [[C2 Detection - Command & Carol]] to generate logs for RITA analysis
- Generating network forensics data for incident investigations

## 🔗 Related Tools
- [[RITA]] — C2 detection using Zeek logs
- [[Wireshark]] — Deep packet inspection
- [[Suricata]] — Signature-based IDS
- [[tcpdump]] — Packet capture

## 📚 References
- [Zeek Documentation](https://docs.zeek.org/)
- [Zeek Log Documentation](https://docs.zeek.org/en/master/logs/index.html)
- [Zeek GitHub](https://github.com/zeek/zeek)
