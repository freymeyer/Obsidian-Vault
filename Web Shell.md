---
tags:
  - "#cybersecurity/malware"
  - "#cybersecurity/red-team/post-exploitation"
  - "#interview/concepts"
aliases:
  - Webshell
  - Web Shells
---

# Web Shell

> **One-liner:** A malicious script uploaded to a web server that provides attackers with remote command execution via a web interface.

## 🎯 What Is It?
A Web Shell is a script (typically PHP, ASP, JSP, or Python) installed on a compromised web server that allows attackers to execute operating system commands through a web browser. Web shells are used in the **Installation** stage of the [[Cyber Kill Chain]] to maintain [[Persistence (Cyber Security)|persistent]] access after initial exploitation.

## 🔬 How It Works

```
Attacker                 Compromised Server              Target System
   │                            │                              │
   ├──Exploits vulnerability───►│                              │
   │  (upload vuln, RCE, etc.)  │                              │
   │                            │                              │
   ├──Uploads web shell────────►│                              │
   │                            ├──Shell saved to webroot──────│
   │                            │                              │
   ├──Accesses via browser─────►│                              │
   │  (https://target/shell.php)│                              │
   │                            │                              │
   ├──Sends commands───────────►├──Executes on server─────────►│
   │                            │                              │
   │◄──Receives output──────────┤                              │
```

### Simple PHP Web Shell Example
```php
<?php
// Simple one-liner web shell
system($_GET['cmd']);
?>
```
```
# Usage: https://target.com/shell.php?cmd=whoami
```

### Advanced Web Shell Features
- File manager (upload, download, edit files)
- Database access
- Reverse shell spawning
- Password-protected access
- Encoding/obfuscation to evade detection

## 📊 Common Web Shells

| Name | Language | Features |
|------|----------|----------|
| **c99** | PHP | Full-featured file manager, DB access |
| **r57** | PHP | System info, file manager |
| **China Chopper** | ASP/PHP | 4KB, highly obfuscated |
| **WSO** | PHP | Web Shell by oRb |
| **Weevely** | PHP | Encrypted communications |
| **ASPXSpy** | ASPX | .NET web shell |

## 🛡️ Detection & Prevention

### How to Detect
- **File integrity monitoring** - Detect new/modified files in webroot
- **Web server log analysis** - Unusual POST requests, cmd parameters
- **Process monitoring** - Web server spawning shells (bash, cmd.exe)
- **Network monitoring** - Unusual outbound traffic from web servers
- **YARA rules** - Signature-based detection of known shells
- **Behavioral analysis** - Detect `system()`, `exec()`, `eval()` usage

### Indicators of Compromise
```
# Suspicious log entries
POST /uploads/image.php HTTP/1.1 (with unusual parameters)
GET /shell.php?cmd=cat+/etc/passwd HTTP/1.1

# Suspicious processes
www-data spawning /bin/bash
IIS worker spawning cmd.exe
```

### How to Prevent / Mitigate
- Validate and sanitize all file uploads
- Disable dangerous PHP functions (`system`, `exec`, `shell_exec`)
- Restrict webroot permissions (no write access where possible)
- Web Application Firewall rules for web shell patterns
- Application allowlisting on web servers
- Regular security scanning of web directories

## 🎤 Interview Angles

### Common Questions
- "What is a web shell and why is it dangerous?"
- "How would you detect web shell activity?"
- "What makes web shells difficult to detect?"

### STAR Story
> **Situation:** Detected unusual outbound connections from a web server during routine monitoring.
> **Task:** Investigate and remediate potential compromise.
> **Action:** Analyzed web logs, found suspicious POST requests to an unknown PHP file. Discovered a c99 web shell uploaded via a file upload vulnerability. Contained the server, removed the shell, patched the vulnerability.
> **Result:** Prevented data exfiltration. Implemented file integrity monitoring and upload validation to prevent recurrence.

## ✅ Best Practices
- Never trust user-uploaded content
- Run web servers with minimal privileges
- Implement defense in depth (WAF + FIM + EDR)
- Regular web application security assessments
- Monitor for process anomalies on web servers

## 🔗 Related Concepts
- [[Cyber Kill Chain]]
- [[Persistence (Cyber Security)]]
- [[Remote Code Execution (RCE)]]
- [[Command and Control (C2)]]
- [[Living off the Land (LOLBAS)]]

## 📚 References
- MITRE ATT&CK - Server Software Component: Web Shell (T1505.003)
- CISA Web Shell Analysis
