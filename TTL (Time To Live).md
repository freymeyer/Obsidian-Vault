---
tags:
  - "#networking/protocols"
  - "#interview/concepts"
aliases:
  - TTL
  - Time To Live
  - Hop Limit
---

# TTL (Time To Live)

> **One-liner:** A field in IP packet headers that limits how many router hops a packet can traverse before being discarded.

## 🎯 What Is It?
Time To Live (TTL) is an 8-bit field in the IP header that prevents packets from circulating indefinitely in a network. Despite its name suggesting time, TTL actually represents a hop count—each router decrements the value by 1, and when it reaches 0, the packet is dropped.

In IPv6, this field is called "Hop Limit" to more accurately describe its function.

## 🤔 Why It Matters
- **Loop Prevention:** Ensures packets don't loop forever in misconfigured networks
- **Reconnaissance:** [[Traceroute]] exploits TTL to map network paths
- **OS Fingerprinting:** Different operating systems use different default TTLs
- **Troubleshooting:** TTL values help diagnose routing issues

## 🔬 How It Works

### Core Principles
1. Sender sets initial TTL value (OS-dependent default)
2. Each router decrements TTL by 1 before forwarding
3. When TTL = 0, router drops packet and sends [[ICMP]] Time Exceeded (Type 11)
4. Destination receives packet if TTL never reaches 0

### Visual Example
```
[Source] TTL=64 → [Router 1] TTL=63 → [Router 2] TTL=62 → [Destination] TTL=61
                   (-1)                (-1)                (received)
```

### TTL Expiration
```
[Source] TTL=2 → [Router 1] TTL=1 → [Router 2] TTL=0 → DROPPED!
                  (-1)               (-1)              ↓
                                                ICMP Time Exceeded
                                                sent back to source
```

## 📊 Default TTL by Operating System

| Operating System | Default TTL |
|------------------|-------------|
| Linux | 64 |
| Windows | 128 |
| macOS | 64 |
| Cisco IOS | 255 |
| Solaris | 255 |

> 💡 **OS Fingerprinting:** Observing TTL values in responses can hint at the target's operating system.

## 🔬 TTL in Action

### Calculating Distance
```
Received TTL: 52
Likely original: 64 (Linux/macOS)
Hops traversed: 64 - 52 = 12 hops
```

### Setting Custom TTL
```bash
# Linux ping with custom TTL
ping -t 10 target.com

# Windows ping with custom TTL
ping -i 10 target.com
```

## 🛡️ Detection & Prevention

### Security Implications
- **Traceroute Detection:** Low or incrementing TTLs indicate traceroute activity
- **Firewall Evasion:** Attackers may manipulate TTL to confuse IDS
- **OS Fingerprinting:** TTL values reveal operating system

### How to Detect
- Monitor for packets with unusually low TTL values
- Alert on patterns of TTL=1, 2, 3... (traceroute signature)
- Log ICMP Time Exceeded messages

### How to Prevent / Mitigate
- Normalize TTL values at network perimeter (set to fixed value)
- Block low-TTL packets at firewall
- Disable ICMP Time Exceeded responses on routers

## 🎤 Interview Angles

### Common Questions
- "What does TTL stand for and what does it actually measure?"
  - Time To Live; despite the name, it measures hop count, not time.
- "How does traceroute use TTL?"
  - Sends packets with incrementing TTL (1, 2, 3...) to trigger ICMP Time Exceeded from each hop.
- "How can you estimate how many hops away a target is?"
  - Subtract received TTL from likely original (64, 128, or 255).

### STAR Story
> **Situation:** Investigating anomalous traffic during a pentest debrief.
> **Task:** Determine why certain scan packets were detected.
> **Action:** Analyzed packet captures; noticed TTL values of 1-15 in logs—signature of traceroute activity from scanning tool.
> **Result:** Documented detection method; recommended disabling traceroute in future scans when stealth is required.

## ✅ Best Practices
- Understand default TTLs for OS fingerprinting during recon
- Use TTL observations to estimate network distance
- Consider TTL manipulation detection in IDS rules

## ❌ Common Misconceptions
- "TTL is measured in time" — It's actually a hop count
- "TTL is only for ping" — It's in every IP packet header

## 🔗 Related Concepts
- [[ICMP]]
- [[Ping]]
- [[Traceroute]]
- [[Active Reconnaissance]]

## 📚 References
- RFC 791 - Internet Protocol (TTL definition)
- TryHackMe - Active Reconnaissance Room
