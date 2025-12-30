---
dg-publish: true
tags:
  - "#cybersecurity/governance"
  - "#compliance"
  - "#interview/concepts"
aliases:
  - GRC
  - Governance Risk Management and Compliance
---

# Governance, Risk and Compliance (GRC)

> **One-liner:** The framework that ensures an organization manages security through policies (Governance), threat mitigation (Risk), and regulatory adherence (Compliance).

## 🎯 What Is It?
GRC is the unified approach to managing three interconnected pillars that keep organizations secure, compliant, and resilient:

1. **Governance** — Policies, procedures, and oversight structures
2. **Risk Management** — Identifying, assessing, and mitigating threats
3. **Compliance** — Meeting legal, regulatory, and contractual obligations

**Why it matters:** Organizations face lawsuits, fines, and reputational damage without GRC—plus they can't prove due diligence to auditors or insurers.

## 📊 The Three Pillars

| Pillar | Focus | Outputs | Example |
|--------|-------|---------|---------|
| **Governance** | How we manage security | Policies, standards, committees | CISO reports to board quarterly |
| **Risk** | What threats we face | Risk register, treatment plans | Ransomware = Critical risk, mitigate via backups |
| **Compliance** | What rules we follow | Audit reports, certifications | Achieve SOC 2 Type II certification |

### How They Connect
```
Governance defines policies
         ↓
Risk identifies threats to those policies
         ↓
Compliance validates you're following policies
         ↓
Governance updates policies based on findings
         (repeat)
```

## 🏛️ Governance Deep Dive

### Key Components
- **Security policies** — Acceptable use, password policy, encryption requirements
- **Standards** — Technical implementation guidelines (e.g., "AES-256 for data at rest")
- **Procedures** — Step-by-step instructions (e.g., incident response runbook)
- **Organizational structure** — CISO, security committee, audit function

### Governance in Action
```
Board of Directors
    ↓
CISO (Chief Information Security Officer)
    ↓
Security Steering Committee
    ↓
├── Policy Team (creates policies)
├── Risk Team (manages threats)
└── Compliance Team (validates adherence)
```

## ⚠️ Risk Management Deep Dive

### Risk Assessment Process
```
1. Identify assets → What needs protecting?
2. Identify threats → Ransomware, insider threat, DDoS
3. Assess vulnerabilities → Unpatched systems, weak passwords
4. Calculate risk → Likelihood × Impact = Risk Score
5. Treat risk → Accept, mitigate, transfer, avoid
```

### Risk Treatment Options

| Strategy | Definition | Example |
|----------|-----------|---------|
| **Mitigate** | Reduce likelihood or impact | Deploy EDR to reduce malware impact |
| **Accept** | Acknowledge risk, do nothing | Legacy system with low criticality |
| **Transfer** | Shift risk to third party | Cyber insurance, cloud provider SLA |
| **Avoid** | Eliminate the activity | Don't process credit cards (avoid PCI-DSS) |

### Risk Matrix
```
           LIKELIHOOD
         Low   Med   High
High    [🟡] [🟠] [🔴]   ← Critical: Immediate action
Impact
Medium  [🟢] [🟡] [🟠]   ← High: Prioritize mitigation
Low     [🟢] [🟢] [🟡]   ← Medium: Monitor and plan
```

## ✅ Compliance Deep Dive

### Common Frameworks

| Framework | Industry | Key Requirements |
|-----------|----------|-----------------|
| **PCI-DSS** | Payment cards | Encrypt card data, segment networks, quarterly scans |
| **HIPAA** | Healthcare | PHI encryption, access controls, breach notification |
| **SOC 2** | SaaS vendors | Security, availability, confidentiality controls |
| **ISO 27001** | General | 114 security controls, ISMS implementation |
| **GDPR** | EU data | Data privacy, right to be forgotten, consent |
| **NIST 800-53** | US Federal | 1000+ controls for government systems |
| **CMMC** | Defense contractors | Maturity-based cybersecurity certification |

### Compliance Audit Workflow
```
1. Gap analysis → Compare current vs. required controls
2. Remediation → Implement missing controls
3. Evidence collection → Screenshots, logs, policies
4. Audit → External auditor validates controls
5. Certification → Receive compliance attestation
6. Continuous monitoring → Maintain compliance year-round
```

## 🛡️ GRC in Security Engineering

### How Security Engineers Use GRC

| Task | Governance Role | Risk Role | Compliance Role |
|------|----------------|-----------|----------------|
| **Deploy firewall** | Follow change mgmt policy | Reduce external attack surface | Meet PCI-DSS req 1.2 |
| **Patch vulnerability** | Per vuln mgmt procedure | Mitigate CVE-2024-XXXX | Satisfy quarterly scan requirement |
| **Incident response** | Follow IR playbook | Contain threat quickly | Document for breach notification |
| **Access review** | Least privilege policy | Insider threat mitigation | SOC 2 quarterly access audit |

## 🎤 Interview Angles

### Common Questions
- "What's the difference between governance and compliance?"
- "How do you balance security risk with business objectives?"
- "Give an example of a risk you'd accept vs. mitigate."

### STAR Story Template
**Situation:** Organization faced potential SOC 2 audit failure due to missing vulnerability management controls
**Task:** Lead GRC initiative to close gaps and achieve certification
**Action:** Implemented risk-based patching (governance policy), prioritized by CVSS score (risk management), documented evidence for auditors (compliance)
**Result:** Achieved SOC 2 Type II on first attempt, reduced critical vulns 85%, formalized security program

## 🛠️ GRC Tools

| Category | Tools | Purpose |
|----------|-------|---------|
| **GRC Platforms** | ServiceNow GRC, RSA Archer, OneTrust | Unified governance, risk, compliance mgmt |
| **Risk Management** | RiskWatch, LogicGate, Resolver | Risk registers, heat maps, treatment tracking |
| **Compliance** | Vanta, Drata, Secureframe | Automated SOC 2, ISO 27001 evidence collection |
| **Policy Management** | PowerDMS, PolicyTech | Centralized policy repository |
| **Audit** | AuditBoard, Workiva | Evidence management for audits |

## 📋 GRC Maturity Levels

| Level | Characteristics | Example Org |
|-------|----------------|------------|
| **1 - Initial** | Ad-hoc, reactive, no documentation | Startup, no formal security |
| **2 - Developing** | Some policies exist, manual compliance | Small business with spreadsheet tracking |
| **3 - Defined** | Documented processes, periodic risk assessments | Mid-size org with GRC tool |
| **4 - Managed** | Metrics tracked, proactive risk mgmt | Enterprise with dedicated GRC team |
| **5 - Optimized** | Continuous improvement, automated monitoring | Fortune 500 with integrated GRC |

## ✅ Best Practices

- **Integrate early** — GRC from Day 1, not after breach or audit failure
- **Risk-based approach** — Prioritize controls by business impact
- **Automate evidence** — Use tools that auto-collect compliance proof
- **Business language** — Translate technical risk to business impact for executives
- **Continuous compliance** — Monitor year-round, not just pre-audit
- **Cross-functional** — GRC requires legal, IT, security, HR collaboration

## ❌ Common Misconceptions

- **"Compliance = Security"** — You can be compliant yet insecure (checkbox mentality)
- **"GRC is only for large orgs"** — Startups need basic GRC (policies, risk tracking)
- **"One-time certification"** — Compliance requires continuous monitoring
- **"GRC slows business"** — Proper GRC enables trust and faster sales

## 🚨 GRC Failures

| Failure | Impact | Example |
|---------|--------|---------|
| **No governance** | Inconsistent security decisions | Every team makes up their own security rules |
| **Ignored risk** | Exploited vulnerabilities | Known critical vuln not patched → breached |
| **Compliance theater** | False sense of security | Pass audit but still get breached (weak passwords allowed) |

## 🔗 Related Concepts

- [[Risk Management]] — Core GRC pillar
- [[Security Policies]] — Governance artifact
- [[Compliance]] — Regulatory adherence pillar
- [[Vulnerability Management]] — Risk mitigation process
- [[Incident Response]] — Governance-defined process
- [[Third-Party Risk]] — Extended GRC to vendors
- [[Security Architecture]] — Implements governance decisions
- [[Audit]] — Validates compliance

## 📚 References

- NIST Risk Management Framework (RMF)
- ISO 31000: Risk Management Guidelines
- COBIT 2019: Governance framework
- COSO Enterprise Risk Management Framework
- OCEG GRC Capability Model
