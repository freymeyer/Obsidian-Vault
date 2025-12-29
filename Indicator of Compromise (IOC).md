---
tags:
  - "#cybersecurity/blue-team/threat-intelligence"
  - "#cybersecurity/blue-team/incident-response"
  - "#interview/concepts"
aliases:
  - IOC
  - IoC
  - Indicators of Compromise
---

# Indicator of Compromise (IOC)

> **One-liner:** Evidence that a security breach has already occurred, such as a known-malicious IP, file hash, or registry key found in your environment.

## 🎯 What Is It?

An **Indicator of Compromise (IOC)** is a forensic artifact or observable that suggests a system has been breached or compromised. IOCs are the "breadcrumbs" attackers leave behind—they answer **"Did they get in?"**

IOCs are **reactive** in nature: they indicate past or ongoing compromise. Finding an IOC in your logs means an attack succeeded or is underway.

## 📊 Common IOC Types

| IOC Type | Description | Example | Detection Method |
|----------|-------------|---------|------------------|
| **IP Address** | Known malicious or C2 server IP | `45.155.205.3` | Firewall logs, SIEM correlation |
| **Domain Name** | Malicious or phishing domain | `evil-updates[.]net` | DNS logs, passive DNS lookups |
| **URL** | Specific malicious endpoint | `hxxp://evil[.]net/payload.exe` | Proxy logs, URL reputation |
| **File Hash** | MD5/SHA1/SHA256 of malware | `e99a18c428cb38d5f48269...` | EDR, VirusTotal, file scanning |
| **Email Address** | Phishing sender address | `billing@fakecorp[.]com` | Email gateway, DMARC failures |
| **Registry Key** | Persistence mechanism | `HKCU\Software\Run\evil.exe` | Registry monitoring, Sysmon |
| **Mutex** | Malware synchronization object | `Global\BumbleBee2023` | Process monitoring, memory forensics |
| **User-Agent** | Malicious tool fingerprint | `Mozilla/4.0 (compatible; Cobalt Strike)` | HTTP logs, web server analysis |
| **Certificate Hash** | SSL cert used by C2 | SHA-1 thumbprint | TLS inspection, certificate logs |

## 🔬 How IOCs Work

### IOC Lifecycle
1. **Discovery:** Threat intel feeds, incident investigations, or malware analysis reveal new IOC
2. **Distribution:** IOC shared via [[STIX]]/[[TAXII]], threat platforms, or ISACs
3. **Ingestion:** SOC imports IOC into SIEM, EDR, firewall, or threat intel platform
4. **Detection:** Security tools alert when IOC matches logs/traffic/files
5. **Response:** Analyst investigates alert, confirms compromise, escalates to IR

### IOC Enrichment
Raw IOCs need context before action. Enrichment answers:
- **Who owns it?** WHOIS lookup for IPs/domains
- **Is it malicious?** VirusTotal, AbuseIPDB, URLhaus reputation
- **When was it seen?** First/last seen dates, campaign associations
- **What does it do?** Sandbox reports (Any.Run, Hybrid-Analysis)
- **What's the confidence?** How many sources report it? Any false positives?

## 🛡️ Detection & Prevention

### How to Detect IOCs
- **Threat Intel Feeds:** Ingest IOC feeds into SIEM/firewall for automated blocking
- **Log Correlation:** Search historical logs for known IOCs retroactively
- **EDR Scanning:** Hash-based file scanning across endpoints
- **DNS Sinkholing:** Redirect malicious domain queries to sinkhole for detection
- **Threat Hunting:** Proactively search for IOCs before they trigger alerts

### Tools for IOC Management
- **Threat Intel Platforms:** [[MISP]], OpenCTI, ThreatConnect
- **Enrichment APIs:** VirusTotal, AbuseIPDB, Shodan, PassiveTotal
- **Detection:** SIEM rules, YARA rules, Snort/Suricata IDS signatures

### IOC Confidence Grading

| Confidence | Criteria | Action |
|------------|----------|--------|
| **High** | 2+ trusted sources + local sighting | Immediate block/isolate |
| **Medium** | Single trusted source, no local hits | Alert-only, monitor |
| **Low** | OSINT only, no corroboration | Watchlist for 14 days |

## ❗ IOC vs IOA

| Aspect | **IOC (Indicator of Compromise)** | **IOA (Indicator of Attack)** |
|--------|-----------------------------------|-------------------------------|
| **Timing** | Evidence of **past/current** breach | Evidence of **ongoing** attack |
| **Focus** | Artifacts left behind | Adversary actions |
| **Examples** | Malware hash, C2 IP, registry key | PowerShell process injection, lateral movement |
| **Detection** | Signature-based (static) | Behavior-based (dynamic) |
| **Response** | "They were here" → Investigate scope | "They are here now" → Stop the action |

**Simple rule:** IOC = **What was left** | IOA = **What is happening**

## 🎤 Interview Angles

### Common Questions
- **"What is an IOC?"**
  - *"An Indicator of Compromise is forensic evidence that a breach occurred—like finding a known malware hash on a system or seeing C2 traffic in logs. It's reactive: it confirms an attack happened."*

- **"Give examples of IOCs."**
  - *"IPs of known C2 servers, file hashes of malware, malicious domains, phishing email addresses, persistence registry keys—anything that indicates attacker presence."*

- **"How do you use IOCs in the SOC?"**
  - *"We ingest IOCs from threat feeds into our SIEM and firewall. When an IOC matches logs or traffic, we get an alert. I enrich it to confirm it's a true positive, then escalate to incident response if validated."*

- **"What's the difference between IOC and IOA?"**
  - *"IOC is evidence something bad already happened—like a malware hash. IOA is observing malicious behavior in real-time—like PowerShell spawning suspicious processes. IOAs detect attacks earlier in the [[Cyber Kill Chain]]."*

### STAR Story
> **Situation:** SIEM alert triggered on outbound connection to IP `91.185.23.222` from a workstation.
> **Task:** Determine if this was a false positive or genuine C2 communication.
> **Action:** Enriched IP via VirusTotal—confirmed high-confidence C2 for BumbleBee malware. Searched logs for file hash `flbpfuh.exe`, matched to known BumbleBee dropper. Found registry persistence key and phishing email vector.
> **Result:** Escalated as confirmed compromise with full IOC chain (email → hash → IP). IR isolated host within 10 minutes. Post-incident, added all IOCs to blocklist, preventing 3 additional infections from same campaign.

## ✅ Best Practices

- **Always enrich IOCs** — Never act on raw indicators without validation
- **Track IOC sources** — Document where each IOC came from for legal/audit trail
- **Age out stale IOCs** — Remove indicators after 90-180 days to reduce false positives
- **Combine IOCs with TTPs** — Context matters; correlate IOCs with [[MITRE ATT&CK]] techniques
- **Test before blocking** — Start new IOC feeds in alert-only mode, then promote to block
- **Respect [[Traffic Light Protocol (TLP)]]** — Don't share IOCs beyond authorized boundaries

## ❌ Common Misconceptions

- **"All IOCs are equally reliable"** — No; OSINT IOCs need more validation than commercial feeds
- **"IOCs detect attacks early"** — No; IOCs are evidence of compromise. [[Indicator of Attack (IOA)]]s detect earlier
- **"More IOCs = better security"** — No; over-ingesting feeds causes alert fatigue and false positives
- **"IOCs never change"** — No; IPs/domains get reassigned; validate IOC age and context

## 🔗 Related Concepts

- [[Indicator of Attack (IOA)]]
- [[Tactics, Techniques, and Procedures (TTP)]]
- [[Cyber Threat Intelligence (CTI)]]
- [[Traffic Light Protocol (TLP)]]
- [[STIX]]
- [[MITRE ATT&CK]]
- [[Threat Hunting]]
- [[SIEM]]
- [[EDR]]
- [[Cyber Kill Chain]]

## 📚 References

- TryHackMe - Intro to Cyber Threat Intel
- NIST SP 800-150: Guide to Cyber Threat Information Sharing
- MITRE ATT&CK Framework
- FIRST.org - TLP Standards
