---
dg-publish: true
tags:
  - "#cybersecurity/web-sec/tools"
  - "#cybersecurity/red-team/tools"
  - "#interview/tools"
aliases:
  - Burp
  - Burp Proxy
---

# Burp Suite

> **One-liner:** An integrated platform for web application security testing that intercepts, inspects, and modifies HTTP/S traffic.

## 🎯 What Is It?
Burp Suite is the industry-standard tool for web application penetration testing. It acts as an intercepting proxy between your browser and the target server, allowing you to capture, analyze, and manipulate HTTP requests and responses in real-time.

**Editions:**
- **Community (Free):** Core features, limited scanner
- **Professional:** Full scanner, advanced tools ($449/year)
- **Enterprise:** Automated scanning, CI/CD integration

## 🤔 Why It Matters

### For Red Team / Pentesters
- **Manual testing:** Intercept and modify requests to bypass security controls
- **Vulnerability discovery:** Find [[Broken Access Control]], [[SQL Injection]], [[Cross-Site Scripting (XSS)]]
- **Session analysis:** Test [[Authentication]] and [[Authorization]] mechanisms

### For Bug Bounty Hunters
- Industry-standard tool mentioned in most bug bounty programs
- Repeater for manual exploitation
- Intruder for fuzzing and brute-forcing

### For Blue Team / Defenders
- Understand attacker methodology
- Reproduce reported vulnerabilities
- Test security controls from attacker perspective

## 🔬 How It Works

### Core Architecture
```
Browser → Burp Proxy → Target Server
          ↓
     [Intercept & Modify]
          ↓
     Tools (Repeater, Intruder, Scanner)
```

### Workflow
1. **Configure proxy** in browser (typically localhost:8080)
2. **Browse target** application normally
3. **Intercept** traffic in Burp
4. **Modify** requests before forwarding
5. **Analyze** responses
6. **Send to tools** for further testing

## 🛠️ Key Components

| Component | Purpose | Use Case |
|-----------|---------|----------|
| **Proxy** | Intercepts HTTP/S traffic | Capture all requests/responses |
| **Repeater** | Manual request editing | Test individual requests repeatedly |
| **Intruder** | Automated fuzzing/payloads | [[Brute-force]], parameter tampering |
| **Scanner** | Automated vuln detection | Find common vulns (Pro only) |
| **Decoder** | Encode/decode data | Base64, URL encoding, hashing |
| **Comparer** | Diff tool for responses | Find subtle differences |
| **Sequencer** | Token randomness analysis | Test session token quality |
| **Extender** | Plugin marketplace | Extend functionality (BApps) |

## 📊 Common Testing Workflows

### 1. Testing Authentication
```
1. Intercept login request in Proxy
2. Send to Repeater
3. Test for:
   - [[Brute-force]] protection
   - [[SQL Injection]] in credentials
   - Response timing differences
   - Session fixation
```

### 2. Testing Authorization ([[IDOR]])
```
1. Capture request accessing resource (e.g., /api/user/123)
2. Send to Repeater
3. Change ID to another user's ID
4. Check if access is granted (IDOR vulnerability)
```

### 3. Fuzzing Parameters
```
1. Send request to Intruder
2. Mark parameter as payload position: /api/user/§123§
3. Load payload list (wordlist, numbers, XSS payloads)
4. Launch attack
5. Analyze responses for anomalies
```

### 4. Testing for [[Cross-Site Scripting (XSS)]]
```
1. Identify input field
2. Send request to Repeater
3. Insert XSS payload: <script>alert(1)</script>
4. Check if reflected in response unencoded
```

## 🔧 Essential Features

### Repeater
The most-used tool for manual testing:
```
• Modify any part of HTTP request
• Send repeatedly with different payloads
• Compare responses side-by-side
• Essential for:
  - [[Command Injection]]
  - [[SQL Injection]]
  - [[Authorization Bypass]]
  - [[Insecure Direct Object Reference (IDOR)]]
```

### Intruder (Attack Types)

| Attack Type | Description | Use Case |
|-------------|-------------|----------|
| **Sniper** | Single payload position, iterate | Parameter fuzzing |
| **Battering Ram** | Same payload in all positions | Password spray |
| **Pitchfork** | Parallel payloads | Username:password pairs |
| **Cluster Bomb** | All combinations | Brute-force multiple params |

### Proxy History & HTTP History
- View all requests/responses
- Search and filter
- Right-click → Send to Repeater/Intruder/Scanner
- Export for documentation

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)
- Monitor for [[User-Agent Spoofing|User-Agent]] headers: `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.130 Safari/537.36` (typical Burp traffic)
- Look for repeated requests with minor parameter variations
- Detect timing patterns (Intruder sends requests in bursts)
- Alert on requests with suspicious [[Cookie|cookies]] or tokens

### How to Prevent / Mitigate
- **Rate limiting:** Slow down Intruder attacks
- **[[Web Application Firewalls (WAFS)|WAF]] rules:** Block common Burp payloads
- **CAPTCHA:** Prevent automated testing (but can be bypassed)
- **Server-side validation:** Don't trust client input (defense in depth)
- **Logging & alerting:** Detect and respond to suspicious activity

## 🎤 Interview Angles

### Common Questions
- **"What is Burp Suite and why is it important?"**
  - *"Burp Suite is an intercepting proxy for web app testing. It's important because it lets you see and modify HTTP traffic in real-time, which is essential for finding vulnerabilities like IDOR, broken access control, and injection flaws."*

- **"Walk me through testing for an IDOR using Burp Suite"**
  - *"I'd intercept the request accessing a resource like /api/invoice/123, send it to Repeater, change the ID to another user's ID like /api/invoice/124, and see if the server returns data I shouldn't access. If it does, that's an [[Insecure Direct Object Reference (IDOR)|IDOR]] vulnerability."*

- **"What's the difference between Repeater and Intruder?"**
  - *"Repeater is for manual testing—you edit and resend requests one at a time. Intruder is for automated testing—you define payload positions and it sends hundreds of variations automatically, useful for fuzzing or brute-forcing."*

- **"Have you used any Burp extensions?"**
  - *"Yes, common ones include Autorize (for authorization testing), Logger++ (advanced logging), and Turbo Intruder (faster attacks). The BApp Store has hundreds of community extensions."*

### STAR Story
> **Situation:** During a web app pentest, the client's API returned `403 Forbidden` for all requests from security tools.
> **Task:** Bypass the security control to test the application for vulnerabilities.
> **Action:** Configured Burp Suite to intercept traffic, captured a legitimate browser request, copied all headers (including [[User-Agent Spoofing|User-Agent]] and `Accept` headers) into Repeater, and replayed the request. The server accepted it. Then tested for [[Broken Access Control]] by changing user IDs in API endpoints.
> **Result:** Discovered 5 critical [[Insecure Direct Object Reference (IDOR)|IDOR]] vulnerabilities allowing access to other users' PII. Documented findings with Burp screenshots and request/response pairs.

## ✅ Best Practices
- Always test with valid credentials in a legal scope
- Use **Target Scope** to avoid accidentally testing out-of-scope domains
- Save project files regularly (Burp can crash with large history)
- Use **Match & Replace** rules for automation (e.g., auto-add headers)
- Master keyboard shortcuts (Ctrl+R for Repeater, Ctrl+I for Intruder)
- Keep Burp updated for latest vulnerability checks

## ❌ Common Misconceptions
- **"Burp Scanner finds everything"** → Scanner is good but manual testing is essential
- **"You need Burp Pro to pentest"** → Community edition has Repeater, which is 80% of manual testing
- **"Burp is only for web apps"** → Can test APIs, mobile app backends, thick clients with HTTP
- **"Intercept mode should always be on"** → Turn off Intercept after capturing target request (workflow tip)

## 🔧 Quick Start Commands

### Set up browser proxy (FoxyProxy)
```
Proxy: localhost
Port: 8080
```

### Import Burp CA certificate
```
1. Browse to http://burp (with proxy on)
2. Download CA certificate
3. Install in browser (Firefox: Preferences > Certificates)
```

### Common Keyboard Shortcuts
| Shortcut | Action |
|----------|--------|
| `Ctrl+R` | Send to Repeater |
| `Ctrl+I` | Send to Intruder |
| `Ctrl+Shift+B` | Send to Scanner (Pro) |
| `Ctrl+F` | Search HTTP history |
| `Ctrl+Space` | Send intercepted request |

## 🔗 Related Concepts
- [[cURL]]
- [[User-Agent Spoofing]]
- [[Cross-Site Scripting (XSS)]]
- [[SQL Injection]]
- [[Insecure Direct Object Reference (IDOR)]]
- [[Broken Access Control]]
- [[Authorization Bypass]]
- [[Web Application Firewalls (WAFS)]]
- [[013 🌐 Web Application Security MOC]]
- [[012 ⚔️ Red Team & Offensive Security MOC]]

## 📚 References
- https://portswigger.net/burp
- PortSwigger Web Security Academy (free training)
- Burp Suite Certified Practitioner (BSCP) exam
- HackerOne/Bugcrowd writeups using Burp
