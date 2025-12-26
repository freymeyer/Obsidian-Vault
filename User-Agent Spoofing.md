---
tags:
  - "#cybersecurity/web-security/techniques"
  - "#cybersecurity/red-team/evasion"
  - "#interview/concepts"
aliases:
  - User-Agent Bypass
  - UA Spoofing
---

# User-Agent Spoofing

> **One-liner:** The technique of modifying the User-Agent HTTP header to impersonate a different client or bypass security controls.

## 🎯 What Is It?
The User-Agent header in [[HTTP Request]]s identifies the client software (browser, bot, tool) making the request. User-Agent spoofing is changing this header to appear as a different client—either to bypass restrictions, evade detection, or access content served differently based on the client type.

## 🤔 Why It Matters
- Some applications block known security tools by User-Agent
- Attackers use spoofing to evade WAF detection
- Helps access mobile-only or desktop-only content
- Understanding spoofing reveals the weakness of client-side trust

## 🔬 How It Works

### Default User-Agent Examples
```
# cURL default
User-Agent: curl/7.81.0

# Python requests
User-Agent: python-requests/2.28.0

# Chrome browser
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
```

### Spoofing with cURL
```bash
# Default request (may be blocked)
curl http://target.com/api

# Spoof as Chrome browser
curl -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/120.0.0.0" http://target.com/api

# Custom/internal User-Agent
curl -A "internalcomputer" http://target.com/admin

# Empty User-Agent
curl -A "" http://target.com/api
```

### Spoofing with Python
```python
import requests

headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/120.0.0.0'
}
response = requests.get('http://target.com', headers=headers)
```

## 📊 Common Spoofing Scenarios

| Scenario | Original UA | Spoofed UA | Purpose |
|----------|-------------|------------|---------|
| Bypass tool blocking | `curl/7.x` | Chrome/Firefox UA | Access blocked endpoint |
| Internal access | Browser UA | `internalscanner` | Access internal resources |
| Mobile content | Desktop UA | Mobile UA | Access mobile version |
| Bot evasion | `python-requests` | Real browser UA | Avoid bot detection |
| WAF bypass | Known tool UA | Legitimate browser | Evade security rules |

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)
- Look for mismatches (Chrome UA but HTTP/1.0, or missing typical headers)
- Correlate User-Agent with behavioral patterns
- Track User-Agent changes within same session
- Monitor for known tool UAs in logs (sqlmap, nikto, etc.)
- Use browser fingerprinting beyond just User-Agent

### How to Prevent / Mitigate
- **Never rely solely on User-Agent for security decisions**
- Implement behavioral analysis, not just header checks
- Use CAPTCHA or JavaScript challenges for suspicious patterns
- Combine multiple signals (headers, timing, TLS fingerprint)
- Rate limit regardless of claimed User-Agent

## 🚨 Security Implications

| Risk | Description |
|------|-------------|
| **False sense of security** | Blocking by UA alone is easily bypassed |
| **Evasion of logging** | Attackers hide tool signatures |
| **Access control bypass** | Weak controls based on UA |
| **Content differential attacks** | Accessing hidden content paths |

## 🎤 Interview Angles

### Common Questions
- "What is User-Agent spoofing and why does it work?"
- "How would you detect if someone is spoofing their User-Agent?"
- "Why shouldn't you rely on User-Agent for security?"

### STAR Story
> **Situation:** A web application blocked cURL and other security tools based on User-Agent header.
> **Task:** Demonstrate that this security control was insufficient.
> **Action:** Used cURL with `-A` flag to spoof a standard browser User-Agent, bypassing the block. Documented additional indicators (missing headers, request patterns) that could be used for better detection.
> **Result:** Client implemented behavioral-based detection instead of User-Agent blocking, improving actual security posture.

## ✅ Best Practices
- Treat User-Agent as informational, never authoritative
- Log User-Agent for forensics but don't trust it
- Implement defense-in-depth (multiple layers of validation)
- Use TLS fingerprinting (JA3/JA4) for additional client identification
- Consider commercial bot detection solutions for critical applications

## ❌ Common Misconceptions
- "Blocking cURL User-Agent stops automated attacks" — Easily spoofed
- "User-Agent accurately identifies the client" — Trivially modified
- "Only attackers spoof User-Agent" — Legitimate uses exist (accessibility, testing)

## 🔗 Related Concepts
- [[HTTP Request]]
- [[cURL]]
- [[Web Application Firewall (WAF)]]
- [[Evasion Techniques]]

## 📚 References
- [MDN User-Agent Header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent)
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
