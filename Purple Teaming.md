---
dg-publish: true
tags:
  - "#cybersecurity/methodology/testing"
  - "#cybersecurity/blue-team/detection"
  - "#cybersecurity/red-team"
  - "#interview/concepts"
aliases:
  - Purple Team
---

# Purple Teaming

> **One-liner:** Collaborative security testing where red and blue teams work together to improve detections, not just find vulnerabilities.

## 🎯 What Is It?
Purple Teaming is a cooperative approach where offensive (red) and defensive (blue) teams join forces to test, validate, and improve detection capabilities. Unlike traditional red teaming (adversarial), purple teaming emphasizes **knowledge transfer and defense improvement**.

**Goal:** Maximize defensive coverage by ensuring blue team can detect what red team executes.

## 📊 Team Comparison

| Aspect | Red Team | Blue Team | Purple Team |
|--------|----------|-----------|-------------|
| **Objective** | Breach undetected | Detect and respond | Improve detections |
| **Relationship** | Adversarial | Defensive | Collaborative |
| **Communication** | Minimal (stealth) | Post-exercise only | Real-time feedback |
| **Output** | What was breached | Alerts generated | Detection gaps filled |
| **Frequency** | Quarterly | Continuous | Ongoing sprints |

## 🔄 Purple Team Workflow

```
1. Red Team: "I'll execute T1003 (Credential Dumping)"
         ↓
2. Red Team executes attack
         ↓
3. Blue Team: "Did we detect it?"
         ↓
4a. YES → Document detection rule
4b. NO → Build new detection, test again
         ↓
5. Iterate to next TTP
```

## 🛠️ Purple Team Exercise Structure

### Pre-Exercise
- **Scope definition** — What TTPs to test (aligned to threat model)
- **Rule baseline** — Document existing detections
- **Success criteria** — 80% detection rate, <5 min alert time, etc.

### During Exercise
```
Day 1: Initial Access + Persistence
09:00 - Red: Phishing simulation
09:15 - Blue: Check SIEM for email alerts
09:30 - Gap identified: No attachment sandbox alerts
10:00 - Blue: Deploy sandbox rule
10:30 - Red: Retest → Blue: Detected ✓

Repeat for scheduled tasks, registry keys...
```

### Post-Exercise
- **Detection coverage report** — What % of MITRE ATT&CK was detected
- **Tuning recommendations** — Reduce false positives
- **Playbook updates** — Document new investigation steps

## 🎯 Purple Team Goals

| Goal | Measurement |
|------|------------|
| **Improve detection** | Coverage % increase (MITRE ATT&CK matrix) |
| **Reduce blind spots** | Critical TTPs without alerts → 0 |
| **Validate tools** | EDR/SIEM catching expected behaviors |
| **Tune alerts** | False positive rate reduction |
| **Train analysts** | Blue team learns attacker TTPs |

## 🛡️ Detection Validation Process

### Example: Credential Dumping (T1003)

**Red Team Execution:**
```powershell
# Mimikatz - dump LSASS
sekurlsa::logonpasswords

# Or Procdump LSASS memory
procdump.exe -ma lsass.exe lsass.dmp
```

**Blue Team Checks:**
- [ ] EDR alerts on LSASS access
- [ ] Sysmon Event ID 10 (ProcessAccess) logs generated
- [ ] SIEM rule triggers on SeDebugPrivilege use
- [ ] Alert reaches SOC dashboard within 2 minutes

**If NOT detected:**
- Build Sysmon rule for LSASS GrantedAccess=0x1010
- Create SIEM correlation: procdump.exe + lsass.exe within 5 seconds
- Test again until blue team detects

## 🎤 Interview Angles

### Common Questions
- "What's the difference between red teaming and purple teaming?"
- "How do you measure the success of a purple team exercise?"
- "When would you choose purple teaming over traditional red teaming?"

### STAR Story Template
**Situation:** SOC had blind spots—ransomware exercise showed 0 alerts before encryption
**Task:** Lead purple team engagement to improve detection coverage
**Action:** Partnered red/blue teams to test 15 TTPs from ransomware kill chain, built 23 new detection rules, tuned 8 existing ones
**Result:** Follow-up test showed 93% detection rate (vs. 12% baseline), discovered ransomware 40 minutes earlier in next real incident

## 🚨 Purple Team vs. Red Team

### When to Use Red Team (Adversarial)
- Test overall security posture
- Validate incident response under realistic stress
- Assess detection when blue team doesn't know attack timing
- Executive leadership wants "are we secure?" answer

### When to Use Purple Team (Collaborative)
- Build detection rules for specific threat actors
- Validate SIEM/EDR tool effectiveness
- Train junior SOC analysts
- Fill detection gaps after breach
- Mature security program focused on improvement

## ✅ Best Practices

- **Align to threat intel** — Test TTPs your org actually faces
- **Map to MITRE ATT&CK** — Track coverage by technique
- **Document everything** — Detection logic, false positives, edge cases
- **Automate validation** — Tools like [[Atomic Red Team]] for repeatable tests
- **Focus on critical assets** — Protect crown jewels first
- **Rotate teams** — Prevent "us vs. them" mentality

### Purple Team Execution Checklist
- [ ] Threat model defines priority TTPs
- [ ] Red team tools match real-world threats
- [ ] Blue team has SIEM/EDR access ready
- [ ] Communication channel established (Slack, Teams)
- [ ] Detection baseline documented
- [ ] Success criteria agreed upon
- [ ] Time-boxed sessions scheduled (2-4 hour sprints)
- [ ] Post-exercise report template ready

## 🛠️ Purple Team Tools

| Category | Tool | Purpose |
|----------|------|---------|
| **TTP Emulation** | [[Atomic Red Team]] | Automated ATT&CK tests |
| | Caldera (MITRE) | Automated adversary emulation |
| | Red Team Automation (RTA) | Elastic's detection validation |
| **Detection** | Splunk / Elastic | SIEM for alert validation |
| | [[Endpoint detection and response (EDR)]] | Endpoint visibility |
| | Sigma rules | Portable detection logic |
| **Collaboration** | AttackIQ | Purple team platform |
| | Vectr.io | Campaign tracking |
| | MITRE ATT&CK Navigator | Coverage visualization |

## 📊 Measuring Purple Team Success

### Detection Coverage Matrix
```
MITRE ATT&CK Tactics:
Initial Access:    █████████░ 90%
Execution:         ███████░░░ 70%
Persistence:       ████████░░ 80%
Privilege Esc:     ██████░░░░ 60%  ← Focus here
Defense Evasion:   █████░░░░░ 50%  ← Focus here
Credential Access: ████████░░ 80%
Discovery:         ██████████ 100%
Lateral Movement:  ███████░░░ 70%
Collection:        █████████░ 90%
Exfiltration:      ████████░░ 80%
C2:                ███████░░░ 70%
```

### Metrics to Track
- **Detection rate** — % of executed TTPs that generated alerts
- **Time to detect** — Median time from execution to SOC alert
- **False positive rate** — Alerts triggered during normal operations
- **Coverage improvement** — Baseline vs. current detection %

## ❌ Common Misconceptions

- **"Purple = less rigorous than red"** — It's differently focused, not easier
- **"One-time event"** — Should be continuous program
- **"Only for mature orgs"** — Actually helps immature programs faster
- **"Red team loses stealth"** — That's the point—collaborative, not adversarial

## 🔗 Related Concepts

- [[Red Teaming]] — Adversarial complement to purple team
- [[Blue Teaming]] — Defensive operations being validated
- [[Detection Engineering]] — Creates rules tested in purple team
- [[Atomic Red Team]] — Automated purple team testing framework
- [[MITRE ATT&CK]] — Framework for mapping detection coverage
- [[Threat Hunting]] — Proactive detection using purple team findings
- [[Tabletop Exercise]] — Discussion-based complement to technical purple team

## 📚 References

- MITRE Engenuity: Purple Teaming with ATT&CK
- Purple Team Exercise Framework (PTEF)
- SANS Purple Teaming: Bridging the Gap
- SpecterOps: Purple Team Methodology
