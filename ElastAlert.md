---
tags:
  - "#cybersecurity/blue-team/detection-engineering"
  - "#tools/siem"
  - "#interview/concepts"
aliases:
  - ElastAlert2
---

# ElastAlert

> **One-liner:** An open-source alerting framework that queries Elasticsearch for patterns, anomalies, and threats, then sends notifications to external services.

## 🎯 What Is It?
ElastAlert is a Python-based alerting tool for Elasticsearch that enables security teams to create detection rules that query log data and trigger alerts when conditions are met. It bridges the gap between Elasticsearch (data storage) and external alerting platforms like Slack, Email, PagerDuty, Jira, and more. ElastAlert2 is the actively maintained fork.

## 🤔 Why It Matters
- **Real-time detection** — Continuously queries Elasticsearch for threats
- **Flexible alerting** — Supports 20+ notification types
- **[[Sigma]] integration** — Convert Sigma rules to ElastAlert (via [[Uncoder.io]])
- **Threat hunting** — Proactively search for IOCs and TTPs
- **Cost-effective** — Open-source alternative to commercial SIEM alerting
- **Customizable** — Python-based, highly extensible

## 🔬 How It Works

### Architecture
```
┌─────────────┐      ┌──────────────┐      ┌─────────────┐
│ Elasticsearch│ ←───│  ElastAlert   │────→│   Alerters   │
│   (Logs)    │Query │  (Detection)  │Send │(Slack/Email) │
└─────────────┘      └──────────────┘      └─────────────┘
```

### Rule Execution Flow
1. ElastAlert reads rule files from `/rules` directory
2. Queries Elasticsearch based on rule filters
3. Compares results against rule conditions
4. If match found → Triggers alert
5. Sends alert to configured alerter (email, Slack, etc.)
6. Logs alert to prevent duplicate notifications (based on `realert` setting)

### Rule Types

| Rule Type | Trigger Condition | Use Case |
|-----------|------------------|----------|
| `any` | Any match | IOC hunting, simple detections |
| `frequency` | X events in Y time | Brute-force, failed logins |
| `spike` | Sudden increase | DDoS, data exfiltration |
| `flatline` | Expected data stops | Service outage, log source failure |
| `change` | Field value changes | Account privilege escalation |
| `cardinality` | Unique values threshold | Port scanning, password spray |
| `new_term` | First-seen value | New user, new process, new domain |

## 🛠️ Rule Structure

### Basic ElastAlert Rule
```yaml
name: dns_sinkhole_detection
type: any
index: filebeat-*

filter:
- query_string:
    query: 'dns.resolved_ip:"0.0.0.0"'

alert:
- debug
- slack

slack_webhook_url: "https://hooks.slack.com/..."

realert:
  minutes: 5

description: "Detects DNS queries resolving to sinkhole IP"
```

### Rule Fields Explained

| Field | Definition | Example |
|-------|------------|---------|
| `name` | Unique rule identifier | `suspicious_dns_query` |
| `type` | Rule type (see table above) | `any`, `frequency` |
| `index` | Elasticsearch index pattern | `filebeat-*`, `winlogbeat-*` |
| `filter` | Query to match events | `query_string`, `term`, `range` |
| `alert` | Alerter types | `debug`, `email`, `slack` |
| `realert` | Suppress duplicate alerts | `minutes: 5` (wait 5 min) |

### Advanced Example: Frequency Rule
```yaml
name: multiple_failed_logins
type: frequency
index: winlogbeat-*

num_events: 5
timeframe:
  minutes: 10

filter:
- query_string:
    query: 'event.code:4625 AND event.action:failure'

alert:
- email

email: security-team@company.com
email_subject: "Alert: {} failed login attempts from {}"
email_subject_args:
- num_matches
- source.ip

realert:
  hours: 1
```

## 🛡️ Detection Use Cases

### 1. [[Indicator of Compromise (IOC)]] Detection
```yaml
# Hunt for known malicious IPs
filter:
- terms:
    destination.ip: ["1.2.3.4", "5.6.7.8"]
```

### 2. [[DNS Sinkhole]] Monitoring
```yaml
# Detect connections to sinkholed domains
filter:
- query_string:
    query: 'dns.resolved_ip:"0.0.0.0"'
```

### 3. [[Brute-force]] Detection
```yaml
type: frequency
num_events: 10
timeframe:
  minutes: 5
filter:
- query_string:
    query: 'event.action:failure AND event.category:authentication'
```

## 🎤 Interview Angles

### Common Questions
- "How does ElastAlert differ from Kibana alerting?"
- "What are the limitations of ElastAlert?"
- "How would you tune an ElastAlert rule to reduce false positives?"
- "How do you convert a Sigma rule to ElastAlert?"

### Key Talking Points
- ElastAlert is **query-based** — runs scheduled queries against Elasticsearch
- **Lightweight** — Doesn't require paid Elastic license features
- **Extensible** — Can write custom alert types in Python
- **Requires tuning** — `realert` settings prevent alert fatigue
- **[[Uncoder.io]]** can convert [[Sigma]] → ElastAlert automatically

### STAR Story
> **Situation:** SOC team needed alerting for [[DNS Sinkhole]] hits but Kibana's built-in alerting was limited for complex queries.
> **Task:** Deploy ElastAlert to detect DNS queries resolving to sinkhole IPs and alert immediately.
> **Action:** Created ElastAlert rule querying `filebeat-*` index for `dns.resolved_ip:"0.0.0.0"`. Configured Slack webhook for real-time notifications. Set `realert` to 5 minutes to prevent spam. Converted existing [[Sigma]] rules to ElastAlert using [[Uncoder.io]].
> **Result:** Detected 40 sinkhole hits across 7 infected hosts in first week. Reduced MTTD from hours to minutes. Automated alerting improved SOC response time by 70%.

## ✅ Best Practices
- Use `realert` to prevent duplicate alerts
- Start with `debug` alerter to test rules before production
- Store rules in version control (Git)
- Use descriptive rule names and documentation
- Combine with [[Sigma]] rules for community-driven detections
- Monitor ElastAlert logs for rule errors
- Use aggregations to reduce alert volume
- Set appropriate `buffer_time` for delayed logs

## ❌ Common Misconceptions
- "ElastAlert = SIEM" — It's just the alerting component, not a full SIEM
- "Set and forget" — Rules require continuous tuning
- "Real-time" — Queries run on schedule (e.g., every 1 minute), not instant
- "Replaces Kibana" — Complements Kibana, doesn't replace visualization

## 🆚 Comparison with Similar Tools

| Tool | Type | Licensing | Integration |
|------|------|-----------|-------------|
| **ElastAlert** | Open-source | Free | Elasticsearch only |
| **Kibana Alerts** | Built-in | Free (basic) / Paid (advanced) | Native Elasticsearch |
| **Watcher** | Commercial | Paid (Elastic X-Pack) | Native Elasticsearch |
| **[[Sigma]]** | Rule format | Free | Platform-agnostic |
| **[[Splunk]]** | Commercial SIEM | Paid | Splunk only |

## 🔗 Related Concepts
- [[Sigma]]
- [[Uncoder.io]]
- [[Security Information and Event Management system (SIEM)]]
- [[Detection Engineering]]
- [[Indicator Detection]]
- [[DNS Sinkhole]]
- [[Threat intelligence]]
- [[Elastic]]

## 📚 References
- ElastAlert2 GitHub: https://github.com/jertel/elastalert2
- ElastAlert Documentation: https://elastalert.readthedocs.io/
- Elastic Community: https://discuss.elastic.co/
- SANS: Threat Hunting with ELK
