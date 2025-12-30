---
dg-publish: true
tags:
  - "#cybersecurity/forensics/malware"
  - "#cybersecurity/blue-team/detection"
  - "#interview/concepts"
aliases:
  - Rootkit
  - Rootkits
  - Kernel Rootkit
  - User-mode Rootkit
---

# Rootkits

> **One-liner:** Stealthy malware that hides its presence by modifying the operating system or kernel, allowing attackers to maintain privileged access while evading detection tools.

## 🎯 What Is It?

A **rootkit** is a collection of malicious software tools designed to:
1. **Gain administrative ("root") access** to a system
2. **Hide the attacker's presence** by manipulating system calls, processes, files, and network connections
3. **Maintain persistent access** even after system reboots

The term "rootkit" comes from:
- **"root"** = highest privilege level on Unix/Linux systems
- **"kit"** = collection of tools for maintaining that access

## 🤔 Why It Matters

- **Stealth:** Rootkits hide from standard detection tools (ps, ls, netstat)
- **Persistence:** Survive reboots and system updates
- **Privilege Escalation:** Often installed after initial compromise to maintain root access
- **Detection Challenge:** Traditional antivirus struggles with kernel-level rootkits
- **Incident Response:** Requires specialized tools (chkrootkit, rkhunter) for detection

## 🔬 How It Works

### Core Principles
1. **System Call Hooking:** Intercept OS functions to hide malicious activity
2. **Kernel Modification:** Alter kernel memory or load malicious kernel modules
3. **File Hiding:** Remove files/processes from directory listings
4. **Network Concealment:** Hide open ports and network connections
5. **Log Manipulation:** Delete or modify system logs

### Technical Deep-Dive

#### Rootkit Types

| Type | Location | Privilege | Stealth | Complexity |
|------|----------|-----------|---------|------------|
| **User-mode** | Application layer | User-level | Medium | Low |
| **Kernel-mode** | Kernel space | Ring 0 | High | High |
| **Bootkit** | Boot loader/MBR | Pre-OS | Very High | Very High |
| **Firmware** | BIOS/UEFI | Hardware | Extreme | Extreme |

#### Techniques

**File Hiding:**
```bash
# Normal: /bin/ls shows all files
$ ls /var/tmp/
backdoor.sh  malware.elf

# With rootkit: hooks readdir() system call
$ ls /var/tmp/
# (empty) - rootkit filters out malicious files
```

**Process Hiding:**
```bash
# Normal: ps shows attacker's process
$ ps aux | grep backdoor
root  1337  malware_backdoor

# With rootkit: hooks /proc filesystem
$ ps aux | grep backdoor
# (no results)
```

**Network Hiding:**
```bash
# Normal: netstat shows C2 connection
$ netstat -antp | grep 4444
tcp  0  0  10.10.10.10:4444  ESTABLISHED

# With rootkit: filters netstat output
$ netstat -antp | grep 4444
# (no results)
```

## 🛡️ Detection & Prevention

### How to Detect

#### Detection Tools (from writeup)

**chkrootkit (Check Rootkit):**
```bash
# Simple shell-based rootkit scanner
sudo chkrootkit

# Output example:
Checking `amd'...                    not found
Checking `basename'...               not infected
Checking `chfn'...                   not infected
...
```

**rkhunter (Rootkit Hunter):**
```bash
# More comprehensive scanner with database
sudo rkhunter --update  # Update signatures first
sudo rkhunter -c -sk    # Check system (-sk skips keypress prompts)

# Output example:
[ Rootkit Hunter version 1.4.6 ]
Checking system commands...
  Checking 'strings' command         [ OK ]
  Checking for preloading variables  [ None found ]
...
```

#### Manual Detection Techniques

```bash
# 1. Compare hashes of system binaries
# Known-good hashes vs current system
debsums -e -s  # Debian package integrity check

# 2. Look for suspicious kernel modules
lsmod | less
# Look for: random names, unusual timing

# 3. Check for hidden processes
# Compare ps output vs /proc directory
ls /proc/ | grep -E '^[0-9]+$' | wc -l  # Count /proc PIDs
ps aux | wc -l                          # Count ps output
# Mismatch = potential process hiding

# 4. Scan for hidden files
# Use external tools on mounted filesystem
```

#### Indicators of Compromise

| Indicator | What to Look For |
|-----------|------------------|
| **Modified system binaries** | `ls`, `ps`, `netstat` with wrong checksums |
| **Unexpected kernel modules** | `lsmod` shows unknown `.ko` files |
| **Hidden processes** | Discrepancy between `/proc` and `ps` |
| **Unusual system calls** | `strace` shows hooks or redirects |
| **Suspicious cron jobs** | Rootkit persistence mechanisms |

### How to Prevent / Mitigate

#### Prevention
```bash
# 1. Keep systems patched (prevent initial compromise)
apt update && apt upgrade

# 2. Use kernel security modules
# - SELinux / AppArmor (mandatory access control)
# - Secure Boot (UEFI signature verification)

# 3. File integrity monitoring
# - AIDE, Tripwire, osquery
# - Alert on changes to /bin, /sbin, /usr/bin

# 4. Disable unnecessary kernel modules
# /etc/modprobe.d/blacklist.conf
blacklist suspicious_module
```

#### Remediation

⚠️ **Critical:** Rootkits at kernel level cannot be trusted to remove cleanly.

**Proper Response:**
1. **Isolate** the system from network
2. **Preserve** evidence (memory dump, disk image)
3. **Rebuild** the system from known-good backups
4. **DO NOT** attempt to "clean" a rootkitted system—assume full compromise

```bash
# If absolutely necessary to investigate live:
# Boot from external media (USB/CD) with trusted binaries
# Mount suspect filesystem as read-only
# Run scanners from external media

# Example: Boot from LiveCD
mount -o ro /dev/sda1 /mnt/suspect
chkrootkit -r /mnt/suspect
```

## 📊 Types/Categories

### By Privilege Level

| Type | Description | Examples |
|------|-------------|----------|
| **User-mode** | Application-level hooks (DLL injection, LD_PRELOAD) | Hacker Defender, FU |
| **Kernel-mode** | Kernel memory manipulation, LKM rootkits | Adore, Knark, Diamorphine |
| **Bootkit** | Boot process manipulation (MBR/UEFI) | TDL4, Rovnix |
| **Firmware** | BIOS/UEFI persistence | LoJax, MosaicRegressor |

### By Platform

| Platform | Common Techniques |
|----------|-------------------|
| **Linux** | Loadable Kernel Modules (LKM), LD_PRELOAD, /dev/kmem |
| **Windows** | DLL injection, DKOM (Direct Kernel Object Manipulation), SSDT hooking |
| **macOS** | Kernel extensions (kexts), Mach-O hooking |

## 🎤 Interview Angles

### Common Questions

- **"What is a rootkit?"**
  - *"A rootkit is malware that hides its presence by manipulating the operating system. It can hide files, processes, network connections, and even modify system utilities to avoid detection. The name comes from 'root' access plus 'kit' of tools."*

- **"How do rootkits differ from regular malware?"**
  - *"Regular malware executes malicious actions. Rootkits focus on **stealth**—they hide other malware, backdoors, and attacker activity. A rootkit doesn't attack directly; it conceals the attack from defenders."*

- **"How would you detect a rootkit?"**
  - *"Use specialized tools like chkrootkit or rkhunter. Check file integrity with debsums. Compare running processes from ps vs /proc directory. Look for modified system binaries or unknown kernel modules. Best practice: boot from trusted external media for offline analysis."*

- **"Can you remove a rootkit?"**
  - *"For kernel-level rootkits, **no**—you cannot trust any tools on the compromised system. The only safe remediation is to wipe and rebuild from clean backups. User-mode rootkits might be cleanable, but rebuilding is still recommended."*

### STAR Story

> **Situation:** Server exhibited strange behavior—security tools reported no issues, but network monitoring showed suspicious outbound traffic.
> **Task:** Investigate potential advanced persistent threat (APT) on the server.
> **Action:** Ran chkrootkit and rkhunter from a LiveCD with external trusted binaries. Discovered modified `/usr/bin/ps` and `/bin/netstat` binaries. Found hidden kernel module via `lsmod` comparison. Used `debsums` to identify 12 modified system binaries. Isolated the server and performed memory forensics, revealing a kernel-mode rootkit hiding a cryptomining botnet.
> **Result:** Imaged the system for forensics, rebuilt from backups, identified entry point as unpatched SSH vulnerability. Implemented automated integrity checking with AIDE and mandatory weekly rkhunter scans. No repeat compromise in 18+ months.

## ✅ Best Practices

- **Assume Compromise:** If rootkit detected, treat entire system as untrusted
- **Offline Analysis:** Use LiveCD/USB with trusted binaries for investigation
- **File Integrity Monitoring:** Baseline and alert on changes to system binaries
- **Regular Scans:** Automated chkrootkit/rkhunter scans (weekly minimum)
- **Secure Boot:** Enable UEFI Secure Boot to prevent bootkits
- **Kernel Hardening:** SELinux, AppArmor, grsecurity
- **Rebuild, Don't Clean:** Never trust a rootkitted system—rebuild from scratch

## ❌ Common Misconceptions

- **"Antivirus will catch rootkits"** (False: rootkits hide from AV by design)
- **"You can remove a rootkit with a tool"** (Dangerous: rootkit controls the OS—can't trust cleanup)
- **"Rootkits only affect servers"** (False: workstations, IoT devices also targeted)
- **"Rebooting removes rootkits"** (False: most persist through reboot; some even survive OS reinstall via firmware)

## 🔗 Related Concepts

- [[Persistence (Cyber Security)]] — Rootkits provide ultimate persistence
- [[Privilege Escalation]] — Often follows initial privilege escalation
- [[SUID and SGID Permissions]] — Can be hidden by rootkits
- [[Kernel Module]] — Linux rootkit delivery mechanism
- [[Incident Response]] — Rootkit detection during IR
- [[Malware Analysis]] — Analyzing rootkit samples
- [[debsums]] — Tool for detecting modified binaries

## 📚 References

- [chkrootkit](https://www.chkrootkit.org/)
- [Rootkit Hunter (rkhunter)](https://rkhunter.sourceforge.net/)
- [TryHackMe: Linux File System Analysis](https://tryhackme.com/room/linuxfilesystemanalysis)
- SANS: Rootkit Detection and Analysis
- Rootkit.com (historical archive)
