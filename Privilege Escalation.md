---
tags:
  - "#cybersecurity/web-sec/access-control"
  - "#cybersecurity/red-team/post-exploitation"
  - "#interview/concepts"
aliases:
  - PrivEsc
  - Privilege Elevation
severity: High
---

# Privilege Escalation

> **One-liner:** Gaining higher permissions than originally granted to access restricted resources or actions.

## 🎯 What Is It?
Privilege escalation occurs when an attacker exploits a vulnerability, design flaw, or misconfiguration to gain elevated access. It's a critical step in most attack chains after initial access.

## 💥 Why It Matters (Impact)
- **Confidentiality:** Access to sensitive data (credentials, PII, secrets)
- **Integrity:** Ability to modify systems, install backdoors
- **Availability:** Can disable security tools, deploy ransomware

## 📊 Types of Privilege Escalation

### Vertical Privilege Escalation
Gaining access to **higher-level** permissions (user → admin → root/SYSTEM).

| Technique | Platform | Example |
|-----------|----------|---------|
| Kernel exploits | Linux/Windows | Dirty COW, PrintNightmare |
| SUID/SUDO abuse | Linux | `sudo -l` misconfigs |
| Service misconfigs | Windows | Unquoted service paths |
| Token manipulation | Windows | Impersonation tokens |

### Horizontal Privilege Escalation
Accessing **same-level** accounts/data you shouldn't have access to.

| Technique | Example |
|-----------|---------|
| [[Insecure Direct Object Reference (IDOR)]] | Changing `?user_id=123` to `?user_id=456` |
| Session hijacking | Stealing another user's session token |
| Parameter tampering | Modifying hidden form fields |

## 🔬 Common PrivEsc Techniques

### Linux
```bash
# Check sudo permissions
sudo -l

# Find SUID binaries
find / -perm -4000 2>/dev/null

# Check cron jobs
cat /etc/crontab
ls -la /etc/cron.*

# World-writable files
find / -writable -type f 2>/dev/null
```

### Windows
```powershell
# Check current privileges
whoami /priv

# Find unquoted service paths
wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "c:\windows"

# Check scheduled tasks
schtasks /query /fo LIST /v
```

## 🛠️ Tools

| Tool | Purpose |
|------|---------|
| LinPEAS | Linux enumeration |
| WinPEAS | Windows enumeration |
| PowerUp | Windows PowerShell PrivEsc |
| BeRoot | Multi-platform checker |
| GTFOBins | Unix binary exploits |
| LOLBAS | Windows living-off-the-land |

## 🛡️ Detection & Prevention

| Control | Implementation |
|---------|---------------|
| Least Privilege | Minimize admin accounts |
| Patch Management | Keep systems updated |
| Monitoring | Alert on privilege changes |
| Hardening | Remove SUID, fix sudoers |

## 🎤 Interview STAR Example
> **Situation:** During a pentest, gained initial shell access as low-privilege web user.
> **Task:** Escalate to root to demonstrate full compromise.
> **Action:** Ran LinPEAS, found cron job running as root that executed a world-writable script. Injected reverse shell into script.
> **Result:** Obtained root shell within 20 minutes. Recommended removing world-writable permissions and using absolute paths.

## 🔗 Related Concepts
- [[Broken Access Control]]
- [[Insecure Direct Object Reference (IDOR)]]
- [[Authorization]]
- [[Authorization Bypass]]

## 📚 References
- GTFOBins: https://gtfobins.github.io/
- LOLBAS: https://lolbas-project.github.io/
- HackTricks PrivEsc
