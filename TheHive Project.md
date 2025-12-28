---
tags:
  - "#cybersecurity/incident-response/case-management"
aliases:
  - TheHive
---

# TheHive Project

> One-liner: Open-source incident response and case management platform for SOC/CSIRT workflows.

## 🎯 What Is It?
TheHive provides case, task, and artifact management, analyst collaboration, and integrations with Cortex analyzers for enrichment and automation.

## 🤔 Why It Matters
- Centralises investigations and evidence.
- Streamlines triage, tasking, and reporting.
- Enables automation and enrichment at scale.

## 🔬 How It Works
### Core Principles
1. Cases contain tasks, observables, and timelines.
2. Collaboration via comments, assignments, and templates.
3. Integration with Cortex for automated analysis/playbooks.

### Technical Deep-Dive
- Artifacts: IOCs (hashes, URLs, IPs) with tags and TLP.
- Templates: standardise investigations by type (e.g., phishing, malware).
- APIs: integrate with SIEM/EDR and ticketing.

## 🛡️ Detection & Prevention
### How to Prevent / Mitigate
- Use templates+workflows to reduce MTTR and improve consistency.

## 🎤 Interview Angles
- "How would you model a phishing case in TheHive?"

## ✅ Best Practices
- Define severity model and case fields.
- Enforce naming, tagging, and ownership conventions.

## ❌ Common Misconceptions
- It’s not a SIEM — it manages investigations, not raw logs.

## 🔗 Related Concepts
- [[Incident Response]]
- [[Security Information and Event Management system (SIEM)]]
- [[SOAR]]

## 📚 References
- https://thehive-project.org/
