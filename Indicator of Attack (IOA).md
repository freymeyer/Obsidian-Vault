---
tags:
  - "#cybersecurity/blue-team/threat-intelligence"
  - "#cybersecurity/blue-team/detection-engineering"
  - "#interview/concepts"
aliases:
  - IOA
  - Indicators of Attack
---

# Indicator of Attack (IOA)

> **One-liner:** Real-time evidence of malicious behavior or techniques being executed, such as PowerShell process injection or suspicious lateral movement—detectable before full compromise.

## 🎯 What Is It?

An **Indicator of Attack (IOA)** is a behavioral pattern or technique that signals an attack **in progress**. Unlike [[Indicator of Compromise (IOC)]]s that show evidence a breach already happened, IOAs detect adversary **actions** as they occur.

IOAs are **proactive** in nature: they identify attacks earlier in the [[Cyber Kill Chain]], often before persistence is established or data is stolen.

## 🔬 How IOAs Work

### Behavior Over Artifacts
IOAs focus on **what attackers do**, not just what they leave behind:
- Process execution patterns (e.g., `cmd.exe` spawning from `winword.exe`)
- Network anomalies (e.g., unusual DNS queries, beaconing patterns)
- Authentication anomalies (e.g., impossible travel, brute-force attempts)
- System changes (e.g., disabling antivirus, creating scheduled tasks)

### Detection Methods
IOAs are detected through:
- **EDR behavioral analytics** — Monitor process trees, API calls, memory injection
- **SIEM correlation rules** — Multiple low-level events = high-confidence IOA
- **UEBA (User & Entity Behavior Analytics)** — Detect deviations from baseline behavior
- **Endpoint telemetry** — Sysmon, Auditd, Windows Event Logs

## 📊 IOA Categories & Examples

| Category | Example IOA | [[MITRE ATT&CK]] Mapping |
|----------|-------------|---------------------------|
| **Execution** | PowerShell spawning encoded commands | T1059.001 PowerShell |
| **Persistence** | Registry Run key created by suspicious process | T1547.001 Registry Run Keys |
| **Privilege Escalation** | Token impersonation via `SeDebugPrivilege` | T1134 Access Token Manipulation |
| **Defense Evasion** | Process hollowing, disabling Windows Defender | T1055 Process Injection, T1562.001 Disable Tools |
| **Credential Access** | LSASS memory dump, mimikatz execution | T1003.001 LSASS Memory |
| **Lateral Movement** | PsExec, WMI remote process creation | T1021.002 SMB/Windows Admin Shares |
| **Command & Control** | HTTP beaconing every 60 seconds | T1071.001 Web Protocols |
| **Exfiltration** | Large data transfer to external IP | T1041 Exfiltration Over C2 Channel |

## 🛡️ Detection & Prevention

### How to Detect IOAs

**Behavioral Detection Rules**
```yaml
# Example Sigma Rule for Suspicious PowerShell
title: Encoded PowerShell Command
status: stable
logsource:
  product: windows
  service: powershell
detection:
  selection:
    EventID: 4104
    ScriptBlockText|contains:
      - '-enc'
      - '-encodedcommand'
      - 'FromBase64String'
  condition: selection
fields:
  - ComputerName
  - User
  - ScriptBlockText
falsepositives:
  - Legitimate admin scripts (context required)
level: high
```

**EDR Telemetry Monitoring**
- **Process Relationships:** `winword.exe → powershell.exe → net.exe`
- **Network Anomalies:** Unusual DNS TXT record queries (potential DNS tunneling)
- **File Activity:** Executables written to `%TEMP%` then launched
- **Authentication:** Multiple failed logins followed by success (brute-force)

### Prevention Strategies
- **Application Whitelisting:** Block unauthorized executables
- **Least Privilege:** Limit user permissions to reduce attack surface
- **EDR Deployment:** Real-time behavioral monitoring and automated response
- **Network Segmentation:** Limit lateral movement opportunities
- **Multi-Factor Authentication:** Prevent credential-based attacks

## ❗ IOA vs IOC

| Aspect | **IOA (Indicator of Attack)** | **IOC (Indicator of Compromise)** |
|--------|-------------------------------|-----------------------------------|
| **Detection Timing** | **During** the attack | **After** compromise |
| **Nature** | Behavioral/dynamic | Artifact-based/static |
| **Focus** | Attacker actions (TTPs) | Attacker artifacts (IPs, hashes) |
| **Examples** | PowerShell process injection, lateral movement | Malware hash, C2 IP address |
| **Detection Method** | Behavioral analytics, UEBA | Signature matching, threat feeds |
| **Kill Chain Phase** | Early (Execution, Persistence) | Late (Installation, C2) |
| **Response** | "Stop the attack now" | "Investigate the breach" |

**Key Insight:** IOAs enable **prevention**; IOCs enable **response**.

## 🎤 Interview Angles

### Common Questions

- **"What is an IOA?"**
  - *"An Indicator of Attack is behavioral evidence that an attack is actively happening—like PowerShell spawning from Microsoft Word or multiple failed login attempts. Unlike IOCs that show a breach occurred, IOAs catch attacks in progress."*

- **"Give examples of IOAs."**
  - *"Process injection, credential dumping via LSASS, lateral movement with PsExec, DNS tunneling, unusual network beaconing—anything showing malicious behavior rather than just malicious artifacts."*

- **"How do IOAs improve detection?"**
  - *"IOAs detect attacks earlier in the kill chain before damage is done. Instead of waiting to find a malware hash (IOC), we see the suspicious behavior when PowerShell is invoked abnormally, giving us time to block before persistence or exfil."*

- **"What's the difference between IOA and IOC?"**
  - *"IOC = Evidence left behind (hash, IP). IOA = Malicious action happening (process injection, lateral movement). IOAs are proactive; IOCs are reactive."*

### STAR Story
> **Situation:** EDR alert triggered on workstation: `WINWORD.EXE` spawned `POWERSHELL.EXE` with encoded command.
> **Task:** Determine if this was legitimate user activity or an active attack.
> **Action:** Analyzed process tree—Word document from external email triggered malicious macro. Decoded PowerShell command: download and execute payload from attacker-controlled domain. Recognized as IOA for T1059.001 (PowerShell) and T1204.002 (Malicious File Execution). Immediately isolated host, blocked domain at firewall.
> **Result:** Stopped attack during initial execution phase—before persistence or lateral movement. Found phishing email, added sender to blocklist. Prevented potential ransomware deployment across 50+ endpoints.

## ✅ Best Practices

- **Prioritize behavioral detections** — IOAs catch attacks IOCs miss
- **Map IOAs to [[MITRE ATT&CK]]** — Understand which techniques you can detect
- **Tune EDR rules** — Balance detection sensitivity vs false positive rate
- **Combine IOAs + IOCs** — Use both for defense-in-depth
- **Hunt for IOAs proactively** — Don't wait for alerts; search logs for known TTPs
- **Measure coverage** — Track what percentage of ATT&CK techniques you can detect

## ❌ Common Misconceptions

- **"IOAs are just advanced IOCs"** — No; fundamentally different—behavior vs artifacts
- **"IOAs eliminate false positives"** — No; behavioral detection still needs tuning
- **"Only EDR can detect IOAs"** — No; SIEM correlation rules, Sysmon, and network monitoring also work
- **"IOAs replace IOCs"** — No; use both together for comprehensive detection

## 🔗 Related Concepts

- [[Indicator of Compromise (IOC)]]
- [[Tactics, Techniques, and Procedures (TTP)]]
- [[Cyber Threat Intelligence (CTI)]]
- [[MITRE ATT&CK]]
- [[Detection Engineering]]
- [[EDR]]
- [[SIEM]]
- [[Threat Hunting]]
- [[Cyber Kill Chain]]
- [[Behavioral Detection]]

## 📚 References

- TryHackMe - Intro to Cyber Threat Intel
- MITRE ATT&CK Framework
- Endgame (now Elastic) - Indicators of Attack Whitepaper
- SANS: Detecting the Unknown with IOAs
