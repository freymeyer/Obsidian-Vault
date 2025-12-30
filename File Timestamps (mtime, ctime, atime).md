---
dg-publish: true
tags:
  - "#cybersecurity/forensics/file-system"
  - "#interview/concepts"
aliases:
  - mtime
  - ctime
  - atime
  - File Timestamps
  - Linux Timestamps
---

# File Timestamps (mtime, ctime, atime)

> **One-liner:** Metadata timestamps on Unix/Linux files that record when content was modified, metadata was changed, and when the file was last accessed—critical for forensic timeline analysis.

## 🎯 What Is It?

Unix-based systems maintain three primary timestamps for every file and directory:

| Timestamp | Full Name | What It Records |
|-----------|-----------|-----------------|
| **mtime** | Modify Time | Last time file **contents** were modified |
| **ctime** | Change Time | Last time file **metadata** was changed (permissions, ownership, filename) |
| **atime** | Access Time | Last time file was **accessed/read** |

## 🤔 Why It Matters

- **Incident Response:** Establish timeline of attacker activity
- **Forensic Analysis:** Determine when files were created, modified, or tampered with
- **Anomaly Detection:** Identify recently modified system binaries or config files
- **Evidence Integrity:** Document when evidence was collected (important: accessing a file updates atime!)

## 🔬 How It Works

### Core Principles
1. Every file operation updates specific timestamps
2. Timestamps are stored in the inode (filesystem metadata structure)
3. Reading a file updates atime (unless mounted with `noatime`)
4. Modifying content updates both mtime and ctime
5. Changing permissions/ownership updates only ctime

### Technical Deep-Dive

#### Viewing Timestamps
```bash
# View mtime (default ls behavior)
ls -l /var/www/html/assets/reverse.elf

# View ctime (change time)
ls -lc /var/www/html/assets/reverse.elf

# View atime (access time)
ls -lu /var/www/html/assets/reverse.elf

# View all three timestamps at once
stat /var/www/html/assets/reverse.elf
```

#### Timestamp Behavior Examples

| Action | mtime | ctime | atime |
|--------|-------|-------|-------|
| Create new file | ✅ | ✅ | ✅ |
| Edit file content | ✅ | ✅ | - |
| `chmod` (change permissions) | - | ✅ | - |
| `chown` (change owner) | - | ✅ | - |
| Rename/move file | - | ✅ | - |
| Read file (`cat`, `less`) | - | - | ✅ |
| Copy file (creates new inode) | ✅ (new) | ✅ (new) | ✅ (new) |

#### stat Command Output
```bash
$ stat /var/www/html/assets/reverse.elf
  File: /var/www/html/assets/reverse.elf
  Size: 250       	Blocks: 8          IO Block: 4096   regular file
Device: ca01h/51713d	Inode: 526643      Links: 1
Access: (0755/-rwxr-xr-x)  Uid: (   33/www-data)   Gid: (   33/www-data)
Access: 2024-02-13 02:31:29.256000000 +0000  # atime
Modify: 2024-02-13 00:26:28.000000000 +0000  # mtime
Change: 2024-02-13 00:34:50.679215113 +0000  # ctime
 Birth: -
```

## 🛡️ Detection & Prevention

### How to Detect

#### Find Recently Modified Files
```bash
# Files modified in last 24 hours
find / -type f -mtime -1 2>/dev/null

# Files with ctime changed in last 5 minutes
find / -type f -cmin -5 2>/dev/null

# Files accessed in last hour
find / -type f -amin -60 2>/dev/null
```

#### Forensic Investigation Pattern
```bash
# 1. Identify suspicious binary
ls -l /var/tmp/bash

# 2. Check all timestamps
stat /var/tmp/bash

# 3. Compare to known-good binary
md5sum /var/tmp/bash /bin/bash

# 4. Check what else was modified around that time
find / -type f -newermt "2024-02-13 00:30" ! -newermt "2024-02-13 00:40" 2>/dev/null
```

### How to Prevent / Mitigate

- **File Integrity Monitoring (FIM):** Tools like AIDE, Tripwire, osquery
- **Immutable Attributes:** Use `chattr +i` on critical system files
- **Audit Logging:** Enable `auditd` to track file access beyond timestamps
- **Mount Options:** Use `noatime` or `relatime` to reduce metadata updates
- **Forensic Collection:** Image systems before live analysis to preserve original timestamps

## 📊 Types/Categories

| Scenario | Key Timestamp | Why It Matters |
|----------|---------------|----------------|
| **Malware dropped** | mtime = when created | Initial compromise timeline |
| **Config tampering** | mtime + ctime | When attacker modified settings |
| **Backdoor account creation** | ctime on `/etc/passwd` | When user was added |
| **Log deletion** | ctime (if deleted) or mtime (if truncated) | Evidence of anti-forensics |
| **Evidence of access** | atime (unreliable in live systems) | What files attacker viewed |

## 🎤 Interview Angles

### Common Questions
- **"What's the difference between mtime and ctime?"**
  - *"mtime is when file **content** changed. ctime is when file **metadata** (permissions, ownership) changed. For example, `chmod 777 file` updates ctime but not mtime."*

- **"Why is atime unreliable during live forensics?"**
  - *"Every time you read a file during investigation—with `cat`, `grep`, or forensic tools—you update its atime. That's why dead-box forensics (imaging first) is preferred for timestamp integrity."*

- **"How would you find all files modified in the last hour?"**
  - `find / -type f -mmin -60 2>/dev/null`

### STAR Story
> **Situation:** Investigating a compromised web server where attacker uploaded a malicious binary.
> **Task:** Establish timeline of the attack and identify all affected files.
> **Action:** Used `stat` on suspicious binary to get mtime (file creation time). Ran `find / -newermt` to identify all files modified within ±10 minutes of that timestamp. Cross-referenced with web server logs showing initial exploit. Discovered attacker modified `/etc/passwd` (ctime), uploaded reverse shell (mtime), and accessed multiple config files (atime before imaging).
> **Result:** Built complete timeline showing: initial exploit (10:26), binary upload (10:26), privilege escalation (10:34), persistence via SSH key (10:34). Timeline matched log evidence and enabled us to scope the full extent of compromise.

## ✅ Best Practices

- **Always use `stat`** for comprehensive timestamp view
- **Document atime** before it gets overwritten by your investigation
- **Compare timestamps** to external sources (logs, network captures)
- **Use forensic imaging** to preserve original timestamps
- **Check `noatime` mount option** when interpreting atime data
- **Correlate with other artifacts** (process logs, network connections)

## ❌ Common Misconceptions

- **"atime shows exactly when attacker accessed a file"** (False: many systems use `relatime` or `noatime`)
- **"Timestamp manipulation is difficult"** (False: `touch` can modify mtime/atime; ctime harder to forge without filesystem manipulation)
- **"ctime = creation time"** (False: Unix doesn't natively track file creation time—ctime is **change** time for metadata)
- **"Timestamps are always accurate"** (False: system clock can be wrong, timezone issues, deliberate manipulation)

## 🔗 Related Concepts

- [[ExifTool]] — Metadata extraction from files
- [[Static Analysis]] — Analyzing file properties without execution
- [[Incident Response]] — Timeline analysis during investigations
- [[Chain of Custody]] — Preserving timestamp integrity
- [[Forensics]] — Dead-box vs live analysis considerations

## 📚 References

- [Linux man page: stat(1)](https://man7.org/linux/man-pages/man1/stat.1.html)
- [Understanding File Timestamps in Linux](https://www.redhat.com/sysadmin/linux-file-timestamps)
- SANS Digital Forensics: Filesystem Timeline Analysis
- [TryHackMe: Linux File System Analysis](https://tryhackme.com/room/linuxfilesystemanalysis)
