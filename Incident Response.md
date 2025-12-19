---
tags:
  - "#cybersecurity/blue-team/incident-response"
  - "#interview/concepts"
aliases:
  - IR
  - Incident Handling
---

# Incident Response

> **One-liner:** The structured approach to detecting, containing, eradicating, and recovering from security incidents.

## 🎯 What Is It?
Incident Response (IR) is the methodology and process organizations use to handle security breaches and cyberattacks. A well-prepared IR capability minimizes damage, reduces recovery time, and lowers costs.

## 📊 Incident Response Phases (NIST)

```
┌──────────────────────────────────────────────────────────────────┐
│                                                                  │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────────────┐  │
│  │ Preparation │───►│  Detection  │───►│    Containment      │  │
│  │             │    │ & Analysis  │    │ Eradication Recovery│  │
│  └─────────────┘    └─────────────┘    └──────────┬──────────┘  │
│         ▲                                         │              │
│         │           ┌─────────────────┐           │              │
│         └───────────│ Post-Incident   │◄──────────┘              │
│                     │    Activity     │                          │
│                     └─────────────────┘                          │
└──────────────────────────────────────────────────────────────────┘
```

### 1. Preparation
- IR plan documentation
- Team roles and contact lists
- Playbooks for common scenarios
- Tools and access ready
- Training and exercises

### 2. Detection & Analysis
- Alert triage
- Log analysis
- IOC identification
- Scope determination
- Severity classification

### 3. Containment
**Short-term:** Immediate actions (isolate host, block IP)
**Long-term:** Sustainable controls while investigating

### 4. Eradication
- Remove malware
- Close attack vectors
- Patch vulnerabilities
- Reset compromised credentials

### 5. Recovery
- Restore from clean backups
- Rebuild compromised systems
- Monitor for re-infection
- Gradually restore services

### 6. Post-Incident Activity
- Lessons learned meeting
- Update IR procedures
- Improve detections
- Executive report

## 📋 Severity Levels

| Level | Description | Response Time | Example |
|-------|-------------|---------------|---------|
| **Critical** | Active breach, data exfil | Immediate | Ransomware spreading |
| **High** | Confirmed compromise | < 1 hour | Malware on server |
| **Medium** | Suspicious activity | < 4 hours | Phishing click, no execution |
| **Low** | Policy violation | < 24 hours | Unauthorized software |

## 🛠️ Essential IR Tools

| Category | Tools |
|----------|-------|
| Forensics | Volatility, Autopsy, FTK |
| Network | Wireshark, tcpdump, Zeek |
| Endpoint | Velociraptor, KAPE, [[PEStudio]] |
| SIEM | [[Splunk]], [[Elastic]], Sentinel |
| Ticketing | TheHive, JIRA, ServiceNow |

## 🎤 Interview STAR Example
> **Situation:** User reported ransomware note on their desktop Monday morning.
> **Task:** Lead incident response to contain and recover.
> **Action:** Immediately isolated affected host from network. Identified patient zero via EDR timeline. Found phishing email from Friday. Scanned for IOCs across all endpoints—found 3 more infected. Contained all systems, restored from Thursday backups, reset credentials for affected users.
> **Result:** Contained ransomware to 4 systems (out of 500). Full recovery in 18 hours. Implemented email filtering rule blocking similar attachments.

## 💡 Interview Tips
- Know the 6 NIST phases by heart
- Have a ransomware scenario ready
- Understand chain of custody for forensics
- Know when to escalate vs handle internally

## 🔗 Related Concepts
- [[Blue Teaming]]
- [[Security Operations Center (SOC)]]
- [[Detection Engineering]]
- [[threat intelligence|Threat Intelligence]]
- [[Alert Triage]]

## 📚 References
- NIST SP 800-61r2: Computer Security Incident Handling Guide
- SANS Incident Handler's Handbook
