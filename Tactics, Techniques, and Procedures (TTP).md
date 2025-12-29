---
tags:
  - "#cybersecurity/blue-team/threat-intelligence"
  - "#cybersecurity/red-team"
  - "#interview/concepts"
aliases:
  - TTPs
  - Tactics Techniques Procedures
---

# Tactics, Techniques, and Procedures (TTP)

> **One-liner:** The detailed behavioral patterns and methodologies adversaries use to conduct attacks—mapped in [[MITRE ATT&CK]] for consistent threat analysis.

## 🎯 What Is It?

**TTPs (Tactics, Techniques, and Procedures)** describe *how* adversaries operate at three levels of granularity:

| Level | Definition | Example |
|-------|------------|---------|
| **Tactic** | The adversary's goal/objective | Initial Access, Persistence, Exfiltration |
| **Technique** | *How* the tactic is achieved | Spear Phishing, Registry Run Keys, DNS Tunneling |
| **Procedure** | Specific implementation details | APT29 uses spear phishing with `.lnk` files disguised as PDFs |

TTPs provide the **behavioral fingerprint** of threat actors, enabling defenders to:
- Identify attack campaigns
- Attribute incidents to known groups
- Build detections that survive IOC rotation

## 🔬 TTP Hierarchy in MITRE ATT&CK

### Example: Credential Access Tactic

```
Tactic: Credential Access (TA0006)
  └─ Technique: OS Credential Dumping (T1003)
      ├─ Sub-Technique: LSASS Memory (T1003.001)
      │   └─ Procedure: Mimikatz via `sekurlsa::logonpasswords`
      ├─ Sub-Technique: Security Account Manager (T1003.002)
      │   └─ Procedure: Invoke-NinjaCopy to extract SAM hive
      └─ Sub-Technique: NTDS.DIT (T1003.003)
          └─ Procedure: DCSync via Impacket's `secretsdump.py`
```

### Real-World TTP Mapping

**Scenario:** BumbleBee malware campaign

| TTP Component | Details | ATT&CK ID |
|---------------|---------|-----------|
| **Initial Access** | Spear phishing attachment | T1566.001 |
| **Execution** | User opens malicious document | T1204.002 |
| **Persistence** | Registry Run key created | T1547.001 |
| **Defense Evasion** | Process hollowing via `svchost.exe` | T1055.012 |
| **Command & Control** | HTTPS beaconing to C2 | T1071.001 |
| **Exfiltration** | Data compressed before transfer | T1560.001 |

## 📊 Why TTPs Matter More Than IOCs

### IOC vs TTP Longevity

| Indicator Type | Lifespan | Example | Attacker Response |
|----------------|----------|---------|-------------------|
| **[[Indicator of Compromise (IOC)]]** | Hours to days | IP: `45.155.205.3` | Spin up new infrastructure |
| **TTP** | Months to years | T1003.001 LSASS dump | Requires retooling, retraining |

**Key Insight:** Adversaries can change IOCs instantly (new IP, new hash), but changing TTPs requires:
- Developing new exploits
- Retraining operators
- Rebuilding tooling

**Defensive Value:** TTP-based detection is more durable than IOC-based detection.

### Detection Pyramid

```
        IOCs (Least durable)
       /    \
      /      \
     /  Tools \
    /    &     \
   / Techniques \
  /__TTPs_______\ (Most durable)
```

## 🛡️ Detection & Prevention

### TTP-Based Detection Strategies

**1. Behavioral Detection Rules**
Instead of blocking IP `1.2.3.4`, detect the TTP:
```yaml
# Sigma Rule: T1003.001 LSASS Memory Dump
detection:
  selection:
    EventID: 10
    TargetImage|endswith: '\lsass.exe'
    GrantedAccess: '0x1410'
  condition: selection
```

**2. UEBA (User & Entity Behavior Analytics)**
Detect TTPs via anomaly detection:
- Normal: User logs in from Office network
- Anomaly (T1078 Valid Accounts): User logs in from foreign IP at 3 AM

**3. Threat Hunting**
Proactively search for TTPs:
- Query: "Show all processes accessing `lsass.exe` memory in last 30 days"
- Result: Identifies T1003.001 attempts before alerts fire

### Mapping TTPs to Mitigations

| TTP | ATT&CK ID | [[MITRE D3FEND]] Mitigation | Implementation |
|-----|-----------|------------------------------|----------------|
| PowerShell Execution | T1059.001 | D3-PSA Process Spawn Analysis | Sysmon EventID 1, EDR |
| LSASS Memory Dump | T1003.001 | D3-PSA Process Spawn Analysis | Protected Process Light for LSASS |
| DNS Tunneling | T1048.003 | D3-DNSTA DNS Traffic Analysis | DNS query length monitoring |

## 🎤 Interview Angles

### Common Questions

- **"What are TTPs?"**
  - *"TTPs describe how adversaries operate: Tactics are their goals (like Credential Access), Techniques are methods (like dumping LSASS), and Procedures are specific implementations (like using Mimikatz). TTPs give the behavioral fingerprint of attackers."*

- **"Why are TTPs more valuable than IOCs?"**
  - *"IOCs like IPs and hashes change quickly—attackers can spin up new infrastructure in minutes. TTPs persist for months or years because changing them requires retooling and retraining. Detecting TTPs builds more durable defenses."*

- **"How do you use TTPs in the SOC?"**
  - *"We map every incident to MITRE ATT&CK techniques. This helps us identify attack patterns, attribute campaigns to known threat actors, and build behavioral detections that survive IOC rotation. It also guides our threat hunting priorities."*

- **"Give an example of TTP-based detection."**
  - *"Instead of blocking a specific malware hash, we detect T1003.001 (LSASS memory access). Even if attackers change their credential dumping tool, the behavior—accessing LSASS memory with certain permissions—stays the same, so our detection still works."*

### STAR Story
> **Situation:** Recurring phishing campaigns targeting our organization—each used different domains/IPs, so IOC-based blocking wasn't effective.
> **Task:** Develop a detection strategy that survives IOC rotation.
> **Action:** Mapped observed behaviors to MITRE ATT&CK: T1566.001 (Spear Phishing Attachment) → T1204.002 (Malicious File Execution) → T1059.001 (PowerShell). Created behavioral detections: Office processes spawning PowerShell with encoded commands, regardless of email sender or file hash.
> **Result:** Detected 8 subsequent phishing attempts despite completely different IOCs (new domains, file names, hashes). Reduced phishing success rate by 95%. Shifted security posture from reactive (IOC blocking) to proactive (TTP detection).

## ✅ Best Practices

- **Map incidents to ATT&CK** — Document TTPs for every investigation
- **Build TTP-based detections** — Prioritize behavioral rules over signature-based
- **Track adversary TTPs** — Maintain profiles of threat actors targeting your industry
- **Use TTP attribution** — Link observed techniques to known campaigns for context
- **Hunt for TTPs proactively** — Don't wait for alerts; search logs for known techniques
- **Combine with IOCs** — Use both: IOCs for quick wins, TTPs for durability
- **Measure TTP coverage** — Track what % of relevant ATT&CK techniques you can detect

## ❌ Common Misconceptions

- **"TTPs are the same as IOCs"** — No; IOCs are artifacts, TTPs are behaviors
- **"TTPs never change"** — No; threat actors evolve, but more slowly than IOCs
- **"TTP detection has no false positives"** — No; legitimate admin tools use same techniques (requires tuning)
- **"You only need TTP detection"** — No; IOCs still provide fast blocking; use both

## 🎯 TTP Example: APT29 (Cozy Bear)

| TTP Component | Details | ATT&CK ID |
|---------------|---------|-----------|
| **Initial Access** | Spear phishing via compromised cloud accounts | T1078.004 |
| **Execution** | WMI for remote execution | T1047 |
| **Persistence** | Scheduled tasks, service creation | T1053.005 |
| **Privilege Escalation** | Exploiting CVE-2020-0688 (Exchange) | T1068 |
| **Defense Evasion** | Masquerading legitimate tools (WinRAR, 7-Zip) | T1036 |
| **Credential Access** | DCSync attack to dump domain credentials | T1003.006 |
| **Discovery** | Active Directory enumeration via PowerView | T1087.002 |
| **Lateral Movement** | Pass-the-ticket with Kerberos tickets | T1550.003 |
| **Collection** | Archiving files before exfiltration | T1560.001 |
| **Exfiltration** | HTTPS to attacker-controlled cloud storage | T1567.002 |

## 🔗 Related Concepts

- [[MITRE ATT&CK]]
- [[MITRE D3FEND]]
- [[Indicator of Compromise (IOC)]]
- [[Indicator of Attack (IOA)]]
- [[Cyber Threat Intelligence (CTI)]]
- [[Threat Hunting]]
- [[Detection Engineering]]
- [[Behavioral Detection]]
- [[Cyber Kill Chain]]

## 📚 References

- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- TryHackMe - Intro to Cyber Threat Intel
- MITRE: Getting Started with ATT&CK
- SANS: TTP-Based Threat Intelligence
- Pyramid of Pain: TTP vs IOC Analysis
