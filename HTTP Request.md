---
tags:
  - "#cybersecurity/web-security/fundamentals"
  - "#infrastructure/networking/protocols"
  - "#interview/concepts"
aliases:
  - HTTP Requests
  - HTTP Response
  - HTTP
  - Hypertext Transfer Protocol
---

# HTTP Request

> **One-liner:** The standardized message format used by clients to request resources from web servers and receive responses.

## 🎯 What Is It?
HTTP (Hypertext Transfer Protocol) is the foundation of data communication on the web. An **HTTP request** is a message sent by a client (browser, [[cURL]], API client) to a server, asking for a resource or action. The server replies with an **HTTP response** containing the requested data or a status indicating the result.

## 🤔 Why It Matters
- Every web application interaction uses HTTP
- Understanding HTTP is essential for web security testing
- Manipulation of requests is the basis for most web attacks
- Required knowledge for [[Cross-Site Scripting (XSS)]], [[SQL Injection]], [[Command Injection]], etc.

## 🔬 How It Works

### HTTP Request Structure
```
[METHOD] [PATH] HTTP/[VERSION]
[HEADERS]

[BODY]
```

**Example GET Request:**
```http
GET /api/users HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: application/json
Cookie: session=abc123
```

**Example POST Request:**
```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 29

username=admin&password=secret
```

### HTTP Response Structure
```http
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: session=xyz789
Content-Length: 1234

<!DOCTYPE html>...
```

## 📊 HTTP Methods

| Method | Purpose | Body? | Idempotent? |
|--------|---------|-------|-------------|
| **GET** | Retrieve data | No | Yes |
| **POST** | Submit data/create | Yes | No |
| **PUT** | Replace/update | Yes | Yes |
| **PATCH** | Partial update | Yes | No |
| **DELETE** | Remove resource | Optional | Yes |
| **HEAD** | GET without body | No | Yes |
| **OPTIONS** | Get allowed methods | No | Yes |

## 📊 HTTP Status Codes

| Range | Category | Examples |
|-------|----------|----------|
| **1xx** | Informational | 100 Continue |
| **2xx** | Success | 200 OK, 201 Created, 204 No Content |
| **3xx** | Redirection | 301 Moved, 302 Found, 304 Not Modified |
| **4xx** | Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| **5xx** | Server Error | 500 Internal Error, 502 Bad Gateway, 503 Unavailable |

## 🔑 Important Headers

### Request Headers
| Header | Purpose | Security Relevance |
|--------|---------|-------------------|
| `Host` | Target domain | Host header attacks |
| `User-Agent` | Client identifier | [[User-Agent Spoofing]] |
| `Cookie` | Session data | [[Cookie]] theft, session hijacking |
| `Authorization` | Auth credentials | Token leakage |
| `Content-Type` | Body format | Content-type confusion |
| `Referer` | Previous page | CSRF checks, info leakage |
| `Origin` | Request origin | CORS, CSRF |

### Response Headers
| Header | Purpose | Security Relevance |
|--------|---------|-------------------|
| `Set-Cookie` | Create cookie | Session management |
| `Content-Security-Policy` | XSS protection | CSP bypass |
| `X-Frame-Options` | Clickjacking protection | Framing attacks |
| `Strict-Transport-Security` | Force HTTPS | Downgrade attacks |

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)
- Log and analyze unusual HTTP methods (PUT, DELETE on web apps)
- Monitor for abnormal header values
- Track request patterns for automation detection
- Watch for parameter manipulation attempts

### How to Prevent / Mitigate
- Validate all input on server-side
- Implement proper authentication and [[Authorization]]
- Use HTTPS for all traffic
- Set secure response headers

## 🎤 Interview Angles

### Common Questions
- "Walk me through what happens when you type a URL in your browser"
- "What's the difference between GET and POST?"
- "Explain HTTP status code categories"
- "What headers are important for security?"

### STAR Story
> **Situation:** A web application was vulnerable to parameter tampering in API requests.
> **Task:** Identify how attackers could exploit the API and recommend fixes.
> **Action:** Analyzed HTTP request structure, identified missing authorization checks on endpoints, demonstrated IDOR by modifying request parameters.
> **Result:** Implemented server-side authorization and input validation, preventing unauthorized data access.

## ✅ Best Practices
- Always validate HTTP method on server-side
- Never trust client-provided headers
- Use secure cookies (HttpOnly, Secure, SameSite)
- Implement proper CORS policies

## 🔗 Related Concepts
- [[cURL]]
- [[Cookie]]
- [[Web Session]]
- [[Cross-Site Scripting (XSS)]]
- [[Cross-site request forgery (CSRF)]]
- [[API Security]]

## 📚 References
- [MDN HTTP Documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP)
- [RFC 7230-7235 - HTTP/1.1](https://tools.ietf.org/html/rfc7230)
