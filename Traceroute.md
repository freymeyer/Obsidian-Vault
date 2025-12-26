---
tags:
  - "#cybersecurity/red-team/recon"
  - "#networking/tools"
  - "#interview/concepts"
aliases:
  - tracert
  - tracepath
---

# Traceroute

> **One-liner:** A network diagnostic tool that maps the path packets take to reach a destination by exploiting [[TTL (Time To Live)]] expiration.

## 🎯 What Is It?
Traceroute is a network diagnostic utility that reveals every router (hop) between your system and a destination. It works by sending packets with incrementally increasing TTL values, causing each router along the path to respond with an [[ICMP]] Time Exceeded message.

On Windows, the command is `tracert`; on Linux/macOS, it's `traceroute`.

## 🤔 Why It Matters
- **Reconnaissance:** Discover network topology and intermediate routers during [[Active Reconnaissance]]
- **Troubleshooting:** Identify where packets are being dropped or delayed
- **Path Analysis:** Understand routing decisions and potential chokepoints
- **Scope Verification:** Identify which routers are in-scope during pentests

## 🔬 How It Works

### Core Principles
1. Send packet with TTL=1 → First router decrements to 0 → Sends ICMP Time Exceeded
2. Send packet with TTL=2 → Second router decrements to 0 → Sends ICMP Time Exceeded
3. Continue incrementing TTL until destination is reached
4. Record IP addresses and response times for each hop

### Visual Flow
```
TTL=1 ──► [Router 1] ──► ICMP Time Exceeded (reveals Router 1 IP)
TTL=2 ──► [Router 1] ──► [Router 2] ──► ICMP Time Exceeded (reveals Router 2 IP)
TTL=3 ──► [Router 1] ──► [Router 2] ──► [Target] ──► ICMP Echo Reply
```

### Technical Deep-Dive
```bash
# Linux: Basic traceroute
traceroute tryhackme.com

# Windows: Basic tracert
tracert tryhackme.com

# Linux: Use ICMP instead of UDP (like Windows)
traceroute -I tryhackme.com

# Linux: Use TCP SYN (bypass ICMP filtering)
sudo traceroute -T -p 443 tryhackme.com

# Linux: Set max hops
traceroute -m 20 tryhackme.com
```

## 📊 Platform Differences

| Aspect | Linux `traceroute` | Windows `tracert` |
|--------|-------------------|-------------------|
| **Default Protocol** | UDP | ICMP |
| **Packets per Hop** | 3 | 3 |
| **Max Hops** | 30 | 30 |
| **Requires Root** | For ICMP/TCP modes | No |

## 📊 Reading Output

```
 1  router1.isp.com (192.168.1.1)    1.234 ms  1.456 ms  1.123 ms
 2  * * *
 3  core.isp.com (10.0.0.1)          15.678 ms 14.234 ms 16.789 ms
```

| Symbol | Meaning |
|--------|---------|
| IP/hostname | Router responded |
| `*` | No response (timeout, filtered, or router doesn't reply) |
| `!H` | Host unreachable |
| `!N` | Network unreachable |
| `!P` | Protocol unreachable |

## 🛡️ Detection & Prevention

### How to Detect
- Monitor for TTL=1 packets or incrementing TTL patterns
- Alert on traceroute-like behavior (multiple ICMP Time Exceeded)
- IDS signatures for traceroute activity

### How to Prevent / Mitigate
- Configure routers to not send ICMP Time Exceeded messages
- Filter outbound ICMP Time Exceeded at network perimeter
- Use firewall rules to drop low-TTL packets

## 🎤 Interview Angles

### Common Questions
- "How does traceroute work?"
  - Sends packets with incrementing TTL; each router that decrements TTL to 0 sends back ICMP Time Exceeded, revealing its IP.
- "Why might some hops show `* * *`?"
  - Router configured not to respond, ICMP filtered, or router too busy.
- "Difference between Linux traceroute and Windows tracert?"
  - Linux uses UDP by default; Windows uses ICMP.

### STAR Story
> **Situation:** Client reported intermittent connectivity to cloud services.
> **Task:** Identify where packet loss was occurring.
> **Action:** Ran traceroute during issue windows, identified consistent packet loss at hop 7 (ISP border router). Correlated with ISP maintenance windows.
> **Result:** Documented evidence for ISP escalation; issue resolved after ISP fixed misconfigured router.

## ✅ Best Practices
- Run multiple times—routes can change dynamically
- Use TCP mode (`-T`) when ICMP/UDP is filtered
- Compare traceroutes from different source locations
- Note that asymmetric routing means return path may differ

## ❌ Common Misconceptions
- "Route will always be the same" — Dynamic routing means paths change
- "All hops must respond" — Many routers are configured not to respond

## 🔗 Related Concepts
- [[ICMP]]
- [[TTL (Time To Live)]]
- [[Ping]]
- [[Active Reconnaissance]]
- [[Netcat (nc)]]

## 📚 References
- RFC 1393 - Traceroute Using an IP Option
- TryHackMe - Active Reconnaissance Room
