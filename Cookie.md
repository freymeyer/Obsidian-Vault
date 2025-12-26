---
tags:
  - "#cybersecurity/web-security/fundamentals"
  - "#interview/concepts"
aliases:
  - Cookies
  - HTTP Cookie
  - Web Cookie
---

# Cookie

> **One-liner:** Small pieces of data stored by the browser and sent with every request to maintain state in the stateless HTTP protocol.

## 🎯 What Is It?
Cookies are key-value pairs that web servers send to browsers via the `Set-Cookie` header. The browser stores them and automatically includes them in subsequent requests to the same domain. They're the primary mechanism for maintaining [[Web Session]] state, user preferences, and tracking.

## 🤔 Why It Matters
- Session management relies on cookies (e.g., `PHPSESSID`, `JSESSIONID`)
- Cookie theft leads to session hijacking
- Misconfigured cookies are a common vulnerability
- Understanding cookies is essential for web security testing

## 🔬 How It Works

### Cookie Lifecycle
```
1. User logs in → Server validates credentials
2. Server creates session → Sends Set-Cookie header
3. Browser stores cookie → Sends it with every request
4. Server reads cookie → Identifies user session
5. User logs out → Cookie destroyed/invalidated
```

### Setting Cookies (Server Response)
```http
HTTP/1.1 200 OK
Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Strict; Path=/; Max-Age=3600
```

### Sending Cookies (Client Request)
```http
GET /dashboard HTTP/1.1
Host: example.com
Cookie: session=abc123; theme=dark
```

## 📊 Cookie Attributes

| Attribute | Purpose | Security Impact |
|-----------|---------|-----------------|
| `HttpOnly` | Block JavaScript access | Prevents XSS cookie theft |
| `Secure` | HTTPS only | Prevents interception over HTTP |
| `SameSite` | Cross-site request control | CSRF protection |
| `Path` | URL path scope | Limits cookie exposure |
| `Domain` | Domain scope | Controls subdomain access |
| `Expires/Max-Age` | Lifetime | Session vs persistent |

### SameSite Values

| Value | Behavior | Use Case |
|-------|----------|----------|
| `Strict` | Never sent cross-site | Highest security |
| `Lax` | Sent on top-level navigation | Default (Chrome) |
| `None` | Always sent (requires Secure) | Third-party/embedded |

## 🚨 Cookie Security Vulnerabilities

| Vulnerability | Cause | Impact |
|---------------|-------|--------|
| **Session Hijacking** | Cookie stolen via [[Cross-Site Scripting (XSS)]] | Account takeover |
| **Session Fixation** | Attacker sets victim's session ID | Account takeover |
| **Cookie Replay** | Reusing captured cookie | Unauthorized access |
| **[[Cross-site request forgery (CSRF)]]** | Missing SameSite | Unauthorized actions |
| **Insecure Transmission** | Missing Secure flag | Cookie interception |

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)
- Monitor for unusual session cookie usage patterns
- Detect session IDs used from different IPs/locations
- Log cookie-related security header misconfigurations
- Track session anomalies (rapid reuse, geographic impossibilities)

### How to Prevent / Mitigate
- Always set `HttpOnly` on session cookies
- Always set `Secure` for production
- Use `SameSite=Strict` or `SameSite=Lax`
- Regenerate session IDs after login
- Implement session timeout and idle logout

## 🔧 Testing Cookies with cURL

**Save cookies after authentication:**
```bash
curl -c cookies.txt -d "user=admin&pass=admin" https://example.com/login
```

**Use saved cookies:**
```bash
curl -b cookies.txt https://example.com/dashboard
```

**View Set-Cookie headers:**
```bash
curl -i https://example.com/login
```

## 🎤 Interview Angles

### Common Questions
- "What's the difference between HttpOnly and Secure flags?"
- "How do cookies maintain session state?"
- "Explain how an XSS attack can steal cookies"
- "What is SameSite and how does it prevent CSRF?"

### STAR Story
> **Situation:** A web application's session cookies were missing security flags, flagged during a security audit.
> **Task:** Assess the risk and implement proper cookie security.
> **Action:** Demonstrated how XSS could steal session cookies due to missing HttpOnly. Implemented HttpOnly, Secure, and SameSite=Strict on all session cookies.
> **Result:** Eliminated session hijacking risk via XSS. Application passed subsequent penetration test with no cookie-related findings.

## ✅ Best Practices
- Use cryptographically random session IDs
- Set appropriate expiration times
- Bind sessions to additional factors (IP, User-Agent) cautiously
- Implement secure logout (server-side session invalidation)
- Use cookie prefixes (`__Secure-`, `__Host-`) for extra protection

## 🔗 Related Concepts
- [[Web Session]]
- [[HTTP Request]]
- [[Cross-Site Scripting (XSS)]]
- [[Cross-site request forgery (CSRF)]]
- [[cURL]]
- [[Session Hijacking]]

## 📚 References
- [MDN HTTP Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
