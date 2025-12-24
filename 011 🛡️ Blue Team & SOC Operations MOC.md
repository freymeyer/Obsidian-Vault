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
    - [[Zeek]] - Network security monitoring and log generation.
    - [[RITA]] - C2 detection via network traffic analysis.
- **Issues:**
    - [[Logging & Alerting Failures]] - Common gaps in visibility.

## 🎯 Threat Hunting
*Proactively searching for threats.*
- [[DNS Tunneling]] - Covert C2/exfiltration via DNS queries.
- [[Command and Control (C2)]] - Attacker infrastructure detection.
- [[Domain Generation Algorithm (DGA)]] - Dynamic domain generation for C2.
- [[Fast Flux]] - IP rotation techniques for evasion.
- [[Lateral Movement]] - Post-exploitation network propagation.
- [[Honeypot]] - Decoy systems for detection.
- **Labs:** [[C2 Detection - Command & Carol]]

## 🚨 Incident Response
*Handling the alerts.*
- [[Alert]] - The initial signal of potential malicious activity.
- [[Alert Triage]] - The process of investigating and prioritizing alerts.
- [[Alert Reporting]] - Documenting findings.
- [[False Positive]] - Benign activity triggering an alert.
## 🛡️ Defense & Prevention
*Security controls and countermeasures.*
- [[Intrusion Prevention System (IPS)]] - Inline threat blocking.
- [[Endpoint detection and response (EDR)]] - Endpoint monitoring and response.
- [[Web Application Firewalls (WAFS)]] - Web attack prevention.
- [[Data Loss Prevention (DLP)]] - Exfiltration prevention.
- [[Multi-Factor Authentication (MFA)]] - Authentication hardening.

## 📚 Frameworks
- [[Cyber Kill Chain]] - Lockheed Martin's 7-stage attack model.
- [[MITRE ATT&CK]] - Adversary tactics and techniques.
- [[Pyramid of Pain]] - Indicator effectiveness hierarchy.