---
tags:
  - "#cybersecurity/linux/privilege-escalation"
  - "#cybersecurity/blue-team/hardening"
  - "#interview/concepts"
aliases:
  - sudoers
  - sudoers file
  - sudo configuration
---

# /etc/sudoers

> **One-liner:** Configuration file that defines which users can run commands as root via sudo—a critical file for privilege escalation and persistence that attackers frequently target.

## 🎯 What Is It?

The `/etc/sudoers` file controls **sudo** (superuser do) privileges on Linux systems. It defines:
- Which users can run commands with elevated privileges
- Which commands they can run
- Whether password authentication is required
- Which hosts the rules apply to

## 🔬 How It Works

### Basic Syntax

Format: `user HOST=(RUNAS) COMMANDS`

```bash
# Example entries
root    ALL=(ALL:ALL) ALL
jane    ALL=(ALL) /usr/bin/pstree
admin   ALL=(ALL) NOPASSWD: /usr/bin/systemctl
```

### Field Breakdown

| Field | Description | Example |
|-------|-------------|---------|
| **User** | Username or %group | `jane`, `%sudo` |
| **Host** | Hostname (usually ALL) | `ALL`, `webserver` |
| **(RunAs)** | User to run as | `(ALL)`, `(root)` |
| **Commands** | Allowed commands | `/usr/bin/pstree`, `ALL` |

### Dangerous Configurations

```bash
# DANGEROUS: User can run ANY command as root
attacker ALL=(ALL) ALL

# DANGEROUS: No password required
badconfig ALL=(ALL) NOPASSWD: ALL

# DANGEROUS: Vulnerable binary (from writeup)
jane ALL=(ALL) /usr/bin/pstree
# pstree can be exploited via GTFOBins!

# SAFE: Restricted command with full path
operator ALL=(ALL) /usr/bin/systemctl restart nginx
```

## 🛡️ Detection & Prevention

### How to Detect Tampering

#### File Integrity Check (from writeup)
```bash
# Debian systems: Check if /etc/sudoers was modified
sudo debsums -e -s
# Output: debsums: changed file /etc/sudoers (from sudo package)
```

#### Manual Review
```bash
# View sudoers file (use visudo for safe editing)
sudo cat /etc/sudoers

# Check timestamps
stat /etc/sudoers

# Look for suspicious entries:
# - NOPASSWD: ALL
# - Attackers' usernames
# - Wildcard commands
# - GTFOBins-exploitable binaries
```

#### Audit Sudo Usage
```bash
# Check who used sudo recently
grep sudo /var/log/auth.log

# View sudo history for specific user
sudo -l -U jane
```

### How to Prevent / Mitigate

#### Safe Configuration Practices
```bash
# ALWAYS edit with visudo (validates syntax)
sudo visudo

# Use principle of least privilege
# Instead of: jane ALL=(ALL) ALL
# Use: jane ALL=(ALL) /specific/command/path

# Require password (avoid NOPASSWD)
# Default behavior: prompt for user's password

# Use command aliases for readability
Cmnd_Alias SERVICES = /usr/bin/systemctl start, /usr/bin/systemctl stop
jane ALL=(ALL) SERVICES
```

#### Hardening Checklist
- ✅ Use `visudo` for all edits (prevents syntax errors)
- ✅ Specify full paths to commands (prevent PATH manipulation)
- ✅ Check commands against [GTFOBins](https://gtfobins.github.io/#+sudo)
- ✅ Avoid wildcards and NOPASSWD unless absolutely necessary
- ✅ Implement file integrity monitoring (FIM) on /etc/sudoers
- ✅ Enable sudo logging in syslog
- ✅ Regular audits of sudo privileges

## 📊 Common Exploitable Binaries

These binaries should **NEVER** be given unrestricted sudo access:

| Binary | Why Dangerous | Exploit Example |
|--------|---------------|-----------------|
| `vim`, `nano`, `less` | Can spawn shell | `:!/bin/bash` in vim |
| `find` | Execute commands | `find / -exec /bin/sh \;` |
| `awk`, `sed` | Execute code | `awk 'BEGIN {system("/bin/sh")}'` |
| `python`, `perl`, `ruby` | Run arbitrary code | `python -c 'import os; os.system("/bin/sh")'` |
| `tar`, `zip` | Arbitrary file write | Create malicious archives |
| `cp`, `mv` | Overwrite system files | Replace /etc/passwd |

**From writeup:** Jane had sudo access to `/usr/bin/pstree`, which can be exploited:
```bash
sudo pstree -p $(echo 'exec /bin/sh' | base64 -d | sh)
```

## 🎤 Interview Angles

### Common Questions

- **"What is /etc/sudoers used for?"**
  - *"/etc/sudoers configures sudo privileges—it defines which users can run commands as root, which commands they can run, and whether a password is required. It's critical for access control and a common target for attackers seeking privilege escalation."*

- **"Why should you use visudo instead of editing /etc/sudoers directly?"**
  - *"`visudo` validates syntax before saving. A syntax error in /etc/sudoers can lock you out of sudo entirely, requiring single-user mode to fix. `visudo` prevents this by checking the file and refusing to save if there are errors."*

- **"How would you check if /etc/sudoers was tampered with?"**
  - *"Use `debsums -e -s` to check package integrity, review the file with `sudo cat /etc/sudoers` for suspicious entries, check timestamps with `stat`, and review sudo usage in `/var/log/auth.log`. Look for NOPASSWD rules, unfamiliar usernames, or dangerous binaries like python, vim, or find."*

### STAR Story

> **Situation:** Investigating a compromised web server where attacker escalated from www-data user to root.
> **Task:** Determine the privilege escalation method and secure the system.
> **Action:** Ran `debsums -e -s` and discovered /etc/sudoers was modified. Reviewed the file and found the attacker added `www-data ALL=(ALL) NOPASSWD: /usr/bin/python3`. This allowed the attacker to run `sudo python3 -c 'import os; os.system("/bin/bash")'` to get root. Checked file timestamps confirming modification during attack window. Removed the malicious entry, implemented FIM on /etc/sudoers, and added detection rule for sudo usage by www-data.
> **Result:** Closed privilege escalation path, prevented future abuse. Implemented automated alerts for /etc/sudoers modifications and sudo usage by service accounts.

## ✅ Best Practices

- **Use visudo exclusively** - Never edit /etc/sudoers directly
- **Specify full paths** - `/usr/bin/systemctl` not `systemctl`
- **Check GTFOBins** - Verify commands can't be exploited
- **Avoid wildcards** - Don't use `*` in command specifications
- **Minimize NOPASSWD** - Require authentication except for specific cases
- **Drop-in files** - Use `/etc/sudoers.d/` for modular configuration
- **Regular audits** - Weekly review of sudo privileges
- **Monitor modifications** - FIM + alerting on /etc/sudoers changes

## ❌ Common Misconceptions

- **"sudo is only for running commands as root"** (False: can run as any user via RunAs)
- **"visudo requires vim"** (False: uses $EDITOR variable, can be nano)
- **"/etc/sudoers.d/ files override /etc/sudoers"** (False: all files are parsed, last match wins)
- **"NOPASSWD is more secure"** (False: removes authentication layer—use sparingly)

## 🔗 Related Concepts

- [[Privilege Escalation]] — /etc/sudoers misconfiguration enables PrivEsc
- [[/etc/passwd and /etc/shadow]] — Other user/auth configuration files
- [[SUID and SGID Permissions]] — Alternative privilege elevation mechanism
- [[GTFOBins]] — Database of sudo-exploitable binaries
- [[debsums]] — Tool to detect /etc/sudoers tampering
- [[Persistence (Cyber Security)]] — Attackers modify sudoers for persistence

## 📚 References

- [sudo manual](https://www.sudo.ws/docs/man/sudoers.man/)
- [GTFOBins: Sudo](https://gtfobins.github.io/#+sudo)
- [TryHackMe: Linux File System Analysis](https://tryhackme.com/room/linuxfilesystemanalysis)
- [Sudoers Security Best Practices](https://www.sudo.ws/posts/2021/01/security-guidance-for-the-sudo-project/)
