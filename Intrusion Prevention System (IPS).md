---
tags:
  - "#cybersecurity/blue-team/defense"
  - "#cybersecurity/networking"
  - "#interview/concepts"
aliases:
  - IPS
  - IDPS
  - Intrusion Detection and Prevention
---

# Intrusion Prevention System (IPS)

> **One-liner:** A network security device that monitors traffic for malicious activity and actively blocks or prevents detected threats.

## 🎯 What Is It?
An Intrusion Prevention System (IPS) is an inline security device that inspects network traffic, detects threats, and takes automatic action to block malicious activity. IPS extends Intrusion Detection Systems (IDS) by adding prevention capabilities. It's a key countermeasure in the **Exploitation** stage of the [[Cyber Kill Chain]].

## 🔬 How It Works

### IDS vs IPS

```
IDS (Detection Only):
┌─────────┐     ┌─────────┐     ┌─────────┐
│ Traffic │────►│  IDS    │     │ Target  │
│         │  │  │(monitor)│     │         │
└─────────┘  │  └────┬────┘     └─────────┘
             │       │
             │       ▼
             │    Alert!
             └──────────────────►(traffic still passes)

IPS (Prevention):
┌─────────┐     ┌─────────┐     ┌─────────┐
│ Traffic │────►│  IPS    │────►│ Target  │
│         │     │(inline) │     │         │
└─────────┘     └────┬────┘     └─────────┘
                     │
                     ▼
              Block + Alert
              (malicious traffic dropped)
```

### Detection Methods

| Method | Description | Pros/Cons |
|--------|-------------|-----------|
| **Signature-based** | Match known attack patterns | Fast, low FP; misses 0-days |
| **Anomaly-based** | Detect deviation from baseline | Catches novel attacks; more FP |
| **Policy-based** | Enforce traffic rules | Precise control; requires tuning |
| **Heuristic** | Behavioral analysis | Good for variants; complex |

### IPS Deployment Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| **Inline** | Traffic flows through IPS | Production blocking |
| **Tap/SPAN** | Copy of traffic (IDS mode) | Monitoring only |
| **Hybrid** | Both inline and monitoring | Staged deployment |

## 📊 IPS Categories

| Type | Location | Protects |
|------|----------|----------|
| **Network IPS (NIPS)** | Network perimeter | All network traffic |
| **Host IPS (HIPS)** | Individual endpoint | Single host |
| **Wireless IPS (WIPS)** | Wireless network | WiFi attacks |
| **NBA (Network Behavior)** | Network wide | Anomalous patterns |

### Common IPS Solutions

| Solution | Type | Notes |
|----------|------|-------|
| **Snort** | Open Source | Signature-based, widely used |
| **Suricata** | Open Source | Multi-threaded, protocol parsing |
| **Palo Alto NGFW** | Commercial | Next-gen with App-ID |
| **Cisco Firepower** | Commercial | Integrated with ASA |
| **OSSEC** | Open Source | Host-based IPS |

## 🛡️ Detection Capabilities

### What IPS Can Detect/Block
- Known exploits and malware signatures
- SQL injection, XSS, command injection
- Port scans and reconnaissance
- Protocol anomalies
- Buffer overflow attempts
- [[Command and Control (C2)]] traffic

### Example Snort Rule
```
# Detect SQL injection attempt
alert tcp any any -> any 80 (
    msg:"SQL Injection Attempt";
    content:"UNION SELECT";
    nocase;
    sid:1000001;
    rev:1;
)
```

## 🎤 Interview Angles

### Common Questions
- "What's the difference between IDS and IPS?"
- "What are the pros and cons of signature vs anomaly detection?"
- "How would you handle false positives in an IPS?"

### Key Talking Points
- IPS is inline and can block; IDS only monitors
- Signature-based catches known threats; anomaly catches novel
- False positives can disrupt business—tuning is critical
- Often integrated into Next-Gen Firewalls (NGFW)

### STAR Story
> **Situation:** Frequent exploitation attempts against public-facing web servers.
> **Task:** Implement real-time protection without disrupting legitimate traffic.
> **Action:** Deployed IPS inline with custom rules for application-specific attacks. Started in detection mode, analyzed alerts for a week, tuned rules to eliminate false positives, then enabled blocking.
> **Result:** Blocked 500+ exploitation attempts weekly with zero false positive blocks. Reduced successful attacks to zero.

## ✅ Best Practices
- Deploy in detection mode first, then enable prevention
- Regularly update signature databases
- Tune for your environment to reduce false positives
- Log all alerts for forensic analysis
- Combine with other defenses (WAF, EDR, SIEM)
- Monitor IPS health to prevent bypass via failure

## ❌ Common Pitfalls
- Blocking legitimate traffic (false positives)
- Failing open during high load (bypass)
- Not updating signatures
- Ignoring encrypted traffic

## 🔗 Related Concepts
- [[Cyber Kill Chain]]
- [[Web Application Firewalls (WAFS)]]
- [[Endpoint detection and response (EDR)]]
- [[Security Information and Event Management system (SIEM)]]
- [[Zero-Day Exploit]]

## 📚 References
- NIST SP 800-94 (Guide to IDPS)
- Snort Documentation
- Suricata User Guide
