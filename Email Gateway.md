---
dg-publish: true
tags:
  - "#cybersecurity/blue-team/prevention"
  - "#networking/email"
  - "#interview/concepts"
aliases:
  - Mail Gateway
  - Secure Email Gateway
  - SEG
---

# Email Gateway

> **One-liner:** A security appliance that filters incoming and outgoing emails to block malicious content, [[Spam]], and [[Phishing]] attempts before they reach user inboxes.

## 🎯 What Is It?
An Email Gateway (also called Secure Email Gateway or SEG) is a security solution that sits between the internet and an organization's mail server to inspect, filter, and control email traffic. It acts as the first line of defense against email-based threats by analyzing messages for malicious content, [[Spam]], [[Phishing]] links, malicious attachments, and policy violations before delivering emails to recipients.

## 🤔 Why It Matters
- **Primary attack vector** — Email is the #1 initial access vector in breaches
- **Prevents [[Phishing]]** — Blocks credential theft and [[Social Engineering]]
- **Stops [[Malware]]** — Detects malicious attachments before execution
- **Reduces [[Spam]]** — Filters junk mail, improving productivity
- **[[Data Loss Prevention (DLP)]]** — Prevents sensitive data from leaving via email
- **Compliance** — Enforces retention, encryption, and regulatory requirements

## 🔬 How It Works

### Email Flow with Gateway
```
1. External sender → Email sent
2. MX record points to Email Gateway (not internal mail server)
3. Gateway receives email
4. Gateway analyzes:
   - Sender reputation (SPF, DKIM, DMARC)
   - Domain reputation (blocklists, threat intel)
   - Attachment scanning (sandboxing, AV)
   - URL analysis (malicious links)
   - Content filtering (spam scoring)
5. Decision:
   - ALLOW → Forward to internal mail server
   - QUARANTINE → Hold for admin review
   - BLOCK → Reject/drop email
6. User receives (or doesn't receive) email
7. Gateway logs event for SIEM
```

### Detection Mechanisms

| Mechanism | Purpose |
|-----------|---------|
| **SPF/DKIM/DMARC** | Validate sender authenticity |
| **Reputation filtering** | Block known malicious senders |
| **Content analysis** | Spam scoring, keyword detection |
| **Attachment sandboxing** | Detonate files in isolated environment |
| **URL rewriting** | Proxy/analyze clicks on links |
| **[[Threat intelligence]]** | Compare against IOC feeds |
| **Machine learning** | Detect anomalous email patterns |
| **Impersonation detection** | Catch CEO fraud/[[Business Email Compromise]]|

## 🛡️ Detection & Prevention

### Domain Blocking via Threat Intelligence
Email Gateways use blocklists of malicious domains to prevent known threat actors from reaching users:

```
# Example blocklist entry
evil-phish.com → BLOCK
malware-c2.net → BLOCK
spam-sender.info → QUARANTINE
```

### Key Filtering Rules
| Rule Type | Example | Action |
|-----------|---------|--------|
| Sender domain block | `*.evil.com` | Reject |
| Attachment type block | `.exe`, `.js`, `.hta` | Strip/Block |
| Keyword detection | "urgent payment", "verify account" | Quarantine |
| External sender warning | Email from outside org | Add banner |
| [[Data Loss Prevention (DLP)]] | Credit card numbers in body | Block outbound |

### SIEM Integration
```kql
# Kibana/Elastic - Blocked emails by domain
source.type:"email_gateway" AND action:"blocked"
| stats count by email.from.domain
| sort -count

# Splunk - Quarantined phishing attempts
index=mail_gateway action=quarantine category=phishing
| stats count by src_domain, subject
```

## ⚔️ Attack Scenarios Email Gateways Prevent

### 1. [[Phishing]] Campaign
- Attacker sends mass phishing emails
- Email Gateway checks sender domain against [[Threat intelligence]] feeds
- Domain `phishing-site[.]com` flagged as malicious
- **Gateway blocks all emails from domain**
- Users never see phishing attempt

### 2. [[Malware]] Delivery
- Attacker sends `.docm` with malicious macro
- Email Gateway sandboxes attachment
- Macro executes in sandbox, attempts to download payload
- **Gateway detects malicious behavior, blocks email**
- Attachment never reaches user

### 3. [[Business Email Compromise]]
- Attacker spoofs CEO email
- Email Gateway checks DMARC authentication
- Email fails DMARC (not from legitimate server)
- **Gateway quarantines suspicious email**
- Admin reviews before delivery

## 🎤 Interview Angles

### Common Questions
- "How does an email gateway differ from an antivirus?"
- "What is the difference between blocking and quarantining?"
- "How do email gateways use threat intelligence?"
- "What are the limitations of email gateways?"

### Key Talking Points
- Email gateways are **proactive** (filter before inbox) vs antivirus **reactive** (scan after delivery)
- Must balance **security vs usability** (false positives annoy users)
- Requires **continuous updates** of threat intelligence feeds
- **Cannot protect against** zero-day attacks without sandboxing
- **Best combined with** user training and [[Multi-Factor Authentication (MFA)]]

### STAR Story
> **Situation:** Organization receiving 500+ phishing emails per day, 15 users clicked malicious links monthly.
> **Task:** Deploy email gateway to reduce phishing exposure and improve email security posture.
> **Action:** Implemented Proofpoint Email Gateway with threat intelligence integration. Configured domain blocking based on abuse.ch and Spamhaus feeds. Enabled attachment sandboxing for all external attachments. Added external sender warning banners. Trained SOC team to review quarantine daily.
> **Result:** Blocked 98% of phishing attempts (490/500 emails). Reduced successful phishing clicks from 15/month to 1/month. Prevented 3 ransomware infections in first quarter via attachment sandboxing.

## ✅ Best Practices
- Integrate [[Cyber Threat Intelligence (CTI)|threat intelligence]] feeds for domain/IP blocklists
- Enable attachment sandboxing for unknown files
- Use DMARC enforcement (not just monitoring)
- Add external email warnings to all untrusted senders
- Regularly review quarantine for false positives
- Log all email events to [[Security Information and Event Management system (SIEM)|SIEM]]
- Conduct phishing simulations to test effectiveness
- Keep gateway updated with latest signatures

## ❌ Common Misconceptions
- "Email gateway = no more phishing" — Users can still be tricked by sophisticated attacks
- "Set and forget" — Requires tuning and threat feed updates
- "Blocks all malware" — Zero-day attacks may bypass signature-based detection
- "Gateway replaces user training" — Both are needed for defense-in-depth

## 🆚 Comparison with Similar Controls

| Control | Function | Timing |
|---------|----------|--------|
| **Email Gateway** | Block malicious emails | Before inbox |
| **Antivirus** | Scan attachments | After download |
| **[[Endpoint detection and response (EDR)]]** | Detect malicious execution | After execution |
| **[[Security Awareness Training]]** | Train users | Preventative |
| **[[Multi-Factor Authentication (MFA)]]** | Protect compromised credentials | At login |

## 🔗 Related Concepts
- [[Phishing]]
- [[Spam]]
- [[Business Email Compromise]]
- [[Malware]]
- [[Data Loss Prevention (DLP)]]
- [[Threat intelligence]]
- [[Indicator of Compromise (IOC)]]
- [[Simple Mail Transfer Protocol (SMTP)]]
- [[Social Engineering]]

## 📚 References
- Gartner Market Guide for Email Security
- MITRE ATT&CK: T1566 (Phishing)
- SANS: Email Security Best Practices
- Proofpoint Email Fraud Defense
- Microsoft Defender for Office 365
