---
tags:
  - "#cybersecurity/attacks/social-engineering"
  - "#cybersecurity/attacks/phishing"
  - "#interview/concepts"
aliases:
  - SMS Phishing
---

# Smishing

> **One-liner:** A phishing attack delivered via SMS text messages to trick victims into clicking malicious links or revealing sensitive information.

## 🎯 What Is It?
Smishing (SMS + Phishing) is a [[Social Engineering]] attack that uses text messages as the delivery mechanism. It's part of the **Delivery** stage of the [[Cyber Kill Chain]]. Attackers send fraudulent SMS messages impersonating trusted entities to steal credentials, install malware, or trick victims into transferring money.

## 🔬 How It Works

```
1. Attacker sends SMS from spoofed number/short code
   └── "Your bank account has been locked. Verify: https://bit.ly/xyz"

2. Victim clicks link
   └── Redirected to fake login page OR
   └── Malware download initiated

3. Credential harvesting / malware execution
   └── Attacker captures credentials
   └── Mobile malware establishes [[Persistence (Cyber Security)|persistence]]
```

### Common Smishing Lures

| Category | Example Message |
|----------|-----------------|
| **Banking** | "Unusual activity detected. Verify your account: [link]" |
| **Delivery** | "Your package is pending. Update delivery: [link]" |
| **Tax/Government** | "IRS refund available. Claim now: [link]" |
| **Tech Support** | "Your Apple ID was compromised. Secure it: [link]" |
| **COVID/Health** | "Vaccine appointment available. Book: [link]" |
| **Prize/Gift** | "You've won a $500 gift card. Claim: [link]" |

## 📊 Why It's Effective
- **Trust:** SMS feels more personal/urgent than email
- **Limited Inspection:** Harder to verify sender on mobile
- **No Spam Filters:** Less protection than email
- **Shortened URLs:** Obscure the true destination
- **Mobile Context:** Users are often distracted, rushed

## 🛡️ Detection & Prevention

### How to Detect
- Unexpected messages from unknown numbers
- Urgency language ("act now," "account locked")
- Shortened or suspicious URLs
- Requests for personal information via text

### How to Prevent / Mitigate
**For Users:**
- Never click links in unexpected SMS messages
- Verify by contacting the organization directly
- Don't reply with personal information
- Report smishing to carrier (forward to 7726/SPAM)

**For Organizations:**
- User awareness training
- Mobile Device Management (MDM) with web filtering
- Enable multi-factor authentication (reduces credential theft impact)
- Monitor for brand impersonation

## 🎤 Interview Angles

### Common Questions
- "What is smishing and how does it differ from phishing?"
- "How would you protect an organization's employees from smishing?"
- "Why might smishing be more effective than email phishing?"

### Key Talking Points
- Mobile users are more vulnerable due to smaller screens and fewer security indicators
- SMS lacks the security controls of email (SPF, DKIM, DMARC)
- Two-factor SMS codes can be intercepted via SIM swapping

## ✅ Best Practices
- Treat unsolicited SMS like unsolicited email—verify independently
- Use authenticator apps instead of SMS for 2FA
- Enable carrier-level spam filtering
- Report suspicious messages

## 🔗 Related Concepts
- [[Phishing]]
- [[Social Engineering]]
- [[Cyber Kill Chain]]
- [[Vishing]] (voice phishing)

## 📚 References
- FTC Consumer Advice on Smishing
- CISA Mobile Security Guidelines
