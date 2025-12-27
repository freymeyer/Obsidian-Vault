---
tags:
  - "#cybersecurity/vulnerabilities"
  - "#cybersecurity/cryptography"
  - "#interview/concepts"
aliases:
  - CVE-2014-0160
  - Heartbleed Bug
---

# Heartbleed

> **One-liner:** A critical OpenSSL vulnerability that allowed attackers to read sensitive memory contents including private keys, passwords, and session tokens.

## 🎯 What Is It?
**Heartbleed** ([[CVE-2014-0160]]) was a catastrophic buffer over-read vulnerability in the OpenSSL cryptographic library discovered in 2014. It affected the TLS/SSL heartbeat extension, allowing attackers to read up to 64KB of server memory per request without authentication or leaving traces in logs.

The vulnerability exposed:
- Private encryption keys
- User credentials (usernames/passwords)
- Session tokens and cookies
- Emails and instant messages
- Sensitive business data

**Affected Versions:** OpenSSL 1.0.1 through 1.0.1f

## 🤔 Why It Matters

### Impact Scale
- **2/3 of all web servers** were vulnerable when disclosed (April 2014)
- Exposed data from major services: Yahoo, Facebook, Google, GitHub
- **No detection possible** — Attack left no server-side logs
- **Persistent risk** — Certificates needed replacement, not just patching

### Historical Significance
- Largest vulnerability in modern internet history
- Led to massive SSL/TLS certificate revocation events
- Prompted creation of the [[Core Infrastructure Initiative (CII)]]
- Changed how the industry views open-source security

## 🔬 How It Works

### TLS Heartbeat Extension
The heartbeat is a "keep-alive" mechanism for TLS connections:
```
Client → "Hey, I'm still here!" (sends data)
Server → "Got it!" (echoes data back)
```

### The Vulnerability
OpenSSL failed to validate the **length field** in heartbeat requests:

```
Normal Heartbeat:
Client: "Send back this 4-byte message: 'test'"
Server: Returns 4 bytes: "test"

Heartbleed Attack:
Client: "Send back this 4-byte message: 'test'" BUT LENGTH=65535
Server: Returns 65535 bytes — INCLUDING ADJACENT MEMORY!
```

### Attack Visualization
```
┌─────────────────────────────────────┐
│  Heartbeat Request Payload          │
├─────────────────────────────────────┤
│ Type: 1 (request)                   │
│ Length: 65535 (LIE!)                │
│ Actual Data: "test" (4 bytes)       │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│  Server Memory Returned             │
├─────────────────────────────────────┤
│ "test"                              │  ← Your 4 bytes
│ "admin:SuperSecretP@ssw0rd!"        │  ← Memory leak!
│ "SESSION_ID=a3f8c92..."             │  ← Stolen session
│ "-----BEGIN PRIVATE KEY-----..."    │  ← SSL private key!
└─────────────────────────────────────┘
```

### Exploitation Process
```bash
# Using Metasploit
use auxiliary/scanner/ssl/openssl_heartbleed
set RHOSTS target.com
set RPORT 443
run

# Manual exploitation
# Send malformed heartbeat with length=65535
# Server returns 64KB of memory content
# Repeat to collect more data
```

## 📊 Technical Details

### Vulnerable Code (Simplified)
```c
// OpenSSL heartbeat handler (vulnerable)
int tls1_process_heartbeat(SSL *s) {
    unsigned char *p = &s->s3->rrec.data[0];
    unsigned short payload_length;
    
    payload_length = (p[0] << 8) | p[1]; // Client-controlled!
    
    // VULNERABLE: No bounds check!
    memcpy(bp, p, payload_length);
    return 0;
}
```

The bug: `payload_length` is **user-controlled** and never validated against actual data size.

## 🛡️ Detection & Prevention

### How to Detect
**For Servers:**
- Check OpenSSL version: `openssl version`
- Run vulnerability scanners:
```bash
nmap --script ssl-heartbleed target.com

# Or Metasploit
msf> use auxiliary/scanner/ssl/openssl_heartbleed
```

**For Defenders:**
- IDS/IPS signatures for heartbeat anomalies
- Monitor for unusual SSL/TLS traffic patterns
- **Note:** Successful attacks often leave NO logs

### How to Prevent / Mitigate

#### Immediate Actions (2014)
1. **Patch OpenSSL** → Upgrade to 1.0.1g or later
2. **Reissue SSL certificates** → Private keys may be compromised
3. **Revoke old certificates** → Invalidate potentially leaked keys
4. **Reset user passwords** — Credentials may have been exposed
5. **Invalidate session tokens** — Force re-authentication

#### Long-Term Solutions
- **Disable heartbeat extension** if not needed:
```bash
# Compile OpenSSL without heartbeat
./config -DOPENSSL_NO_HEARTBEATS
```
- Use memory-safe languages for crypto (Rust-based alternatives)
- Implement memory protections (ASLR, canaries)
- Regular security audits of critical libraries

### Testing for Heartbleed
```bash
# Using nmap
nmap -p 443 --script ssl-heartbleed <target>

# Using sslscan
sslscan --heartbleed <target>:443

# Online tester
https://filippo.io/Heartbleed/
```

## 🎤 Interview Angles

### Common Questions
- **Q: What is Heartbleed and why was it so severe?**
  - A: Heartbleed (CVE-2014-0160) was an OpenSSL bug that allowed attackers to read server memory remotely without authentication. It exposed private keys, passwords, and session data from 2/3 of all web servers. Severity came from: widespread impact, silent exploitation, and need to revoke/reissue certificates globally.

- **Q: How does Heartbleed differ from other vulnerabilities?**
  - A: It's a **memory leak**, not code execution. Attackers read adjacent memory, not run commands. Also: no logs, no authentication needed, and repeated exploitation was possible to collect more data.

- **Q: What should a blue team do if Heartbleed is found today?**
  - A: 1) Immediately patch OpenSSL, 2) Assume private keys are compromised—regenerate and reissue SSL certificates, 3) Force password resets, 4) Invalidate all sessions, 5) Review logs for indicators (though attacks are mostly silent).

### STAR Example
> **Situation:** During a 2014 incident response engagement, a financial client discovered their servers were running vulnerable OpenSSL versions post-disclosure.
> **Task:** Determine scope of compromise and restore security posture.
> **Action:** Patched all OpenSSL instances, revoked and reissued 47 SSL certificates, forced company-wide password reset, invalidated all active sessions. Analyzed network traffic for heartbeat anomalies (found none, but couldn't rule out prior exploitation).
> **Result:** Restored secure communications within 72 hours. Implemented continuous vulnerability scanning to prevent future delays in patching critical CVEs.

## ✅ Best Practices
- **Never assume old vulnerabilities are gone** — Legacy systems may still run vulnerable OpenSSL
- Monitor CVE databases for critical library vulnerabilities
- Use automated scanning to detect Heartbleed in your environment
- Treat crypto library vulnerabilities as **critical priority**
- Always assume worst-case (private key compromise) and reissue certificates

## ❌ Common Misconceptions
- **"Heartbleed is just a password leak"** — No, it exposed private keys, which is far worse. Entire certificate infrastructure needed replacement.
- **"Patching fixes everything"** — No, you must also revoke/reissue certificates and reset credentials.
- **"You can detect if you were exploited"** — Mostly false. Attacks left minimal traces; assume compromise if you were vulnerable.
- **"Only affects web servers"** — No, any service using vulnerable OpenSSL (VPNs, email, chat) was affected.

## 🔗 Related Concepts
- [[OpenSSL]]
- [[Transport Layer Security (TLS)]]
- [[Buffer Overflow]]
- [[Memory Corruption]]
- [[CVE (Common Vulnerabilities and Exposures)]]
- [[SSL Certificate]]
- [[Public Key Infrastructure (PKI)]]

## 📚 References
- https://heartbleed.com/ — Official Heartbleed Information
- https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0160
- https://www.openssl.org/news/secadv/20140407.txt — OpenSSL Security Advisory
- XKCD Heartbleed Explanation: https://xkcd.com/1354/
