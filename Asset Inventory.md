---
dg-publish: true
tags:
  - "#cybersecurity/governance/asset-management"
  - "#interview/concepts"
aliases:
  - Asset Management
  - Asset Register
---

# Asset Inventory

> **One-liner:** A comprehensive catalog of an organization's digital and physical assets with ownership, location, and configuration details.

## 🎯 What Is It?
Asset Inventory (also called Asset Management or Asset Register) is the foundation of organizational security—a detailed record of all hardware, software, data, and cloud resources. If you don't know what you have, you can't protect it.

Security engineers use asset inventories to:
- Define security perimeter and attack surface
- Prioritize vulnerability management
- Track ownership and accountability
- Enable incident response and forensics
- Support compliance audits

## 📊 Asset Types

| Asset Type | Examples | Critical Attributes |
|------------|----------|-------------------|
| **Hardware** | Servers, laptops, mobile devices | Serial number, IP, location, owner |
| **Software** | Applications, OS, databases | Version, license, install date |
| **Data** | Customer records, IP, configs | Classification, location, access |
| **Network** | Routers, firewalls, switches | IP range, VLAN, interfaces |
| **Cloud** | EC2, S3 buckets, SaaS apps | Account ID, region, IAM roles |
| **People** | Employees, contractors | Access level, dept, onboarding date |

## 🛠️ Essential Inventory Fields

Every asset record should include:
```yaml
Asset Name: PROD-WEB-01
Type: Virtual Server
IP Address: 10.0.1.50
Operating System: Ubuntu 22.04 LTS
Owner: DevOps Team (John Smith)
Location: AWS us-east-1
Business Function: Customer Portal (Critical)
Access: Public-facing
Last Updated: 2025-12-30
```

## 🔄 Asset Lifecycle Management

```
Acquisition → Deployment → Maintenance → Decommissioning
    ↓            ↓             ↓              ↓
 Approve      Configure     Update        Sanitize
 Budget       Baseline      Patch         Archive
 Procure      Document      Monitor       Remove
```

## 🛡️ Detection & Prevention

### Blue Team Use Cases
- **Unauthorized assets** — Alert on unknown devices connecting to network
- **Shadow IT** — Discover unapproved cloud services via DNS logs
- **Configuration drift** — Detect changes from security baseline
- **EOL software** — Identify unsupported OS/applications
- **Missing patches** — Cross-reference CVEs against inventory

### Tools
| Tool | Purpose |
|------|---------|
| **ServiceNow** | CMDB and asset tracking |
| **Lansweeper** | Automated network discovery |
| **Qualys** | Asset discovery + vuln scanning |
| **AWS Config** | Cloud resource inventory |
| **Microsoft Defender** | Endpoint asset visibility |

## 🎤 Interview Angles

### Common Questions
- "Why is asset inventory critical for security?"
- "How would you discover shadow IT in your organization?"
- "What happens if an asset isn't in your inventory during an incident?"

### STAR Story Template
**Situation:** Incident response couldn't identify compromised server because it wasn't documented
**Task:** Implement automated asset discovery to prevent future blind spots
**Action:** Deployed network scanning + CMDB integration + quarterly audits
**Result:** Achieved 99% asset visibility, reduced incident response time by 40%

## 🚨 Common Issues

| Problem | Impact | Solution |
|---------|--------|----------|
| Stale data | Assets exist but aren't tracked | Automated discovery + quarterly audits |
| Missing owner | No accountability during incidents | Enforce ownership field requirement |
| No classification | Can't prioritize protections | Implement tiering (Critical/High/Medium/Low) |
| Shadow IT | Security gaps, compliance risk | DNS monitoring, CASB, policy enforcement |

## ✅ Best Practices

- **Automate discovery** — Manual inventories become stale immediately
- **Single source of truth** — One authoritative CMDB, not scattered spreadsheets
- **Classify assets** — Tag by criticality, data sensitivity, compliance scope
- **Integrate with tools** — Feed inventory into SIEM, vuln scanner, EDR
- **Regular audits** — Quarterly reconciliation between inventory and reality
- **Decommission properly** — Remove retired assets from inventory and network

## ❌ Common Misconceptions

- **"Inventory is IT's job"** — Security owns the risk, must validate completeness
- **"Cloud doesn't need tracking"** — Ephemeral resources still need governance
- **"Discovery tools find everything"** — Manual verification required for accuracy

## 🔗 Related Concepts

- [[Attack Surface Management]] — Asset inventory defines what's exposed
- [[Vulnerability Management]] — Can't patch what you don't know exists
- [[Configuration Management]] — Tracks asset configurations over time
- [[Change Management]] — Asset updates trigger security reviews
- [[Incident Response]] — Inventory accelerates containment and forensics
- [[Compliance]] — Auditors require complete asset documentation

## 📚 References

- NIST SP 800-53: CM-8 (Information System Component Inventory)
- CIS Controls v8: Control 1 (Inventory and Control of Enterprise Assets)
- ISO 27001: A.8.1 (Asset Management)
