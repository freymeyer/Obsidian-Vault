---
tags:
  - "#cybersecurity/blue-team/defense"
  - "#cybersecurity/data-protection"
  - "#interview/concepts"
aliases:
  - DLP
  - Data Leak Prevention
---

# Data Loss Prevention (DLP)

> **One-liner:** Security controls that detect and prevent unauthorized transmission or exfiltration of sensitive data.

## 🎯 What Is It?
Data Loss Prevention (DLP) is a set of tools and policies designed to prevent sensitive data from leaving an organization's control. DLP is a critical countermeasure against the **Actions on Objectives** stage of the [[Cyber Kill Chain]], specifically targeting [[Data Exfiltration]].

## 🔬 How It Works

### DLP Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    DLP Components                        │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐              │
│  │ Endpoint │  │ Network  │  │  Cloud   │              │
│  │   DLP    │  │   DLP    │  │   DLP    │              │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘              │
│       │             │             │                     │
│       ▼             ▼             ▼                     │
│  USB/Print     Email/Web     SaaS Apps                 │
│  Local Copy    File Transfer  Cloud Storage            │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### Detection Methods

| Method | Description | Example |
|--------|-------------|---------|
| **Content Inspection** | Analyze file/message content | Credit card numbers, SSNs |
| **Context Analysis** | Evaluate who, where, when | Unusual time, location |
| **Pattern Matching** | Regex for structured data | `\d{3}-\d{2}-\d{4}` (SSN) |
| **Fingerprinting** | Hash-based document matching | Specific confidential docs |
| **Machine Learning** | Classify sensitive content | Unstructured PII detection |

### DLP Actions

| Action | Description |
|--------|-------------|
| **Monitor** | Log and alert, don't block |
| **Block** | Prevent transmission |
| **Encrypt** | Force encryption before allowing |
| **Quarantine** | Hold for review |
| **Notify** | Alert user/admin |
| **Justify** | Require user explanation |

## 📊 DLP Deployment Types

| Type | Location | Protects Against |
|------|----------|------------------|
| **Endpoint DLP** | Workstations | USB, printing, clipboard |
| **Network DLP** | Perimeter | Email, web, FTP |
| **Cloud DLP** | SaaS/IaaS | Cloud storage, apps |
| **Email DLP** | Mail gateway | Email attachments, body |

### Common DLP Solutions

| Vendor | Solution |
|--------|----------|
| Microsoft | Purview DLP |
| Symantec | Data Loss Prevention |
| Forcepoint | DLP |
| Digital Guardian | DLP |
| Zscaler | Cloud DLP |

## 🛡️ Implementation Considerations

### Data Classification
```
Classification Levels:
├── Public - No restrictions
├── Internal - Company only
├── Confidential - Need-to-know
├── Restricted - Highly sensitive (PII, PHI)
└── Secret - Maximum protection
```

### Common Sensitive Data Patterns
```regex
# Credit Card (Visa)
4[0-9]{12}(?:[0-9]{3})?

# SSN
\d{3}-\d{2}-\d{4}

# Email
[\w.-]+@[\w.-]+\.\w+

# AWS Access Key
AKIA[0-9A-Z]{16}
```

## 🎤 Interview Angles

### Common Questions
- "What is DLP and how would you implement it?"
- "How do you balance security with productivity when deploying DLP?"
- "What data classification scheme would you recommend?"

### STAR Story
> **Situation:** Organization had no visibility into data leaving via cloud storage services.
> **Task:** Implement controls to prevent unauthorized data exfiltration.
> **Action:** Deployed cloud DLP integrated with CASB. Defined policies for PII, financial data, and source code. Started in monitor mode, tuned for false positives, then enabled blocking.
> **Result:** Identified and blocked 200+ unauthorized upload attempts in first month. Reduced data exposure risk while maintaining productivity through user education.

## ✅ Best Practices
- Start in monitor mode—understand data flows before blocking
- Define clear data classification policies
- Involve stakeholders (legal, HR, business units)
- Tune policies to reduce false positives
- Educate users on data handling requirements
- Regular policy review and updates

## ❌ Common Pitfalls
- Over-blocking causes user workarounds
- Ignoring encrypted traffic
- No incident response process for DLP alerts
- Treating DLP as "set and forget"

## 🔗 Related Concepts
- [[Data Exfiltration]]
- [[Cyber Kill Chain]]
- [[Insider Threat]]
- [[CASB (Cloud Access Security Broker)]]
- [[Data Classification]]

## 📚 References
- NIST SP 800-122 (Protecting PII)
- Gartner DLP Market Guide
