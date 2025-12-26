---
tags:
  - "#cybersecurity/red-team/tools"
  - "#infrastructure/networking/tools"
  - "#interview/concepts"
aliases:
  - curl
  - Client URL
---

# cURL

> **One-liner:** A command-line tool for transferring data using various protocols, primarily used for making HTTP requests from the terminal.

## 🎯 What Is It?
cURL (Client URL) is a powerful command-line utility for sending and receiving data using URL syntax. It supports a wide range of protocols including HTTP, HTTPS, FTP, and more. In security testing, cURL is invaluable when GUI tools aren't available or when precise control over requests is needed.

## 🤔 Why It Matters
- Available on almost every system (Linux, macOS, Windows)
- Allows raw inspection of [[HTTP Request]] and responses
- Essential for scripting and automation in security testing
- Foundation for understanding how tools like [[Burp Suite]], Hydra, and WFuzz work

## 🔬 How It Works

### Core Flags Reference

| Flag | Purpose | Example |
|------|---------|---------|
| `-X` | Specify HTTP method | `-X POST` |
| `-d` | Send data in body | `-d "user=admin"` |
| `-H` | Add custom header | `-H "Content-Type: application/json"` |
| `-A` | Set User-Agent | `-A "Mozilla/5.0"` |
| `-c` | Save cookies to file | `-c cookies.txt` |
| `-b` | Send cookies from file | `-b cookies.txt` |
| `-i` | Include response headers | `-i` |
| `-s` | Silent mode (no progress) | `-s` |
| `-L` | Follow redirects | `-L` |
| `-k` | Ignore SSL errors | `-k` |
| `-o` | Output to file | `-o response.html` |
| `-v` | Verbose output | `-v` |

### Common Usage Examples

**Basic GET Request:**
```bash
curl https://example.com
```

**POST Request with Form Data:**
```bash
curl -X POST -d "username=admin&password=secret" https://example.com/login
```

**POST Request with JSON:**
```bash
curl -X POST -H "Content-Type: application/json" \
     -d '{"user":"admin","pass":"secret"}' \
     https://example.com/api/login
```

**Using Cookies (Session Handling):**
```bash
# Save cookies after login
curl -c cookies.txt -d "user=admin&pass=admin" https://example.com/login

# Reuse cookies for authenticated request
curl -b cookies.txt https://example.com/dashboard
```

**Custom User-Agent (Bypass Filtering):**
```bash
curl -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64)" https://example.com
```

**View Response Headers:**
```bash
curl -i https://example.com
```

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)
- Monitor for `curl/*` or default cURL User-Agent strings in logs
- Look for automated request patterns (rapid requests, sequential parameter testing)
- Analyze unusual request headers or missing typical browser headers
- Check for scripted login attempts (same endpoint, varying credentials)

### How to Prevent / Mitigate
- Implement rate limiting on authentication endpoints
- Use CAPTCHAs for login forms
- Block or challenge suspicious User-Agents (while knowing they can be spoofed)
- Require additional authentication factors ([[Multi-Factor Authentication (MFA)]])

## 🔄 Security Testing Use Cases

| Use Case | Command Pattern |
|----------|-----------------|
| [[Brute-force]] login | Loop with `-d` varying passwords |
| Session testing | `-c` and `-b` for [[Cookie]] replay |
| [[User-Agent Spoofing]] | `-A` to bypass filters |
| API testing | `-H` for auth tokens, `-X` for methods |
| File download | `-o` or `-O` flags |

## 🎤 Interview Angles

### Common Questions
- "How would you test an API endpoint without a browser?"
- "Explain how you'd brute-force a login form using cURL"
- "What's the difference between `-d` and `-H` in cURL?"

### STAR Story
> **Situation:** During a penetration test, the client's web application blocked Burp Suite but the terminal was available.
> **Task:** Test the login functionality for brute-force vulnerabilities.
> **Action:** Used cURL with a bash loop to iterate through a password list, sending POST requests to the login endpoint and checking for success indicators in responses.
> **Result:** Identified weak password policy and demonstrated successful brute-force attack, leading to implementation of account lockout and rate limiting.

## ✅ Best Practices
- Always use `-s` (silent) in scripts to avoid progress meter noise
- Use `-L` to follow redirects when testing
- Save full responses with `-o` for later analysis
- Combine with `grep` to filter responses automatically

## 🔗 Related Concepts
- [[HTTP Request]]
- [[Brute-force]]
- [[Cookie]]
- [[Web Session]]
- [[User-Agent Spoofing]]

## 📚 References
- [cURL Documentation](https://curl.se/docs/)
- [cURL Man Page](https://curl.se/docs/manpage.html)
