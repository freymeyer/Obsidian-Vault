---
tags:
  - "#cybersecurity/blue-team/threat-intelligence"
  - "#infrastructure/standards"
  - "#interview/concepts"
aliases:
  - Trusted Automated eXchange of Indicator Information
---

# TAXII

> **One-liner:** A set of secure APIs for exchanging cyber threat intelligence in near real-time, enabling automated distribution and collection of [[STIX]] data.

## 🎯 What Is It?

**TAXII (Trusted Automated eXchange of Indicator Information)** is an application protocol designed by OASIS to transport [[STIX]] threat intelligence between organizations and systems. It's the "delivery mechanism" for STIX content.

Think of it this way:
- **[[STIX]]** = The language/format of threat intel
- **TAXII** = The postal service that delivers it

TAXII enables **automated, bidirectional** threat intelligence sharing at scale, replacing manual email attachments and CSV downloads.

## 🔬 How It Works

### TAXII Architecture

TAXII uses **RESTful APIs** over HTTPS to transfer STIX bundles between:
- **Producers:** Organizations generating threat intel
- **Consumers:** Organizations receiving threat intel
- **Servers:** Central repositories hosting STIX collections

### TAXII 2.1 Components

| Component | Purpose | Example |
|-----------|---------|---------|
| **Server** | Hosts threat intel collections | CISA AIS TAXII Server |
| **API Root** | Base URL for accessing collections | `https://taxii.example.com/api/` |
| **Collection** | Logical grouping of STIX objects | "Ransomware IOCs", "APT Campaigns" |
| **Discovery Endpoint** | Lists available API roots | `/taxii2/` |
| **Collection Endpoint** | Lists objects in a collection | `/api/collections/{id}/objects/` |

## 📊 TAXII Sharing Models

### 1. Collection (Hub Model)
A **producer hosts** threat intel; consumers pull on-demand.

```
Producer → TAXII Server (Collection)
              ↓
         Consumer A pulls
         Consumer B pulls
         Consumer C pulls
```

**Use Case:** Threat intel vendor distributing IOC feeds to subscribers.

**Workflow:**
1. Consumer authenticates to TAXII server
2. Consumer queries `/collections/` to list available data
3. Consumer requests `/collections/{id}/objects/` to retrieve STIX bundles
4. Consumer polls periodically for updates

### 2. Channel (Pub/Sub Model)
A **central server publishes** intel; consumers subscribe for push notifications.

```
Producer A → TAXII Server (Channel) → Push to Subscriber 1
Producer B ↗                        ↘ Push to Subscriber 2
```

**Use Case:** ISAC broadcasting urgent IOCs to all members simultaneously.

**Workflow:**
1. Subscribers register with channel
2. Producers push STIX bundles to server
3. Server distributes to all subscribers in near real-time

## 🛡️ How SOC Analysts Use TAXII

### Consumer Workflow (Receiving Intel)
```bash
# 1. Discover available collections
curl -X GET https://taxii.example.com/taxii2/ \
  -H "Authorization: Bearer $TOKEN"

# 2. List STIX objects in collection
curl -X GET https://taxii.example.com/api/collections/{id}/objects/ \
  -H "Authorization: Bearer $TOKEN"

# 3. Retrieve specific indicator
curl -X GET https://taxii.example.com/api/collections/{id}/objects/{object-id}/ \
  -H "Authorization: Bearer $TOKEN"
```

### Producer Workflow (Sharing Intel)
```bash
# Push STIX bundle to TAXII server
curl -X POST https://taxii.example.com/api/collections/{id}/objects/ \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/taxii+json;version=2.1" \
  -d @stix_bundle.json
```

### Automated Integration
Most CTI platforms (MISP, OpenCTI) provide **TAXII connectors**:
1. Configure TAXII endpoint URL and API key
2. Select collections to subscribe
3. Set polling interval (e.g., every 15 minutes)
4. Platform auto-ingests new STIX bundles

## 🔐 Security Features

| Feature | Purpose |
|---------|---------|
| **HTTPS/TLS** | Encrypted transport |
| **API Keys / OAuth 2.0** | Authentication and authorization |
| **[[Traffic Light Protocol (TLP)]]** | Embedded in STIX objects to control sharing |
| **Role-Based Access** | Different permissions per collection |
| **Audit Logs** | Track who accessed what intel |

## 📊 TAXII Versions

| Version | Release | Protocol | Status |
|---------|---------|----------|--------|
| **TAXII 1.x** | 2014 | SOAP/XML | Deprecated |
| **TAXII 2.0** | 2017 | REST/JSON | Supported |
| **TAXII 2.1** | 2020 | REST/JSON | Current standard |

**Migration Note:** TAXII 2.x is **not backward compatible** with 1.x—different APIs and transport.

## 🎤 Interview Angles

### Common Questions

- **"What is TAXII?"**
  - *"TAXII is a secure API standard for exchanging STIX threat intelligence. It's the transport layer—like a postal service—that delivers threat intel between organizations in near real-time."*

- **"What's the difference between STIX and TAXII?"**
  - *"STIX is the format/language of threat intel—what the data looks like. TAXII is the protocol/delivery mechanism—how that data moves between systems. You need both: STIX defines the content, TAXII delivers it."*

- **"What are TAXII sharing models?"**
  - *"Collection model: Consumers pull intel from a server on-demand—like visiting a library. Channel model: Server pushes intel to subscribers automatically—like a news alert. Collection is for feeds, Channel is for urgent broadcasts."*

- **"How do SOC teams use TAXII?"**
  - *"We subscribe to TAXII feeds from vendors and ISACs. Our threat intel platform polls the TAXII server every 15 minutes, pulls new STIX bundles, extracts IOCs, and auto-generates firewall blocks or SIEM alerts."*

### STAR Story
> **Situation:** Manually downloading daily IOC CSVs from 5 different vendors—taking 90+ minutes each morning and missing time-sensitive intel.
> **Task:** Automate threat feed ingestion for faster, reliable updates.
> **Action:** Migrated all vendors to TAXII 2.1 subscriptions. Configured MISP to poll TAXII endpoints every 15 minutes via API keys. Set up TLP-based access controls to ensure AMBER-labeled intel stayed internal. Created alerting for failed TAXII polls.
> **Result:** Reduced feed update time from 90 minutes to <1 minute (fully automated). Captured 3 critical IOCs within 10 minutes of vendor publication—blocked lateral movement from active ransomware campaign. Zero missed updates due to automation.

## ✅ Best Practices

- **Use TAXII 2.1** — Current standard with best tooling support
- **Authenticate all connections** — Never expose TAXII endpoints without auth
- **Poll responsibly** — Don't hammer servers; 15-30 min intervals are typical
- **Validate STIX bundles** — Check schema compliance before ingestion
- **Respect [[Traffic Light Protocol (TLP)]]** — Don't re-share AMBER/RED intel via TAXII
- **Monitor for failures** — Alert on TAXII connection drops or stale data
- **Use collections strategically** — Separate high-priority IOCs from bulk feeds

## ❌ Common Misconceptions

- **"TAXII is only for big enterprises"** — No; free TAXII servers (like CISA AIS) are open to all
- **"TAXII requires special hardware"** — No; it's REST APIs over HTTPS—works with any HTTP client
- **"TAXII 1.x and 2.x are compatible"** — No; completely different protocols (SOAP/XML vs REST/JSON)
- **"You can use TAXII without STIX"** — Technically yes, but STIX is the standard payload

## 🔗 Related Concepts

- [[STIX]]
- [[Cyber Threat Intelligence (CTI)]]
- [[Indicator of Compromise (IOC)]]
- [[Traffic Light Protocol (TLP)]]
- [[MISP]]
- [[OpenCTI]]
- [[Threat Intelligence Sharing]]
- [[API]]
- [[RESTful API]]

## 📚 References

- [OASIS TAXII 2.1 Specification](https://docs.oasis-open.org/cti/taxii/v2.1/taxii-v2.1.html)
- [CISA AIS TAXII Server](https://www.cisa.gov/ais)
- TryHackMe - Intro to Cyber Threat Intel
- [MISP TAXII Integration Guide](https://www.misp-project.org/2019/09/12/MISP-2.4.116-released.html)
- [Anomali TAXII Best Practices](https://www.anomali.com/resources/taxii)
