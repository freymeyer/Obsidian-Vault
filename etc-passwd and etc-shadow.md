---
tags:
  - "#cybersecurity/forensics/linux"
  - "#cybersecurity/red-team/enumeration"
  - "#interview/concepts"
aliases:
  - /etc/passwd
  - /etc/shadow
  - passwd file
  - shadow file
---

# /etc/passwd and /etc/shadow

> **One-liner:** Core Linux files storing user account information (/etc/passwd) and password hashes (/etc/shadow)—critical targets for reconnaissance, privilege escalation, and backdoor detection.

## 🎯 What Is It?

**`/etc/passwd`** - World-readable file containing user account information
**`/etc/shadow`** - Root-only file containing password hashes and aging policies

These files are fundamental to Linux user management and authentication.

## 🔬 How It Works

### /etc/passwd Structure

Format: `username:x:UID:GID:comment:home_dir:shell`

```bash
root:x:0:0:root:/root:/bin/bash
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
jane:x:1002:1002:Jane Smith:/home/jane:/bin/bash
```

| Field | Description | Example |
|-------|-------------|---------|
| Username | Login name | `root`, `jane` |
| Password | `x` = in /etc/shadow | `x` |
| UID | User ID (0 = root) | `0`, `1002` |
| GID | Primary group ID | `0`, `1002` |
| GECOS | User info/comment | `Jane Smith` |
| Home Dir | User's home directory | `/home/jane` |
| Shell | Login shell | `/bin/bash`, `/usr/sbin/nologin` |

### /etc/shadow Structure

Format: `username:hash:lastchange:min:max:warn:inactive:expire:reserved`

```bash
root:$6$rounds=5000$...:19000:0:99999:7:::
jane:$6$salt$hash...:19000:0:99999:7:::
```

| Field | Description |
|-------|-------------|
| Username | Links to /etc/passwd |
| Hash | Password hash (salted) |
| Last change | Days since 1970-01-01 |
| Min days | Min days between changes |
| Max days | Password expiry |
| Warn | Days before expiry warning |

## 🛡️ Detection & Prevention

### How to Detect Backdoors

#### UID 0 Backdoor (from writeup)
```bash
# Find all accounts with UID 0 (root privileges)
cat /etc/passwd | cut -d: -f1,3 | grep ':0$'
# Expected output: root:0
# If additional accounts: BACKDOOR!
```

#### Suspicious Accounts
```bash
# List all user accounts
cat /etc/passwd

# Check for:
# - Accounts with UID 0 besides root
# - Accounts with /bin/bash shell but shouldn't have login
# - Recently created accounts (check timestamps with stat)
stat /etc/passwd
```

#### Shadow File Access
```bash
# Only root should read /etc/shadow
ls -l /etc/shadow
-rw-r----- 1 root shadow 1234 Dec 30 10:00 /etc/shadow

# Check group membership
getent group shadow
# shadow:x:42:
```

### How to Prevent / Mitigate

- **Monitor UID 0 accounts** - Only root should have UID 0
- **File integrity monitoring** - Alert on /etc/passwd and /etc/shadow modifications
- **Audit user creation** - Log all `useradd`, `adduser` commands
- **Restrict permissions** - /etc/shadow should be mode 640 or 600
- **Review shells** - System accounts should use `/usr/sbin/nologin` or `/bin/false`

## 🎤 Interview Angles

### Common Questions

- **"What's the difference between /etc/passwd and /etc/shadow?"**
  - *"/etc/passwd is world-readable and contains user account metadata—username, UID, home directory, shell. /etc/shadow is root-only and contains actual password hashes. This separation exists for security—you don't want hashes readable by everyone."*

- **"How would you detect a backdoor account?"**
  - *"Check for UID 0 accounts besides root: `cat /etc/passwd | grep ':0:'`. Look for recently modified timestamps on /etc/passwd. Check for accounts with login shells that shouldn't have them. Review /etc/shadow for accounts with no password set (! or *)."*

- **"Why is UID 0 significant?"**
  - *"UID 0 is root—full system privileges. If an attacker creates another account with UID 0, they have a backdoor with root access. It's a persistence technique."*

### STAR Story

> **Situation:** During forensic analysis of a compromised Linux server, discovered evidence of unauthorized access.
> **Task:** Identify how the attacker maintained persistence on the system.
> **Action:** Examined /etc/passwd using `cat /etc/passwd | cut -d: -f1,3 | grep ':0$'` and found a second UID 0 account named "b4ckd00r3d" alongside root. Checked timestamps with `stat /etc/passwd` showing modification during the attack window. Verified in /etc/shadow that the account had a valid password hash set.
> **Result:** Confirmed backdoor account providing persistent root access. Removed the account, identified initial access vector (SSH key misconfiguration), implemented FIM on critical files. Added detection rule to alert on any UID 0 accounts besides root.

## ✅ Best Practices

- **Regular audits** - Weekly review of /etc/passwd for UID 0, unexpected shells
- **File integrity monitoring** - AIDE, Tripwire, osquery on /etc/passwd and /etc/shadow
- **Centralized auth** - Use LDAP/AD instead of local accounts where possible
- **Password policies** - Enforce via /etc/login.defs and PAM
- **Audit logging** - Enable auditd rules for /etc/passwd and /etc/shadow modifications

## ❌ Common Misconceptions

- **"x in password field means no password"** (False: x means hash is in /etc/shadow)
- **"Only /etc/shadow contains sensitive data"** (False: /etc/passwd shows usernames, UIDs, shells—valuable for recon)
- **"/etc/passwd modifications are always logged"** (False: requires auditd or SIEM integration)
- **"nologin shell prevents all access"** (False: prevents interactive shell but not SFTP, cron jobs, etc.)

## 🔗 Related Concepts

- [[/etc/sudoers]] — Sudo privilege configuration
- [[SSH authorized_keys]] — Another persistence mechanism
- [[Privilege Escalation]] — UID 0 backdoor enables escalation
- [[Persistence (Cyber Security)]] — Backdoor accounts for persistence
- [[File Timestamps (mtime, ctime, atime)]] — Detect recent modifications
- [[SUID and SGID Permissions]] — Another privilege escalation vector

## 📚 References

- [Linux man page: passwd(5)](https://man7.org/linux/man-pages/man5/passwd.5.html)
- [Linux man page: shadow(5)](https://man7.org/linux/man-pages/man5/shadow.5.html)
- [TryHackMe: Linux File System Analysis](https://tryhackme.com/room/linuxfilesystemanalysis)
- SANS Linux Forensics
