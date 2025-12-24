---
tags:
  - "#cybersecurity/attacks/post-exploitation"
  - "#cybersecurity/malware"
  - "#interview/concepts"
aliases:
  - Persistence Mechanisms
  - Maintaining Access
---

# Persistence (Cyber Security)

> **One-liner:** Techniques attackers use to maintain access to a compromised system across reboots, credential changes, and remediation attempts.

## 🎯 What Is It?
Persistence is the **Installation** stage of the [[Cyber Kill Chain]], where attackers ensure they can return to a compromised system without re-exploiting it. The goal is to survive reboots, user logoffs, and even some remediation efforts.

## 🔬 How It Works

### Persistence Categories

| Category | Description | Examples |
|----------|-------------|----------|
| **Boot/Logon** | Execute on system start or user login | Registry Run keys, Scheduled Tasks, Startup folder |
| **Account** | Maintain valid credentials | Create accounts, SSH keys, golden tickets |
| **Implant** | Install persistent malware | [[Web Shell]], rootkits, backdoors |
| **Hijacking** | Modify legitimate processes | DLL hijacking, service replacement |

## 📊 Common Techniques

### Windows Persistence

```powershell
# Registry Run Keys
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v Backdoor /t REG_SZ /d "C:\malware.exe"

# Scheduled Task
schtasks /create /tn "UpdateCheck" /tr "C:\malware.exe" /sc onlogon

# Service Creation
sc create backdoorsvc binpath= "C:\malware.exe" start= auto

# WMI Event Subscription
# Executes payload when specific event occurs
```

| Technique | Registry/Location | Detection |
|-----------|-------------------|-----------|
| Run Keys | `HKCU\...\Run`, `HKLM\...\Run` | Monitor registry changes |
| Scheduled Tasks | `C:\Windows\System32\Tasks` | Task Scheduler logs |
| Services | `HKLM\SYSTEM\...\Services` | New service creation |
| Startup Folder | `%APPDATA%\...\Startup` | File system monitoring |
| WMI Subscriptions | WMI repository | WMI event logs |

### Linux Persistence

```bash
# Cron job
echo "* * * * * /tmp/backdoor.sh" >> /var/spool/cron/root

# SSH authorized_keys
echo "ssh-rsa AAAA... attacker@c2" >> ~/.ssh/authorized_keys

# .bashrc modification
echo "/tmp/backdoor.sh &" >> ~/.bashrc

# Systemd service
# Create malicious .service file in /etc/systemd/system/
```

| Technique | Location | Detection |
|-----------|----------|-----------|
| Cron jobs | `/etc/crontab`, `/var/spool/cron/` | Cron log monitoring |
| SSH keys | `~/.ssh/authorized_keys` | File integrity monitoring |
| Systemd | `/etc/systemd/system/` | New service files |
| Init scripts | `/etc/init.d/` | File changes |

## 🛡️ Detection & Prevention

### How to Detect
- **[[Endpoint detection and response (EDR)|EDR]]** - Monitor persistence locations
- **File Integrity Monitoring (FIM)** - Detect changes to startup scripts
- **Registry monitoring** - Alert on Run key modifications
- **Process auditing** - Track scheduled task and service creation
- **Baseline comparison** - Compare against known-good state

### How to Prevent / Mitigate
- Restrict administrative privileges ([[Principle of Least Privilege]])
- Application allowlisting
- Disable or monitor [[Living off the Land (LOLBAS)|LOLBins]]
- Regular system audits
- Configuration management (revert unauthorized changes)
- Credential hygiene (prevent golden ticket attacks)

## 🎤 Interview Angles

### Common Questions
- "What persistence mechanisms would you look for during incident response?"
- "How would you detect unauthorized scheduled tasks?"
- "What's the difference between persistence and [[Privilege Escalation]]?"

### Key Detection Locations
```
Windows Quick Checks:
├── HKLM/HKCU Run keys
├── Scheduled Tasks
├── Services
├── Startup folders
├── WMI subscriptions
└── DLL search order

Linux Quick Checks:
├── Crontabs
├── SSH authorized_keys
├── Systemd services
├── /etc/rc.local
├── .bashrc / .profile
└── LD_PRELOAD
```

## ✅ Best Practices
- Document baseline persistence mechanisms in your environment
- Alert on any new persistence created
- Include persistence checks in incident response playbooks
- Regularly audit startup items and scheduled tasks
- Use immutable infrastructure where possible

## 🔗 Related Concepts
- [[Cyber Kill Chain]]
- [[Living off the Land (LOLBAS)]]
- [[Web Shell]]
- [[Rootkit]]
- [[Privilege Escalation]]
- [[Command and Control (C2)]]

## 📚 References
- MITRE ATT&CK - Persistence Techniques
- Autoruns (Sysinternals)
