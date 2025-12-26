---
tags:
  - "#cybersecurity/red-team/recon"
  - "#interview/concepts"
aliases:
  - Service Enumeration
  - Banner Enumeration
---

# Banner Grabbing

> **One-liner:** A reconnaissance technique that connects to services to extract version information from their welcome messages.

## 🎯 What Is It?
Banner grabbing is an [[Active Reconnaissance]] technique where an attacker connects to network services and captures the "banner"—the identifying information that services send upon connection. This reveals software names, versions, and sometimes operating system details, which can be used to identify exploitable vulnerabilities.

## 🤔 Why It Matters
- **Vulnerability Mapping:** Identify specific versions to match against known CVEs
- **Attack Surface Analysis:** Understand what services are running
- **Exploit Selection:** Choose appropriate exploits based on version info
- **Low Noise:** Simple connections appear as normal traffic

## 🔬 How It Works

### Core Principles
1. Establish TCP connection to target port
2. Service responds with banner/greeting
3. Extract version and software information
4. Research vulnerabilities for identified versions

### Common Banner Examples

**Web Server (HTTP):**
```
Server: Apache/2.4.61 (Ubuntu)
Server: nginx/1.6.2
Server: Microsoft-IIS/10.0
```

**SSH:**
```
SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.5
```

**FTP:**
```
220 vsFTPd 0.17 ready...
```

**SMTP:**
```
220 mail.example.com ESMTP Postfix
```

## 🔬 Techniques & Tools

### Manual Methods
```bash
# Using Telnet
telnet 10.10.10.1 80
GET / HTTP/1.1
Host: target

# Using Netcat
nc 10.10.10.1 80
GET / HTTP/1.1
Host: target

# Using cURL (HTTP only)
curl -I http://10.10.10.1
```

### Automated Methods
```bash
# Nmap service version detection
nmap -sV -p 80,443,22,21 10.10.10.1

# Nmap with scripts
nmap -sV --script=banner 10.10.10.1

# Netcat batch scanning
echo "" | nc -v -n -w1 10.10.10.1 21-25
```

## 📊 Service Banners by Port

| Port | Service | Banner Contains |
|------|---------|-----------------|
| 21 | FTP | FTP daemon name/version |
| 22 | SSH | SSH version, OS hints |
| 25 | SMTP | Mail server name |
| 80/443 | HTTP | Web server version |
| 3306 | MySQL | Database version |
| 3389 | RDP | Windows version hints |

## 🛡️ Detection & Prevention

### How to Detect
- Monitor for connections that send minimal/no data after connecting
- Alert on HTTP HEAD requests with no follow-up
- IDS signatures for known recon tools (nmap version scan patterns)

### How to Prevent / Mitigate
- **Modify Banners:** Change or remove version information
- **Apache:** `ServerTokens Prod` and `ServerSignature Off`
- **Nginx:** `server_tokens off;`
- **SSH:** Modify `/etc/ssh/sshd_config` banner
- Use reverse proxies to hide backend server versions

## 🔬 Defense Configuration Examples

```apache
# Apache httpd.conf
ServerTokens Prod
ServerSignature Off
# Result: "Server: Apache" instead of "Server: Apache/2.4.61 (Ubuntu)"
```

```nginx
# Nginx nginx.conf
server_tokens off;
# Result: "Server: nginx" instead of "Server: nginx/1.6.2"
```

## 🎤 Interview Angles

### Common Questions
- "What is banner grabbing?"
  - Connecting to services to capture version information for vulnerability research.
- "What tools can you use for banner grabbing?"
  - Telnet, Netcat, nmap -sV, curl, Nmap NSE scripts.
- "How do you defend against banner grabbing?"
  - Minimize version disclosure in server configurations.

### STAR Story
> **Situation:** Tasked with initial reconnaissance on a client's external perimeter.
> **Task:** Identify services and versions without triggering security alerts.
> **Action:** Used manual banner grabbing with Netcat to check key ports (22, 80, 443). Discovered Apache 2.4.49—known for path traversal CVE-2021-41773.
> **Result:** Reported critical vulnerability before starting full assessment; client patched immediately.

## ✅ Best Practices
- Always grab banners during reconnaissance phase
- Document all discovered versions for later CVE research
- Use manual methods when stealth is required
- Correlate banner info with CVE databases (NVD, Exploit-DB)

## ❌ Common Misconceptions
- "Removing banners stops reconnaissance" — Attackers can fingerprint via behavior
- "Banner grabbing is always detected" — Simple connections look like normal traffic

## 🔗 Related Concepts
- [[Active Reconnaissance]]
- [[Telnet]]
- [[Netcat (nc)]]
- [[Passive Reconnaissance]]
- [[nmap]]

## 📚 References
- OWASP Testing Guide - Banner Grabbing
- TryHackMe - Active Reconnaissance Room
