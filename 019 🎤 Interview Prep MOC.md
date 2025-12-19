---
tags:
  - "#interview"
  - "#learning"
---

# 🎤 Interview Prep MOC

> Your centralized hub for cybersecurity interview preparation. Each section is designed for quick recall and confident delivery.

---

## 🚀 30-Second Definitions
*Master these for rapid-fire technical screening questions*

### Identity & Access
- [[Authentication]] — Proving who you are (username + password)
- [[Authorization]] — What you're allowed to do after authentication
- [[Identification, Authentication, Authorization, and Accountability (IAAA)]]

### Security Fundamentals
- [[CIA Triad]] — Confidentiality, Integrity, Availability
- [[Defense in Depth]] — Layered security controls
- [[Zero Trust]] — Never trust, always verify
- [[Principle of Least Privilege]]

### Attack & Defense
- [[Blue Teaming]] — Defensive security operations
- [[Red Teaming]] — Offensive security/adversary simulation
- [[Purple Team]] — Collaboration between red and blue

---

## 🌐 OWASP Top 10 (Must Know)
*Asked in virtually every web security interview*

| Rank | Vulnerability | One-Liner |
|------|--------------|-----------|
| A01 | [[Broken Access Control]] | Users act outside intended permissions |
| A02 | [[Cryptographic Failure]] | Sensitive data exposure via weak crypto |
| A03 | [[Injection]] | Untrusted data sent to interpreter |
| A04 | [[Insecure Design]] | Missing security controls in design phase |
| A05 | [[Security Misconfiguration]] | Default/incomplete configurations |
| A06 | [[Vulnerable Components]] | Using components with known vulns |
| A07 | [[Authentication Failures]] | Broken authentication mechanisms |
| A08 | [[Software and Data Integrity Failures]] | Code/data integrity not verified |
| A09 | [[Security Logging and Monitoring Failures]] | Insufficient logging for detection |
| A10 | [[Server-Side Request Forgery (SSRF)]] | Server makes requests to unintended locations |

---

## 🎯 STAR Stories Ready
*Behavioral questions: "Tell me about a time when..."*

### Technical Scenarios
- [[XSS]] → Finding and remediating stored XSS
- [[SQL Injection]] → Discovering SQLi in production app
- [[Phishing]] → Investigating a phishing campaign
- [[Incident Response]] → Walking through an IR scenario

### Soft Skills Angles
| Scenario Type | Note to Prepare |
|--------------|-----------------|
| Conflict Resolution | Disagreement on security priority |
| Communication | Explaining risk to non-technical stakeholders |
| Learning Quickly | Picking up a new tool/technology fast |
| Failure & Recovery | Missed alert, lessons learned |

---

## 🔥 Common Scenario Questions

### SOC/Blue Team
- "Walk me through investigating a phishing alert"
- "How would you respond to ransomware?"
- "What logs would you check for lateral movement?"
- "Explain your triage process for a security alert"

### Penetration Testing/Red Team
- "Describe your methodology for a web app pentest"
- "How would you escalate privileges on a Linux box?"
- "What's your approach to bypassing AV/EDR?"

### General Security
- "What's the difference between IDS and IPS?"
- "Explain encryption at rest vs in transit"
- "How does TLS/SSL work?"
- "What is the CIA triad and give examples of each"

---

## 🖼️ Whiteboard Concepts
*Be ready to draw and explain these*

- [[Kill Chain]] — 7 stages of an attack
- [[MITRE ATT&CK]] — Adversary tactics and techniques matrix
- [[Diamond Model]] — Adversary, capability, infrastructure, victim
- [[Zero Trust Architecture]] — Never trust, always verify model
- [[Defense in Depth]] — Layered security controls
- [[OSI Model]] — 7 layers of networking
- [[TCP Three-Way Handshake]] — SYN, SYN-ACK, ACK

---

## 🛠️ Tools You Should Know

### Blue Team
| Tool | Purpose | Note |
|------|---------|------|
| [[Wireshark]] | Packet analysis | |
| [[Splunk]] | SIEM/Log analysis | |
| [[Elastic]] / [[ELK Stack]] | SIEM stack | |
| [[YARA]] | Malware pattern matching | |

### Red Team
| Tool | Purpose | Note |
|------|---------|------|
| [[Nmap]] | Port scanning | |
| [[Burp Suite]] | Web app testing | |
| [[Metasploit]] | Exploitation framework | |
| [[Hydra]] | Password cracking | |

---

## 📝 Interview Day Checklist

- [ ] Review OWASP Top 10 one-liners
- [ ] Practice 3 STAR stories out loud
- [ ] Review the job description keywords
- [ ] Prepare questions to ask them
- [ ] Know your resume projects inside-out
- [ ] Review recent CVEs/breaches in the news

---

## 🔗 Related MOCs
- [[010 Cybersecurity MOC]]
- [[011 🛡️ Blue Team & SOC Operations MOC]]
- [[012 ⚔️ Red Team & Offensive Security MOC]]
- [[013 🌐 Web Application Security MOC]]
- [[018 🎓 Learning & Projects MOC]]
