---
tags:
  - "#cybersecurity/blue-team/threat-intelligence"
  - "#infrastructure/standards"
  - "#interview/concepts"
aliases:
  - Structured Threat Information Expression
---

# STIX

> **One-liner:** A standardized JSON schema for describing cyber threat intelligence—indicators, relationships, and context—in machine-readable format.

## 🎯 What Is It?

**STIX (Structured Threat Information Expression)** is an open standard developed by OASIS for representing cyber threat intelligence. It enables organizations to share threat data in a consistent, structured format that tools can automatically ingest, correlate, and act upon.

STIX replaces ad-hoc CSV files and unstructured reports with a **graph-based model** where threat objects (IPs, malware, campaigns, threat actors) and their relationships are explicitly defined.

## 🔬 How It Works

### STIX Domain Objects (SDOs)

STIX represents threat intelligence using **objects** connected by **relationships**:

| Object Type | Description | Example |
|-------------|-------------|---------|
| **Indicator** | Observable pattern that suggests malicious activity | IP address, file hash, domain |
| **Malware** | Malicious software instance | BumbleBee, Emotet, CobaltStrike |
| **Threat Actor** | Individual or group conducting attacks | APT29, Lazarus Group |
| **Campaign** | Set of malicious activities over time | "Operation Aurora" |
| **Attack Pattern** | TTP describing how attacks are performed | Spear Phishing (T1566) |
| **Course of Action** | Defensive measures or mitigations | "Block IP at firewall" |
| **Identity** | Individuals, organizations, or sectors targeted | Healthcare sector, FinTech Corp |
| **Vulnerability** | Weakness that can be exploited | CVE-2021-44228 (Log4Shell) |
| **Intrusion Set** | Grouped malicious activities attributed to an actor | APT28 campaign artifacts |

### STIX Relationships

Objects are connected via **Relationship Objects (SROs)**:

```json
{
  "type": "relationship",
  "relationship_type": "uses",
  "source_ref": "threat-actor--[UUID]",
  "target_ref": "malware--[UUID]"
}
```

**Example:** `APT29 → uses → BumbleBee Malware → indicates → IP 45.155.205.3`

### STIX 2.1 Example

```json
{
  "type": "bundle",
  "id": "bundle--[UUID]",
  "objects": [
    {
      "type": "indicator",
      "id": "indicator--[UUID]",
      "created": "2023-07-14T12:00:00.000Z",
      "modified": "2023-07-14T12:00:00.000Z",
      "name": "Malicious IP",
      "pattern": "[ipv4-addr:value = '45.155.205.3']",
      "pattern_type": "stix",
      "valid_from": "2023-07-14T12:00:00.000Z",
      "labels": ["malicious-activity"],
      "indicator_types": ["malicious-activity"]
    },
    {
      "type": "malware",
      "id": "malware--[UUID]",
      "name": "BumbleBee",
      "is_family": true,
      "malware_types": ["backdoor", "loader"]
    },
    {
      "type": "relationship",
      "relationship_type": "indicates",
      "source_ref": "indicator--[UUID]",
      "target_ref": "malware--[UUID]"
    }
  ]
}
```

## 📊 STIX Versions

| Version | Release | Key Features | Status |
|---------|---------|--------------|--------|
| **STIX 1.x** | 2012-2017 | XML-based, complex schema | Deprecated |
| **STIX 2.0** | 2017 | JSON-based, simplified structure | Widely adopted |
| **STIX 2.1** | 2020 | Enhanced relationships, better MITRE ATT&CK support | Current standard |

## 🛡️ How SOC Analysts Use STIX

### Consumption Workflow
1. **Ingest:** Receive STIX bundles via [[TAXII]] server or threat feed
2. **Parse:** CTI platform (MISP, OpenCTI) extracts indicators and relationships
3. **Enrich:** Platform correlates with existing intel, maps to [[MITRE ATT&CK]]
4. **Action:** Firewall rules, SIEM alerts, EDR blocks generated from indicators
5. **Share:** Produce STIX bundles for partner orgs (respecting [[Traffic Light Protocol (TLP)]])

### Tools That Support STIX
- **Threat Intel Platforms:** MISP, OpenCTI, ThreatConnect, Anomali
- **SIEM/SOAR:** Splunk, QRadar, Cortex XSOAR
- **Libraries:** `stix2` (Python), `cti-python-stix2`, `oasis-open/cti-stix2-json-schemas`

### Why STIX Matters for L1 Analysts
- **Automation:** No manual CSV imports—STIX feeds auto-populate platforms
- **Context:** Relationships show *why* an indicator matters (linked to campaign, actor, TTP)
- **Consistency:** All vendors/partners use same schema—no format translation
- **Correlation:** Platforms can auto-link your local IOCs to global STIX campaigns

## ❗ STIX vs Other Formats

| Format | Structure | Use Case | Machine-Readable |
|--------|-----------|----------|------------------|
| **STIX** | JSON graph with relationships | Comprehensive threat intel sharing | ✅ Yes |
| **CSV** | Flat table | Simple IOC lists | ⚠️ Limited |
| **PDF Report** | Unstructured text | Human consumption | ❌ No |
| **OpenIOC** | XML-based | Legacy malware forensics | ⚠️ Limited adoption |
| **YARA** | Rule-based pattern matching | Malware detection | ⚠️ Specialized |

**STIX advantage:** Captures relationships, context, and attribution—not just raw indicators.

## 🎤 Interview Angles

### Common Questions

- **"What is STIX?"**
  - *"STIX is a standardized JSON format for sharing threat intelligence. It represents indicators, malware, threat actors, and their relationships in a machine-readable way so tools can automatically ingest and correlate intel."*

- **"Why use STIX instead of CSV?"**
  - *"CSV is just a flat list of IOCs. STIX captures context—like 'this IP is used by APT29 in Campaign X targeting Healthcare via Spear Phishing.' That context helps analysts prioritize and understand attribution."*

- **"How do you use STIX in the SOC?"**
  - *"We receive STIX bundles via TAXII feeds, which our threat intel platform (like MISP) parses. It extracts indicators, maps them to MITRE ATT&CK techniques, and generates firewall rules or SIEM alerts automatically."*

- **"What's the difference between STIX 2.0 and 2.1?"**
  - *"STIX 2.1 added better support for MITRE ATT&CK mappings, enhanced relationship types, and improved indicator patterns. Both use JSON, but 2.1 is the current standard."*

### STAR Story
> **Situation:** Receiving threat intel from multiple ISACs and vendors—each in different formats (CSV, PDF, custom JSON). Manual ingestion took 3+ hours per feed.
> **Task:** Standardize intel ingestion for faster, automated processing.
> **Action:** Migrated all feeds to STIX 2.1 format via TAXII subscriptions. Configured MISP to auto-parse STIX bundles, extract IOCs, and generate SIEM correlation rules. Created playbook for handling TLP-labeled STIX objects.
> **Result:** Reduced feed ingestion time from 3 hours to 5 minutes. Gained automatic attribution context (threat actor → campaign → TTP). Correlated 40% more IOCs with existing incidents due to relationship mapping.

## ✅ Best Practices

- **Use STIX 2.1** — Latest version with best tooling support
- **Validate bundles** — Use OASIS validators to check schema compliance
- **Preserve relationships** — Don't strip relationship objects when importing
- **Map to ATT&CK** — Use STIX's `attack-pattern` objects to link TTPs
- **Include TLP** — Add [[Traffic Light Protocol (TLP)]] marking objects to all bundles
- **Version control** — Track modifications with `created` and `modified` timestamps

## ❌ Common Misconceptions

- **"STIX is just for big enterprises"** — No; any SOC can use free STIX tools like MISP
- **"STIX replaces human analysis"** — No; it structures data for analysts, doesn't replace judgment
- **"All threat intel comes in STIX"** — No; many feeds still use CSV/PDF; conversion tools exist
- **"STIX 1.x and 2.x are compatible"** — No; completely different schemas (XML vs JSON)

## 🔗 Related Concepts

- [[TAXII]]
- [[Cyber Threat Intelligence (CTI)]]
- [[Indicator of Compromise (IOC)]]
- [[Traffic Light Protocol (TLP)]]
- [[MITRE ATT&CK]]
- [[MISP]]
- [[OpenCTI]]
- [[Threat Intelligence Sharing]]

## 📚 References

- [OASIS STIX 2.1 Specification](https://docs.oasis-open.org/cti/stix/v2.1/stix-v2.1.html)
- [STIX 2.1 GitHub Repository](https://github.com/oasis-open/cti-stix2-json-schemas)
- TryHackMe - Intro to Cyber Threat Intel
- MITRE ATT&CK and STIX Integration
- FIRST.org - STIX Best Practices
