---
dg-publish: true
tags:
  - "#cybersecurity/attacks/delivery"
  - "#cybersecurity/web-security"
  - "#interview/concepts"
aliases:
  - Malicious Advertising
---

# Malvertising

> **One-liner:** The use of legitimate online advertising networks to distribute malware to unsuspecting users.

## 🎯 What Is It?
Malvertising (malicious advertising) is an attack vector in the **Delivery** stage of the [[Cyber Kill Chain]] where attackers inject malicious code into legitimate advertising networks. Victims can be infected simply by viewing a webpage with a malicious ad—no clicking required (drive-by download).

## 🔬 How It Works

```
Attacker                  Ad Network              Legitimate Site
   │                          │                         │
   ├──Submits malicious ad───►│                         │
   │                          ├──Serves ad to site─────►│
   │                          │                         ├──User visits site
   │                          │                         │
   │◄─────────────────────────┼─────────────────────────┤
   │         Malicious ad loads in user's browser       │
   │                          │                         │
   └──Redirects to exploit kit / delivers payload──────►│
```

### Attack Types

| Type | Description | User Action Required |
|------|-------------|---------------------|
| **Drive-by Download** | Exploit executes automatically via [[Exploit Kit]] | None |
| **Click-based** | Redirects to malicious site on click | Click required |
| **Fake Alerts** | Displays fake virus warnings | User must interact |
| **Forced Redirect** | Auto-redirects to malicious page | None |

## 📊 Why It's Effective
- **Trust:** Ads appear on legitimate, trusted websites
- **Scale:** One malicious ad can reach millions of users
- **Evasion:** Difficult to detect; ads rotate constantly
- **Targeting:** Ad networks allow geographic/demographic targeting

## 🛡️ Detection & Prevention

### How to Detect
- Unusual browser redirects
- Unexpected downloads initiated from ad frames
- Network traffic to known malicious domains
- IDS/IPS signatures for exploit kit traffic

### How to Prevent / Mitigate
**For Users:**
- Use ad blockers (uBlock Origin, etc.)
- Keep browser and plugins patched
- Enable click-to-play for plugins
- Use browser sandboxing

**For Organizations:**
- Web filtering / proxy inspection
- DNS filtering for malicious domains
- Endpoint detection for exploit kit behavior
- User awareness training

**For Website Owners:**
- Vet advertising partners carefully
- Use Content Security Policy (CSP) headers
- Monitor third-party scripts

## 🎤 Interview Angles

### Common Questions
- "What is malvertising and how does it differ from regular phishing?"
- "How would you detect malvertising on your network?"
- "What defenses can mitigate malvertising attacks?"

### Key Talking Points
- No user interaction may be required (drive-by)
- Attacks legitimate sites with high traffic
- Ad blockers are a surprisingly effective security control

## ✅ Best Practices
- Layer defenses: ad blocking + patching + web filtering
- Treat third-party scripts as untrusted code
- Monitor for unauthorized redirects
- Implement CSP to control script sources

## 🔗 Related Concepts
- [[Cyber Kill Chain]]
- [[Exploit Kit]]
- [[Phishing]]
- [[Drive-by Download]]
- [[Web Application Firewalls (WAFS)]]

## 📚 References
- MITRE ATT&CK - Drive-by Compromise (T1189)
- Google Safe Browsing
