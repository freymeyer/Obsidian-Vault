---
tags:
  - "#cybersecurity/blue-team/soc"
  - "#interview/concepts"
aliases:
  - Blue Team
  - Defensive Security
---

# Blue Teaming

> **One-liner:** Defensive security operations focused on detecting, preventing, and responding to cyber threats.

## 🎯 What Is It?
Blue teaming encompasses all defensive security activities—monitoring, detection, incident response, and hardening. Blue teams protect organizations by identifying and mitigating threats before, during, and after attacks.

## 🔵 Blue Team vs Red Team

| Aspect | Blue Team | Red Team |
|--------|-----------|----------|
| **Goal** | Defend & detect | Attack & exploit |
| **Mindset** | Protective | Adversarial |
| **Activities** | Monitoring, IR, hardening | Pentesting, social engineering |
| **Success Metric** | MTTD, MTTR | Access achieved |
| **Tools** | SIEM, EDR, IDS | Metasploit, Cobalt Strike |

## 🛡️ Core Blue Team Functions

### 1. Security Monitoring
- Log analysis via [[Security Information and Event Management system (SIEM)|SIEM]]
- Network traffic analysis
- [[Endpoint detection and response (EDR)|EDR]] monitoring

### 2. Threat Detection
- [[Detection Engineering]] — Building detection rules
- [[Indicator Detection]] — IOC matching
- [[Threat Behavior detections]] — TTP-based detection

### 3. Incident Response
- Alert triage and investigation
- Containment and eradication
- Recovery and lessons learned

### 4. Threat Intelligence
- [[threat intelligence|Threat intel]] consumption
- IOC integration
- Threat landscape awareness

### 5. Security Hardening
- Vulnerability management
- Configuration hardening
- Patch management

## 🛠️ Essential Blue Team Tools

| Category | Tools |
|----------|-------|
| SIEM | [[Splunk]], [[Elastic]], Microsoft Sentinel |
| EDR | CrowdStrike, Carbon Black, Defender |
| Network | Wireshark, Zeek, Suricata |
| Forensics | Volatility, Autopsy, [[PEStudio]] |

## 📊 Key Metrics

| Metric | Description |
|--------|-------------|
| **MTTD** | Mean Time to Detect |
| **MTTR** | Mean Time to Respond |
| **Alert Volume** | Alerts per day/week |
| **False Positive Rate** | % of alerts that aren't real threats |

## 🎤 Interview STAR Example
> **Situation:** SOC was overwhelmed with 500+ daily alerts, most were false positives.
> **Task:** Reduce alert fatigue and improve detection quality.
> **Action:** Analyzed 30 days of alerts, identified top noise sources, tuned 15 detection rules, created allowlists for known-good behavior.
> **Result:** Reduced alerts by 60%, improved true positive rate from 15% to 45%.

## 🔗 Related Concepts
- [[Red Teaming]]
- [[Security Operations Center (SOC)]]
- [[Detection Engineering]]
- [[Pyramid of Pain]]
- [[MITRE ATT&CK]]

## 📚 References
- SANS Blue Team Operations
- MITRE D3FEND Framework
