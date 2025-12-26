---
tags:
  - "#networking/protocols"
  - "#cybersecurity/red-team/recon"
  - "#interview/concepts"
aliases:
  - Teletype Network
---

# Telnet

> **One-liner:** A legacy protocol for remote CLI access that transmits all data (including credentials) in cleartext.

## 🎯 What Is It?
TELNET (Teletype Network) is a network protocol developed in 1969 for remote command-line access to systems. It operates over TCP port 23 by default. From a security perspective, Telnet is considered insecure because all communications—including usernames and passwords—are transmitted in plaintext.

Despite being deprecated for remote administration (replaced by [[Secure Shell (SSH)]]), the `telnet` client remains valuable for [[Banner Grabbing]] and testing TCP connectivity.

## 🤔 Why It Matters
- **Reconnaissance:** Used for [[Banner Grabbing]] to identify services and versions
- **Legacy Systems:** Still found on older devices (routers, switches, IoT)
- **Vulnerability:** Credentials can be captured via network sniffing
- **Testing Tool:** Quick way to test if TCP ports are open and responsive

## 🔬 How It Works

### Core Principles
1. Client initiates TCP connection to target port (default 23)
2. All data exchanged in plaintext (no encryption)
3. Session provides interactive command-line access
4. Can connect to any TCP port for manual protocol interaction

### Banner Grabbing Example
```bash
# Connect to web server and grab banner
telnet 10.10.10.1 80
Trying 10.10.10.1...
Connected to 10.10.10.1.
Escape character is '^]'.
GET / HTTP/1.1
Host: example.com

HTTP/1.1 200 OK
Server: Apache/2.4.61      # ← Banner reveals version!
...
```

### Testing SMTP
```bash
telnet mail.example.com 25
EHLO test
MAIL FROM:<test@test.com>
```

## 📊 Common Use Cases

| Use Case | Port | Purpose |
|----------|------|---------|
| Remote Admin (legacy) | 23 | Shell access (insecure) |
| [[Banner Grabbing]] HTTP | 80/443 | Identify web server |
| [[Banner Grabbing]] SMTP | 25 | Identify mail server |
| [[Banner Grabbing]] FTP | 21 | Identify FTP service |
| Port Testing | Any | Verify TCP connectivity |

## 🛡️ Detection & Prevention

### Security Risks
- **Credential Theft:** Passwords visible to network sniffers
- **Man-in-the-Middle:** Sessions easily hijacked
- **Reconnaissance:** Reveals service versions for exploit matching

### How to Detect
- Monitor for connections on port 23
- Alert on Telnet traffic to sensitive systems
- Network IDS can identify Telnet protocol signatures

### How to Prevent / Mitigate
- Replace Telnet with [[Secure Shell (SSH)]] for administration
- Disable Telnet services on all devices
- Block port 23 at network perimeter
- If Telnet is required, restrict to management VLAN

## 🎤 Interview Angles

### Common Questions
- "Why is Telnet insecure?"
  - All data including credentials transmitted in cleartext.
- "What replaced Telnet?"
  - [[Secure Shell (SSH)]] provides encrypted remote access.
- "Why would a pentester still use Telnet?"
  - [[Banner Grabbing]]—manually connecting to services to identify versions.

### STAR Story
> **Situation:** During a network assessment, discovered Telnet enabled on core switches.
> **Task:** Demonstrate the risk and recommend remediation.
> **Action:** Used Wireshark to capture Telnet session credentials in plaintext during authorized test. Documented with screenshots.
> **Result:** Client immediately disabled Telnet and migrated to SSH. Finding rated as Critical in report.

## ✅ Best Practices
- Never use Telnet for remote administration
- Use `telnet` client for quick connectivity tests and banner grabbing
- Prefer [[Netcat (nc)]] for more flexible banner grabbing

## ❌ Common Misconceptions
- "Telnet is only for remote access" — Useful for testing any TCP service
- "Telnet is obsolete and never seen" — Still common on legacy devices, IoT, industrial systems

## 🔗 Related Concepts
- [[Secure Shell (SSH)]]
- [[Banner Grabbing]]
- [[Netcat (nc)]]
- [[Active Reconnaissance]]
- [[Hydra]]

## 📚 References
- RFC 854 - Telnet Protocol Specification
- TryHackMe - Active Reconnaissance Room
