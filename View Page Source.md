---
tags:
  - "#cybersecurity/web-security/recon"
  - "#cybersecurity/osint"
aliases:
  - View Source
  - HTML Source Inspection
---

# View Page Source

> **One-liner:** A basic recon technique where you inspect a webpage’s raw HTML/JS/CSS to find hidden clues, endpoints, comments, or leaked secrets.

## 🎯 What Is It?
“View page source” (or inspecting the DOM via DevTools) is a quick way to see what a site actually serves to clients. While it won’t reveal server-side code, it can expose:
- Hidden links/paths
- JS bundle names and API endpoints
- Comments and TODOs
- Client-side config values

## 🤔 Why It Matters
- **CTFs/OSINT:** Common place to hide hints, usernames, or credentials.
- **Recon:** Helps map client-side routes and API calls.
- **Security:** Secrets or sensitive identifiers sometimes leak into HTML/JS.

## 🔬 How It Works
### Core Principles
1. Browsers receive HTML/JS/CSS and render it.
2. Anything delivered to the browser can be inspected.
3. Minification/obfuscation slows analysis but doesn’t prevent it.

### Technical Deep-Dive
Practical checks:
- Search for `api`, `token`, `key`, `admin`, `wp-`, `robots.txt`
- Look for script tags and source maps
- Identify forms and endpoints

## 🛡️ Detection & Prevention
### How to Detect
- Monitor for unusual scraping/automation patterns (rate, headers, IPs).
- Detect accidental secret exposure with CI scanning (secret scanners) on web assets.

### How to Prevent / Mitigate
- Never ship secrets to the client (API keys, passwords, private endpoints).
- Use environment-specific configs server-side.
- Review build artifacts for leaked config.

## 📊 Types/Categories
| Type | Description | Example |
|------|-------------|---------|
| **HTML source** | Raw HTML delivered | Comments, hidden links |
| **DevTools/DOM** | Rendered DOM + runtime state | JS variables, network calls |
| **Network tab** | Observed HTTP requests | API endpoints, params |

## 🎤 Interview Angles
### Common Questions
- "What kind of information can you get from client-side source?"
- "Why is security through obscurity insufficient?"

### STAR Story
> **Situation:** A client suspected data exposure.
> **Task:** Validate whether sensitive data was shipped to browsers.
> **Action:** Inspected served JS/HTML and identified leaked config values.
> **Result:** Removed client-side secrets and added CI checks.

## ✅ Best Practices
- Treat front-end assets as public.
- Add secret-scanning to build pipelines.

## ❌ Common Misconceptions
- "Minified JS is safe because it’s hard to read" (it’s still inspectable).

## 🔗 Related Concepts
- [[WordPress]]
- [[Open Source Intelligence (OSINT)]]

## 📚 References
- OWASP Testing Guide (general web recon)
