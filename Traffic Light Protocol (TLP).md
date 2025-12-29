---
tags:
  - "#cybersecurity/blue-team/threat-intelligence"
  - "#interview/concepts"
aliases:
  - TLP
---

# Traffic Light Protocol (TLP)

> **One-liner:** A four-color labeling system (CLEAR, GREEN, AMBER, RED) that governs how widely threat intelligence can be shared.

## 🎯 What Is It?

The **Traffic Light Protocol (TLP)** is a standardized framework created by FIRST.org (Forum of Incident Response and Security Teams) to control the dissemination of sensitive threat intelligence.

Each TLP label indicates who can see and share the information, preventing accidental disclosure that could:
- Breach legal agreements or NDAs
- Violate privacy regulations
- Tip off adversaries that their campaign is detected
- Erode trust with intelligence-sharing partners

## 🚦 TLP Color Codes

| TLP Label | Sharing Boundary | Typical SOC L1 Behavior | Use Cases |
|-----------|------------------|-------------------------|-----------|
| **TLP: CLEAR** | No restrictions; public sharing allowed | Post to internal wiki or public CTI platform | Public threat reports, known malware IOCs |
| **TLP: GREEN** | Share within your community, but not publicly | Upload to MISP/Slack restricted to partner SOCs | ISAC-shared IOCs, industry-specific campaigns |
| **TLP: AMBER** | Organization-wide only; limited external sharing | Keep in internal CTI platform; reference but don't copy | Incident details, client-specific intelligence |
| **TLP: AMBER+STRICT** | Named recipients only within organization | Restricted Slack channel, encrypted notes | Ongoing investigations, legal holds |
| **TLP: RED** | Named individuals only; no further sharing | Encrypted email, verbal briefing only | Active breaches, law enforcement coordination |

### Visual Reference
```
🟢 TLP:CLEAR   → Share anywhere (public)
🟢 TLP:GREEN   → Share with peers (community)
🟠 TLP:AMBER   → Share within organization
🔴 TLP:RED     → Named recipients only
```

## 🔬 How It Works

### TLP in Practice: SOC Workflow

**Scenario:** Analyst receives threat intel report labeled `TLP: AMBER`

1. **Intake:** Note TLP label on first page/email header
2. **Storage:** Save in internal-only CTI platform with TLP tag
3. **Ticket Creation:** Reference IOCs but don't copy full report into ticketing system (external contractors may have access)
4. **Sharing:** Can discuss with coworkers, but cannot forward to partner SOC without permission
5. **Escalation:** Maintain TLP label when escalating to IR team

**Critical Rule:** When combining intelligence from multiple sources, **inherit the most restrictive TLP** label.

### TLP Inheritance Example
```
Source A: IP 1.2.3.4 (TLP:CLEAR)
Source B: IP 1.2.3.4 (TLP:AMBER)
Combined Intel: IP 1.2.3.4 → TLP:AMBER ❗
```

## 📊 Decision Tree: Which TLP to Use

When **producing** threat intelligence:

```
Is this information sensitive or from a private source?
├─ NO  → Can anyone see it without harm?
│         ├─ YES → TLP:CLEAR
│         └─ NO  → TLP:GREEN
└─ YES → Does it contain client/case details?
          ├─ YES → Is it investigation-critical?
          │         ├─ YES → TLP:RED
          │         └─ NO  → TLP:AMBER
          └─ NO  → TLP:GREEN
```

## 🛡️ SOC Analyst Responsibilities

### When Receiving TLP-Labeled Intel
- ✅ **Note the label** on every artifact you store
- ✅ **Respect boundaries** — don't share beyond the label's scope
- ✅ **Ask before escalating** externally (e.g., to vendors, partners)
- ✅ **Preserve labels** when correlating with other intel

### When Creating Reports
- Choose the **least restrictive** TLP that protects sensitive details
- Clearly mark TLP on **every page/slide** of the report
- Include TLP definitions in report appendix for non-technical readers

### Common Violations to Avoid
| ❌ Violation | Why It's Bad | Correct Action |
|--------------|--------------|----------------|
| Posting TLP:AMBER IOCs to public GitHub | Breaches sharing agreement | Use internal GitLab with access controls |
| Forwarding TLP:RED email to IR vendor | Unauthorized external sharing | Get explicit approval first |
| Copying TLP:GREEN intel to Jira (external access) | Community-only intel goes public | Reference, don't duplicate full report |
| Not labeling your own intel | Recipients don't know sharing limits | Always apply TLP when distributing |

## 🎤 Interview Angles

### Common Questions

- **"What is TLP?"**
  - *"Traffic Light Protocol is a four-color system (CLEAR, GREEN, AMBER, RED) that controls how widely threat intelligence can be shared. It prevents accidental disclosure and maintains trust between intel-sharing partners."*

- **"When would you use TLP:AMBER vs TLP:RED?"**
  - *"TLP:AMBER is for org-wide sharing—like incident details I can discuss with my team but not external partners. TLP:RED is for named recipients only, like active breach details coordinated with law enforcement."*

- **"What happens if you violate TLP?"**
  - *"You can breach NDAs, face legal consequences, tip off adversaries that their campaign is detected, and lose access to future intel feeds. It breaks trust with the community."*

- **"How do you handle mixing TLP labels?"**
  - *"Always inherit the most restrictive label. If one source labels an IOC TLP:CLEAR and another labels it TLP:AMBER, the combined intel becomes TLP:AMBER."*

### STAR Story
> **Situation:** Received high-value IOCs from commercial vendor labeled `TLP:AMBER` during active ransomware investigation.
> **Task:** Share IOCs with SOC team while respecting vendor license restrictions.
> **Action:** Added IOCs to internal-only MISP instance with TLP:AMBER tagging. Created SIEM correlation rules referencing IOCs by hash without exposing vendor report. Briefed IR team verbally instead of forwarding vendor email. Documented source attribution in case notes.
> **Result:** Successfully blocked 12 C2 connections using vendor IOCs while maintaining TLP compliance. Vendor renewed our intel-sharing agreement based on proper handling. Post-incident, formalized TLP training for L1 analysts.

## ✅ Best Practices

- **Default to TLP:AMBER** for internal investigations unless cleared for wider sharing
- **Label everything** — No label = confusion and potential violations
- **Train your team** — Ensure all analysts understand TLP boundaries
- **Use platform controls** — Configure MISP/OpenCTI to enforce TLP sharing rules
- **Document TLP sources** — Track where labeled intel came from for audit trails
- **Review before sharing** — Always check TLP label before forwarding anything

## ❌ Common Misconceptions

- **"TLP is just a suggestion"** — No; it's a binding agreement. Violations have legal/business consequences
- **"Internal sharing means anyone in the company"** — No; TLP:AMBER may still exclude external contractors with company access
- **"TLP:GREEN means private"** — No; it means community-shareable (e.g., ISAC members), not public
- **"Older intel loses its TLP"** — No; TLP labels persist unless explicitly changed by the source

## 🔗 Related Concepts

- [[Cyber Threat Intelligence (CTI)]]
- [[Indicator of Compromise (IOC)]]
- [[STIX]]
- [[TAXII]]
- [[MISP]]
- [[Information Sharing]]
- [[ISAC]]

## 📚 References

- [FIRST.org - TLP Definitions v2.0](https://www.first.org/tlp/)
- TryHackMe - Intro to Cyber Threat Intel
- CISA: Traffic Light Protocol (TLP) Guidance
- MISP Project - TLP Implementation Guide
