---
dg-publish: true
tags:
  - "#cybersecurity/persistence"
  - "#cybersecurity/forensics/linux"
  - "#interview/concepts"
aliases:
  - authorized_keys
  - SSH keys
  - SSH public key authentication
---

# SSH authorized_keys

> **One-liner:** File containing trusted SSH public keys for passwordless authentication—a common persistence mechanism where attackers add their own keys to maintain backdoor access.

## 🎯 What Is It?

The `~/.ssh/authorized_keys` file contains a list of **public keys** that are authorized to log in to a user's account via SSH without a password. Each line is one public key.

**Location:** `~/.ssh/authorized_keys` (per-user) or `/home/username/.ssh/authorized_keys`

## 🔬 How It Works

### SSH Key Authentication Flow

```
1. User generates key pair: ssh-keygen
   ├─ Private key: ~/.ssh/id_rsa (keep secret!)
   └─ Public key: ~/.ssh/id_rsa.pub (share this)

2. Public key added to server's authorized_keys
   echo "ssh-rsa AAAA..." >> ~/.ssh/authorized_keys

3. User connects with private key
   ssh -i ~/.ssh/id_rsa user@server

4. Server verifies signature using public key
   ✓ Match = Login granted (no password)
   ✗ No match = Authentication failed
```

### File Format

Each line contains: `<key-type> <public-key-data> <comment>`

```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC8... jane@laptop
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD9... jane@workstation
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFq... backdoor
```

## 🛡️ Detection & Prevention

### How to Detect Backdoors (from writeup)

#### Check File Permissions
```bash
ls -al /home/jane/.ssh/authorized_keys
-rw-rw-rw- 1 jane jane 1136 Feb 13 00:34 authorized_keys
# ⚠️ WORLD-WRITABLE (666) = VULNERABLE!
```

**Proper permissions:** `600` (read/write owner only) or `644` (owner read/write, others read)

#### Inspect Key Contents
```bash
cat ~/.ssh/authorized_keys

# Look for:
# - Unrecognized keys
# - Suspicious comments (e.g., "backdoor", "attacker")
# - Keys with no comment (unusual)
# - Recently added keys (check timestamps)
```

#### Check Timestamps
```bash
# When was authorized_keys last modified?
stat ~/.ssh/authorized_keys

# From writeup: modification time revealed attack window
stat /home/jane/.ssh/authorized_keys
Modify: 2024-02-13 00:34:16.005897449 +0000
```

#### Find All authorized_keys Files
```bash
# Search entire system
find / -name authorized_keys 2>/dev/null

# Check multiple user accounts
for user in /home/*; do
  if [ -f "$user/.ssh/authorized_keys" ]; then
    echo "=== $user ==="
    cat "$user/.ssh/authorized_keys"
  fi
done
```

### How to Prevent / Mitigate

#### Secure File Permissions
```bash
# Fix permissions on authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh

# Verify ownership
chown user:user ~/.ssh/authorized_keys

# Make immutable (prevents modification even by root)
sudo chattr +i ~/.ssh/authorized_keys
# To modify: sudo chattr -i ~/.ssh/authorized_keys
```

#### SSH Server Hardening
```bash
# /etc/ssh/sshd_config
PubkeyAuthentication yes          # Allow key auth
PasswordAuthentication no         # Disable passwords
PermitRootLogin prohibit-password # Root only via keys
StrictModes yes                   # Enforce permission checks
AuthorizedKeysFile .ssh/authorized_keys  # Specify location
```

#### Monitoring & Detection
```bash
# File integrity monitoring
# AIDE, Tripwire, osquery

# Audit rule for authorized_keys modifications
auditctl -w /home -p wa -k ssh_key_changes

# Alert on new SSH sessions
grep "Accepted publickey" /var/log/auth.log
```

## 📊 Attack Scenarios

### Scenario 1: World-Writable authorized_keys (from writeup)

**Vulnerability:**
```bash
ls -l /home/jane/.ssh/authorized_keys
-rw-rw-rw- 1 jane jane 1136 Feb 13 00:34 authorized_keys
```

**Exploitation:**
```bash
# Attacker (with www-data access) appends their key
echo "ssh-rsa AAAAB3NzaC... backdoor" >> /home/jane/.ssh/authorized_keys

# Now attacker can SSH as jane
ssh -i attacker_key jane@target
```

### Scenario 2: Web Shell to SSH Backdoor

```bash
# 1. Attacker gains web shell (www-data user)
# 2. Generates SSH key pair on attacker machine
ssh-keygen -t rsa -f backdoor_key

# 3. Finds world-writable authorized_keys
find /home -name authorized_keys -perm -002 2>/dev/null

# 4. Adds public key
echo "ssh-rsa AAAA... backdoor" >> /home/jane/.ssh/authorized_keys

# 5. Persistent SSH access established
ssh -i backdoor_key jane@target
```

## 🎤 Interview Angles

### Common Questions

- **"What is SSH authorized_keys and why is it a security risk?"**
  - *"authorized_keys contains public keys that can authenticate to a user account without a password. It's a risk because if an attacker can write to it—due to misconfigured permissions—they can add their own key and establish persistent backdoor access."*

- **"What permissions should authorized_keys have?"**
  - *"600 (rw-------) owned by the user. The .ssh directory should be 700 (rwx------). SSH's StrictModes will refuse keys if permissions are too open, but misconfigured servers or disabled StrictModes allow exploitation."*

- **"How would you detect an SSH backdoor?"**
  - *"Check authorized_keys files for unfamiliar public keys, verify file permissions aren't world-writable, review modification timestamps with `stat`, and correlate with SSH authentication logs in /var/log/auth.log."*

### STAR Story

> **Situation:** Forensic investigation of compromised web server revealed attacker maintained access even after web shell was removed.
> **Task:** Identify persistence mechanism enabling continued unauthorized access.
> **Action:** Enumerated all authorized_keys files across user accounts. Found `/home/jane/.ssh/authorized_keys` had world-writable permissions (666). File contained two public keys—one legitimate for Jane, one with comment "backdoor" added during the attack window (confirmed via `stat` timestamp). Cross-referenced with auth.log showing SSH logins from attacker IP using public key authentication.
> **Result:** Removed malicious key, fixed permissions to 600, implemented FIM on all authorized_keys files, added alerting for SSH public key authentication from unexpected IPs. Attacker lost persistent access.

## ✅ Best Practices

- **Strict Permissions:** 600 for authorized_keys, 700 for .ssh directory
- **File Integrity Monitoring:** Alert on any modifications
- **Regular Audits:** Review keys monthly, remove stale entries
- **Key Management:** Document all authorized keys and owners
- **Centralized Auth:** Use LDAP/AD with SSH keys instead of local files
- **Immutable Attribute:** Use `chattr +i` for high-security accounts
- **StrictModes:** Keep enabled in sshd_config (default on most systems)

## ❌ Common Misconceptions

- **"SSH keys are more secure than passwords"** (Partially true: only if private keys are protected and authorized_keys has proper permissions)
- **"Only root can modify authorized_keys"** (False: file owner or anyone if permissions are misconfigured)
- **"Removing SSH access removes the backdoor"** (False: keys persist even if SSH port blocked—fix permissions and remove keys)
- **"StrictModes prevents all attacks"** (False: only prevents some permission issues; doesn't stop authorized users from adding keys)

## 🔗 Related Concepts

- [[Persistence (Cyber Security)]] — SSH keys provide persistent access
- [[/etc/passwd and /etc/shadow]] — User account enumeration
- [[File Timestamps (mtime, ctime, atime)]] — Detect recent key additions
- [[Privilege Escalation]] — Often combined with SSH key backdoors
- [[Web Shell]] — Initial access vector leading to SSH backdoor
- [[Secure Shell (SSH)]] — Protocol using authorized_keys

## 📚 References

- [OpenSSH authorized_keys](https://www.ssh.com/academy/ssh/authorized-keys-file)
- [SSH Key Best Practices](https://www.ssh.com/academy/ssh/keygen)
- [TryHackMe: Linux File System Analysis](https://tryhackme.com/room/linuxfilesystemanalysis)
- SANS: SSH Key Abuse Detection
