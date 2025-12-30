---
dg-publish: true
tags:
  - "#cybersecurity/forensics/tools"
  - "#cybersecurity/blue-team/soc"
  - "#interview/concepts"
aliases:
  - debsums
  - Package integrity verification
  - Debian checksums
---

# debsums

> **One-liner:** Debian/Ubuntu package integrity verification tool that detects file tampering by comparing installed files against their MD5 checksums stored in package metadata.

## 🎯 What Is It?

**debsums** is a Linux utility for **verifying the integrity of installed Debian/Ubuntu packages** by checking if system files have been modified, corrupted, or replaced since installation.

**Purpose:** Detect:
- Rootkit modifications to system binaries
- Configuration file tampering
- Malware replacing system utilities
- Accidental corruption

**Install:**
```bash
sudo apt install debsums
```

## 🔬 How It Works

### MD5 Checksum Verification

```
┌──────────────┐       ┌─────────────────┐
│   Package    │       │  Installed File │
│   Database   │       │  (e.g., /bin/ls)│
│              │       │                 │
│ MD5: abc123  │ ───── │  Calculate MD5  │
└──────────────┘       └─────────────────┘
        │                       │
        │                       ├─ abc123 ✓ MATCH
        └───────────────────────┼─ def456 ✗ TAMPERED
```

### Basic Usage

#### Check All Packages
```bash
# Verify all installed packages (noisy, shows missing docs)
sudo debsums

# Silent mode - only show errors
sudo debsums -s

# Check configuration files only
sudo debsums -a

# Check only altered files (excludes missing files)
sudo debsums -e -s
```

#### Check Specific Package
```bash
# Verify openssh-server
sudo debsums openssh-server

# Verify sudo package (from writeup)
sudo debsums sudo
debsums: changed file /etc/sudoers (from sudo package)
```

#### Detect Modified Binaries
```bash
# Check critical system binaries
sudo debsums coreutils
sudo debsums bash
sudo debsums systemd

# Check if /bin/ls has been replaced (rootkit indicator)
sudo debsums -e coreutils | grep '/bin/ls'
```

## 🛡️ Detection & Prevention

### How to Detect Tampering (from writeup)

#### Find Modified Configuration Files
```bash
# From writeup Task 6
sudo debsums -e -s
debsums: changed file /etc/sudoers (from sudo package)

# This revealed attacker modified sudo configuration
```

#### Investigate Specific File
```bash
# Compare file hash manually
md5sum /etc/sudoers

# Check what was changed
sudo cat /etc/sudoers
# Found: www-data ALL=(ALL) NOPASSWD: /usr/bin/python3
```

#### Detect Rootkit Binaries
```bash
# Check if core utilities were replaced
sudo debsums -s coreutils bash util-linux

# Example output if rootkit present:
debsums: changed file /bin/ls (from coreutils package)
debsums: changed file /bin/ps (from procps package)
```

### Integration with Forensic Workflow

```bash
# Step 1: Baseline check on clean system
sudo debsums > /var/log/debsums_baseline.txt

# Step 2: During incident response
sudo debsums -e -s > /var/log/debsums_current.txt

# Step 3: Compare results
diff /var/log/debsums_baseline.txt /var/log/debsums_current.txt

# Step 4: Investigate changed files
sudo stat /etc/sudoers
sudo cat /etc/sudoers
```

### Limitations

- **Only checks files with MD5 in package DB** (not all files tracked)
- **Configuration files expected to change** (use `-a` to include them)
- **Doesn't detect new files** (e.g., web shells in /var/www)
- **MD5 can be spoofed** (if rootkit has root access, it can manipulate)
- **No protection against kernel-level rootkits**

## 📊 Attack Detection Examples

### Scenario 1: Modified /etc/sudoers (from writeup)

**Context:** Attacker gained www-data web shell access, modified sudoers for privilege escalation

**Detection:**
```bash
sudo debsums -e -s
debsums: changed file /etc/sudoers (from sudo package)

# Investigate
sudo tail /etc/sudoers
www-data ALL=(ALL) NOPASSWD: /usr/bin/python3
# ⚠️ BACKDOOR: www-data can sudo python3 without password!
```

**Remediation:**
```bash
# Restore original
sudo apt install --reinstall sudo

# Verify integrity
sudo debsums sudo
```

### Scenario 2: Rootkit Detection

**Binary Replacement:**
```bash
# Attacker replaces /bin/ls to hide files
sudo debsums coreutils
debsums: changed file /bin/ls (from coreutils package)

# Restore clean binary
sudo apt install --reinstall coreutils
```

### Scenario 3: Proactive Monitoring

**Daily Integrity Check:**
```bash
# Cronjob: /etc/cron.daily/debsums-check
#!/bin/bash
CHANGES=$(debsums -e -s 2>&1)
if [ -n "$CHANGES" ]; then
  echo "$CHANGES" | mail -s "ALERT: File integrity violations" soc@company.com
fi
```

## 🎤 Interview Angles

### Common Questions

- **"What is debsums and why is it useful in incident response?"**
  - *"debsums is a Debian/Ubuntu tool that verifies installed package integrity by comparing file MD5 checksums. In IR, it helps detect if system binaries or configuration files were tampered with by attackers or rootkits."*

- **"How does debsums differ from AIDE or Tripwire?"**
  - *"debsums uses package metadata checksums (built-in, no setup), while AIDE/Tripwire require baseline creation and track all files. debsums is quick for spot checks; AIDE/Tripwire provide comprehensive FIM."*

- **"What are the limitations of debsums?"**
  - *"Only checks packaged files, doesn't detect new files (like web shells), MD5 can be bypassed if rootkit has root access, configuration files often legitimately modified (use `-a` flag)."*

### STAR Story

> **Situation:** Security alert triggered for suspicious sudo activity on production web server. Required quick determination if system files were compromised.
> **Task:** Verify integrity of critical system files and identify unauthorized modifications without taking server offline.
> **Action:** Ran `debsums -e -s` to check all packages for changes. Tool flagged `/etc/sudoers` as modified. Inspected file and found unauthorized entry: `www-data ALL=(ALL) NOPASSWD: /usr/bin/python3`, allowing web user to execute Python with root privileges. Checked timestamps with `stat`, confirmed modification during attack window. Used debsums to verify core binaries (coreutils, bash) were clean—no rootkit.
> **Result:** Identified privilege escalation backdoor without false positives from legitimate config changes. Reinstalled sudo package (`apt install --reinstall sudo`), removed backdoor, implemented FIM on critical files. Attack contained without full system rebuild.

## ✅ Best Practices

- **Regular Scans:** Run daily debsums checks via cron
- **Alert on Changes:** Email SOC when critical files modified
- **Combine with FIM:** Use AIDE/Tripwire for comprehensive coverage
- **Baseline on Install:** Capture clean system state
- **Check After Updates:** Verify package integrity post-patching
- **Focus on Critical Packages:** sudo, openssh-server, coreutils, bash
- **Correlate with Logs:** Match modifications to auth.log, syslog

## 🧰 Cheat Sheet

```bash
# Basic verification
sudo debsums                    # Check all packages (verbose)
sudo debsums -s                 # Silent (errors only)
sudo debsums -e -s              # Exclude missing files, silent

# Configuration files
sudo debsums -a                 # Include config files (expected changes)
sudo debsums -c                 # Check config files only

# Specific package
sudo debsums <package>          # Check one package
sudo debsums sudo openssh-server coreutils

# Generate checksums (if missing)
sudo debsums -g                 # Generate missing MD5s

# Forensic workflow
sudo debsums -e -s > tampered_files.txt
while read file; do stat "$file"; done < tampered_files.txt

# Restore integrity
sudo apt install --reinstall <package>
```

## ❌ Common Misconceptions

- **"debsums detects all tampering"** (False: only checks packaged files, misses web shells, scripts)
- **"Changed file = compromise"** (False: config files legitimately change—use `-e` and context)
- **"debsums can replace AIDE"** (False: complementary tools—debsums for quick package checks, AIDE for full FIM)
- **"Works on all Linux distros"** (False: Debian/Ubuntu only; RHEL uses `rpm -Va`)

## 🔗 Related Concepts

- [[/etc/sudoers]] — Common file to check; attacker target for privilege escalation
- [[Rootkits]] — debsums detects binary replacement rootkits
- [[File Integrity Monitoring (FIM)]] — Broader concept; debsums is one implementation
- [[Incident Response]] — debsums used during IR for quick integrity assessment
- [[AIDE (Advanced Intrusion Detection Environment)]] — Comprehensive FIM alternative
- [[Tripwire]] — Commercial FIM solution
- [[Static Analysis]] — Comparing known-good vs. current state

## 📚 References

- [debsums man page](https://manpages.debian.org/debsums)
- [TryHackMe: Linux File System Analysis](https://tryhackme.com/room/linuxfilesystemanalysis)
- [Debian Wiki: Package Integrity](https://wiki.debian.org/DebianPackageManagement)
- SANS: Using debsums for Incident Response
