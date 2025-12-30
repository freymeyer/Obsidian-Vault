---
dg-publish: true
tags:
  - "#cybersecurity/blue-team/incident-response"
  - "#training/exercise"
  - "#interview/concepts"
aliases:
  - TTX
  - Tabletop Drill
---

# Tabletop Exercise

> **One-liner:** A discussion-based simulation where the security team walks through incident scenarios without executing technical actions.

## 🎯 What Is It?
A Tabletop Exercise (TTX) is a low-stress, collaborative security drill where stakeholders discuss their roles and responses to a simulated incident. It's like a rehearsal for a play—no actual systems are touched, but everyone practices their lines.

**Purpose:** Validate incident response plans, identify gaps, and build team coordination before a real crisis.

## 📊 Exercise Types Comparison

| Type | Technical Actions | Cost | Stress Level | Use Case |
|------|------------------|------|--------------|----------|
| **Tabletop** | None (discussion) | Low | Low | Plan validation |
| **Simulation** | Limited (sandbox) | Medium | Medium | Tool training |
| **Red Team** | Full attack | High | High | Detection testing |
| **Live Drill** | Controlled outage | High | Very High | Full IR test |

## 🎭 TTX Structure

### 1. Planning Phase
```
Define Objectives → Select Scenario → Invite Participants → Create Injects
```
- **Objectives:** What are we testing? (Ransomware response, data breach, DDoS)
- **Scenario:** Realistic but controlled situation
- **Participants:** CSIRT, IT, legal, PR, management
- **Injects:** Timed scenario updates to simulate evolving incident

### 2. Exercise Phase (2-4 hours)
```
1. Facilitator presents scenario
2. Team discusses initial actions
3. Inject #1: "Malware spread to 50 hosts"
4. Team discusses containment steps
5. Inject #2: "Media is calling for comment"
6. Team discusses communications
7. Continue until incident resolved
```

### 3. Debrief Phase
- What went well?
- What gaps were identified?
- Who was unclear on their role?
- What playbook updates are needed?

## 🛠️ Sample Tabletop Scenarios

### Scenario 1: Ransomware Outbreak
```
09:00 - SOC detects ransomware on accounting workstation
09:15 - 15 additional hosts show encryption activity
09:30 - Ransom note demands $500k in Bitcoin
09:45 - File servers showing encrypted files
10:00 - Threat actor posts sample data on dark web
```

**Discussion prompts:**
- Who declares the incident?
- What systems get isolated?
- Who contacts cyber insurance?
- When do we notify customers?

### Scenario 2: Third-Party Breach
```
08:00 - Vendor notifies you their environment was breached
08:30 - Your organization's data may be exposed
09:00 - No details on what data was accessed
10:00 - Media reports the breach publicly
```

**Discussion prompts:**
- How do you assess impact without vendor details?
- When do you trigger breach notification laws?
- Who handles media relations?

## 🛡️ Blue Team Use Cases

| Objective | What You Validate |
|-----------|------------------|
| **Playbook testing** | Are documented procedures realistic? |
| **Role clarity** | Does everyone know their responsibilities? |
| **Communication** | Are escalation paths clear? |
| **Decision-making** | Who has authority to isolate systems? |
| **External coordination** | Law enforcement, legal, PR integration |

## 🎤 Interview Angles

### Common Questions
- "What's the difference between a tabletop exercise and a red team exercise?"
- "How often should an organization run tabletop exercises?"
- "What makes a tabletop exercise effective?"

### STAR Story Template
**Situation:** Organization had IR plan but never tested it
**Task:** Design and facilitate first tabletop exercise for ransomware scenario
**Action:** Created realistic scenario, invited cross-functional team, documented gaps
**Result:** Identified 8 critical gaps (backup access, legal contacts, comms templates), updated playbooks, reduced mean-time-to-containment by 40% in next real incident

## 🚨 Common Mistakes

| Mistake | Why It Hurts | Fix |
|---------|-------------|-----|
| **Too technical** | Non-technical stakeholders disengage | Balance technical and business discussion |
| **No injects** | Static, boring, unrealistic | Add time pressure and evolving scenario |
| **Wrong participants** | Key decision-makers absent | Mandate C-level attendance |
| **No follow-up** | Gaps identified but never fixed | Assign action items with owners/deadlines |
| **Too scripted** | No room for genuine discussion | Use injects as prompts, not scripts |

## ✅ Best Practices

- **Quarterly cadence** — At minimum, run 4x per year
- **Rotate scenarios** — Don't repeat same incident type
- **Cross-functional** — Include legal, PR, HR, not just IT
- **No-blame environment** — Goal is learning, not punishment
- **Document everything** — Record gaps and action items
- **Track improvements** — Measure gap closure over time

### Tabletop Exercise Checklist
- [ ] Objectives defined and communicated
- [ ] Realistic scenario created
- [ ] Key stakeholders confirmed attendance
- [ ] Injects prepared with timing
- [ ] Facilitator briefed (neutral party preferred)
- [ ] Recording method ready (notes, scribe)
- [ ] Debrief template prepared
- [ ] Action item tracker ready

## 📋 Sample Inject Timeline

| Time | Inject | Teams Involved |
|------|--------|---------------|
| T+0 | Alert fires: Suspicious encrypted files | SOC, IR |
| T+15 | 50 hosts now affected, spreading | SOC, IR, IT |
| T+30 | Backups appear encrypted, ransom note found | IR, IT, Management |
| T+45 | Media asks for comment, regulator inquiry | PR, Legal, C-suite |
| T+60 | Threat actor threatens data leak | IR, Legal, C-suite |

## ❌ Common Misconceptions

- **"It's just a meeting"** — Requires structured facilitation and objectives
- **"Only for SOC"** — Requires cross-functional participation
- **"Technical skills tested"** — Tests process and coordination, not hacking ability
- **"One and done"** — Should be ongoing program, not checkbox

## 🔗 Related Concepts

- [[Incident Response]] — TTX validates IR plans
- [[Disaster Recovery]] — Similar exercise for business continuity
- [[Red Teaming]] — Technical complement to TTX
- [[Purple Teaming]] — Combines red/blue coordination (TTX can prepare for this)
- [[CSIRT]] — Primary participants in TTX
- [[Playbooks]] — Documents being tested in TTX
- [[Business Continuity]] — Often tested alongside TTX

## 📚 References

- NIST SP 800-84: Guide to Test, Training, and Exercise Programs
- CISA Tabletop Exercise Packages (free scenarios)
- SANS Tabletop Exercise Guide
- FEMA Exercise Evaluation Guides (adapted for cyber)
