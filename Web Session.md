---
tags:
  - "#cybersecurity/web-security/fundamentals"
  - "#interview/concepts"
aliases:
  - Web Sessions
  - Session Management
  - HTTP Session
---

# Web Session

> **One-liner:** A mechanism to maintain user state across multiple HTTP requests by associating a unique identifier with server-side data.

## 🎯 What Is It?
Since [[HTTP Request]]s are stateless (each request is independent), web sessions provide a way to "remember" users across requests. After [[Authentication]], the server creates a session and gives the client a session identifier (usually in a [[Cookie]]). This ID is sent with every request, allowing the server to look up the user's session data.

## 🤔 Why It Matters
- Sessions are the backbone of authenticated web applications
- Session vulnerabilities lead to account takeover
- Understanding sessions is critical for web security testing
- Common target for attackers (session hijacking, fixation)

## 🔬 How It Works

### Session Flow
```
1. User submits credentials
2. Server verifies identity
3. Server creates session (stores user data server-side)
4. Server sends session ID to client (Set-Cookie: PHPSESSID=abc123)
5. Client includes session ID in subsequent requests
6. Server looks up session data using ID
7. User logs out → Session destroyed
```

### Diagram
```
┌─────────┐                        ┌─────────┐
│ Browser │                        │ Server  │
└────┬────┘                        └────┬────┘
     │ POST /login (user:pass)          │
     │─────────────────────────────────>│
     │                                  │ Create session
     │    Set-Cookie: session=xyz       │ Store user data
     │<─────────────────────────────────│
     │                                  │
     │ GET /dashboard                   │
     │ Cookie: session=xyz              │
     │─────────────────────────────────>│
     │                                  │ Lookup session
     │    200 OK (user's data)          │ Return user data
     │<─────────────────────────────────│
```

## 📊 Session Storage Types

| Type | Description | Pros | Cons |
|------|-------------|------|------|
| **Server-side** | Data stored on server, ID in cookie | Secure, controlled | Scalability, state |
| **Client-side (JWT)** | Data in signed token | Stateless, scalable | Token size, revocation |
| **Database** | Session in DB | Persistent, shared | Latency, DB load |
| **In-memory (Redis)** | Session in cache | Fast, scalable | Complexity |

## 🚨 Session Vulnerabilities

| Attack | Description | Prevention |
|--------|-------------|------------|
| **Session Hijacking** | Stealing session ID | HttpOnly cookies, HTTPS |
| **Session Fixation** | Forcing known session ID | Regenerate ID after login |
| **Session Prediction** | Guessing weak session IDs | Cryptographic randomness |
| **Session Replay** | Reusing captured session | Token binding, short expiry |
| **Insufficient Timeout** | Sessions live too long | Implement idle/absolute timeout |

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)
- Monitor for session IDs used from multiple IPs
- Detect impossible travel (same session, distant locations)
- Track session creation/destruction patterns
- Alert on session reuse after logout

### How to Prevent / Mitigate
- Regenerate session ID after authentication
- Use cryptographically secure random session IDs
- Implement both idle and absolute session timeouts
- Properly invalidate sessions on logout (server-side)
- Bind sessions to client fingerprint cautiously

## 🔧 Session Testing with cURL

**Capture session after login:**
```bash
curl -c session.txt -d "username=admin&password=admin" http://target.com/login
```

**Reuse session for authenticated request:**
```bash
curl -b session.txt http://target.com/profile
```

**Test session after logout:**
```bash
# After logging out, try reusing the old session
curl -b session.txt http://target.com/profile
# Should be redirected or get 401
```

## 🎤 Interview Angles

### Common Questions
- "How do web sessions work?"
- "What's the difference between session-based and token-based auth?"
- "Explain session hijacking and how to prevent it"
- "How would you test session management security?"

### STAR Story
> **Situation:** During a security assessment, discovered the application didn't regenerate session IDs after login.
> **Task:** Demonstrate the session fixation vulnerability and recommend remediation.
> **Action:** Showed how an attacker could set a victim's session ID before login, then hijack the authenticated session afterward. Recommended regenerating session ID post-authentication.
> **Result:** Development team implemented session regeneration, eliminating the fixation vulnerability.

## ✅ Best Practices
- Always regenerate session ID after authentication state change
- Use 128+ bit random session IDs
- Set appropriate session timeouts (idle: 15-30 min, absolute: 8-24 hrs)
- Invalidate session server-side on logout
- Use secure [[Cookie]] flags (HttpOnly, Secure, SameSite)

## 🔗 Related Concepts
- [[Cookie]]
- [[HTTP Request]]
- [[Authentication]]
- [[Session Hijacking]]
- [[cURL]]
- [[Cross-Site Scripting (XSS)]]

## 📚 References
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- [OWASP Testing Guide - Session Management](https://owasp.org/www-project-web-security-testing-guide/)
