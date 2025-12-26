---
tags:
  - "#cybersecurity/red-team/recon"
  - "#networking/protocols"
  - "#interview/concepts"
aliases:
  - ICMP Echo
  - ping command
---

# Ping

> **One-liner:** A network utility that sends ICMP Echo requests to test connectivity and determine if a remote host is online.

## 🎯 What Is It?
Ping is a fundamental network diagnostic tool that sends [[ICMP]] Echo Request packets (Type 8) to a target and waits for Echo Reply packets (Type 0). It's used to verify that a host is reachable and measure round-trip latency.

The name comes from sonar—like pinging a submarine to detect its location.

## 🤔 Why It Matters
- **Reconnaissance:** First step in [[Active Reconnaissance]] to verify a target is alive before deeper scanning
- **Troubleshooting:** Quickly diagnose network connectivity issues
- **Latency Measurement:** Measure round-trip time (RTT) to hosts
- **Packet Loss Detection:** Identify unreliable network paths

## 🔬 How It Works

### Core Principles
1. Sends ICMP Echo Request (Type 8, Code 0) to target
2. Target responds with ICMP Echo Reply (Type 0, Code 0)
3. Measures time between request and reply (RTT)
4. Reports statistics: packets sent, received, lost, and timing

### Technical Deep-Dive
```bash
# Linux: Send 5 ping packets
ping -c 5 10.10.10.1

# Windows: Send 5 ping packets
ping -n 5 10.10.10.1

# Linux: Set custom packet size (data portion)
ping -s 1000 -c 3 10.10.10.1

# Linux: Flood ping (requires root)
sudo ping -f 10.10.10.1
```

### ICMP Header Structure
| Field | Size | Description |
|-------|------|-------------|
| Type | 1 byte | 8 for Echo Request, 0 for Echo Reply |
| Code | 1 byte | Always 0 for ping |
| Checksum | 2 bytes | Error-checking |
| Identifier | 2 bytes | Match requests to replies |
| Sequence | 2 bytes | Track packet order |
| **Total ICMP Header** | **8 bytes** | |

## 🛡️ Detection & Prevention

### How to Detect
- Monitor for unusual ICMP traffic patterns (volume, timing)
- Log ICMP Echo requests in firewall
- Detect ping sweeps (sequential IP targeting)

### How to Prevent / Mitigate
- **Windows Firewall:** Blocks ICMP by default
- **Linux iptables:** `iptables -A INPUT -p icmp --icmp-type echo-request -j DROP`
- Rate-limit ICMP to prevent reconnaissance and DoS

## 📊 Command Options

| Option (Linux) | Option (Windows) | Description |
|----------------|------------------|-------------|
| `-c <count>` | `-n <count>` | Number of packets to send |
| `-s <size>` | `-l <size>` | Payload size in bytes |
| `-i <interval>` | `-w <timeout>` | Interval/timeout settings |
| `-t <ttl>` | `-i <ttl>` | Set [[TTL (Time To Live)]] |
| `-W <timeout>` | `-w <timeout>` | Wait timeout for reply |

## 🎤 Interview Angles

### Common Questions
- "What protocol does ping use?"
  - ICMP (Internet Control Message Protocol)
- "Why might a ping fail even if the host is up?"
  - Firewall blocking ICMP, router filtering, host configured to ignore ICMP
- "What's the ICMP header size?"
  - 8 bytes

### STAR Story
> **Situation:** During a pentest, initial nmap scans showed all hosts as down.
> **Task:** Determine if hosts were actually offline or blocking ICMP.
> **Action:** Used nmap with `-Pn` flag to skip host discovery and ran TCP SYN scans directly. Also tried ARP ping for local subnet.
> **Result:** Discovered 12 live hosts with open services that were blocking ICMP—classic firewall evasion scenario.

## ✅ Best Practices
- Don't rely solely on ping for host discovery—ICMP may be blocked
- Use `-c` (Linux) or `-n` (Windows) to limit packet count
- Combine with other discovery methods ([[Netcat (nc)]], nmap TCP/ARP ping)

## ❌ Common Misconceptions
- "If ping fails, the host is down" — False; ICMP may be filtered
- "Ping is harmless" — Can be used for reconnaissance and DoS (ping flood)

## 🔗 Related Concepts
- [[ICMP]]
- [[Active Reconnaissance]]
- [[Traceroute]]
- [[TTL (Time To Live)]]
- [[Netcat (nc)]]

## 📚 References
- RFC 792 - Internet Control Message Protocol
- TryHackMe - Active Reconnaissance Room
