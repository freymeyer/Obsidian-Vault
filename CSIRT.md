---
dg-publish: true
tags:
  - "#cybersecurity/incident-response/teams"
aliases:
  - Computer Security Incident Response Team
  - CSIRT
---

# CSIRT

> One-liner: A cross-functional team that prepares for, detects, responds to, and recovers from cybersecurity incidents.

## 🎯 What Is It?
A CSIRT is an organisational team with defined roles, authority, and processes to handle security incidents end-to-end. It typically includes technical responders, management, legal, HR, PR/communications, and business owners.

## 🤔 Why It Matters
- Ensures coordinated, timely incident handling aligned to business risk.
- Clarifies decision rights, communication paths, and legal obligations.
- Improves resilience through continuous improvement and lessons learned.

## 🔬 How It Works
### Core Principles
1. Clear scope and definitions (event vs incident).
2. Documented IR plan, playbooks, and communication paths.
3. Authority to act (access, containment, notifications).

### Technical Deep-Dive
- Inputs: alerts, user reports, threat intel, monitoring.
- Outputs: containment actions, tickets/cases, evidence packages, reports.
- Tools: [[Security Information and Event Management system (SIEM)]], case management (e.g., [[TheHive Project]]), EDR, ticketing.

## 🛡️ Detection & Prevention
### How to Detect
- N/A — CSIRT is a function, not a signal.

### How to Prevent / Mitigate
- Train members; run tabletop exercises.
- Maintain on-call rotation and escalation paths.
- Keep up-to-date playbooks and contact lists.

## 🎤 Interview Angles
- "What is a CSIRT and how does it differ from a SOC?"
- "How would you structure CSIRT communications during a ransomware event?"
- "Walk me through CSIRT roles and decision-making."

## ✅ Best Practices
- Define activation criteria and severity model.
- Pre-approve emergency actions (isolate hosts, block accounts).
- Integrate legal and PR early for breach comms.

## ❌ Common Misconceptions
- CSIRT = SOC. SOC monitors; CSIRT leads incident handling across stakeholders.

## 🔗 Related Concepts
- [[Incident Response]]
- [[Security Operations Center (SOC)]]
- [[Alert Triage]]

## 📚 References
- NIST SP 800-61 Rev.2 (Computer Security Incident Handling Guide)
