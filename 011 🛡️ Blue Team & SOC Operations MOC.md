---
tags:
  - "#cybersecurity/blue-team"
---

---
tags:
  - "#cybersecurity/blue-team"
---

# 🛡️ Blue Team & SOC Operations MOC

## 🧠 Core Concepts
- [[Blue Teaming]] - The defensive side of cybersecurity.
- [[Security Operations Center (SOC)]] - The centralized unit for security monitoring.
- [[SOC analysts]] - Roles and responsibilities (Tier 1, 2, 3).
- [[DevSecOps]] - Integrating security into the DevOps pipeline.

## 🔎 Detection Engineering
*Building the logic to catch threats.*
- [[Detection Engineering]] - The overall process of creating detection rules.
- [[Detection Maturity Level Model]] - Assessing the maturity of detections.
- [[Alerting and Detection Strategies Framework]] - Framework for documenting detections.
- **Types of Detection:**
    - [[Indicator Detection]] - Detecting specific IOCs (hashes, IPs).
    - [[Threat-based detection]] - Detecting TTPs (Tactics, Techniques, Procedures).
    - [[Environment-based detection]] - Detecting anomalies in the specific environment.
    - [[Threat Behavior detections]] - Behavioral analysis.

## 🪵 Logging & SIEM
*Collecting and analyzing data.*
- [[Security Information and Event Management system (SIEM)]] - Centralized log management.
- **Tools:**
    - [[Splunk]] - Industry standard SIEM.
    - [[ELK Stack]] - Elastic, Logstash, Kibana (Open source).
        - [[Elasticsearch]] - Search and analytics engine.
        - [[Logstash]] - Data processing pipeline.
        - [[Kibana]] - Visualization dashboard.
    - [[Sysmon]] - Advanced Windows system monitoring.
- **Issues:**
    - [[Logging & Alerting Failures]] - Common gaps in visibility.

## 🚨 Incident Response
*Handling the alerts.*
- [[Alert]] - The initial signal of potential malicious activity.
- [[Alert Triage]] - The process of investigating and prioritizing alerts.
- [[Alert Reporting]] - Documenting findings.
- [[False Positive]] - Benign activity triggering an alert.
