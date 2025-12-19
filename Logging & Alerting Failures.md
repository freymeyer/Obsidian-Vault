---
tags:
  - "#cybersecurity/blue-team/logging-siem"
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - Security Logging Failures
  - Insufficient Logging
severity: Medium
owasp: A09
---

# Logging & Alerting Failures

> **One-liner:** Insufficient logging and monitoring prevents detection of breaches and hinders incident response.

## 🎯 What Is It?
This is **A09** of [[OWASP]], part of failures in [[Identification, Authentication, Authorization, and Accountability (IAAA)]]. When applications don't record security-relevant events, defenders can't detect attacks or investigate breaches.

## 💥 Why It Matters (Impact)
- **Detection:** Average breach goes undetected for 200+ days
- **Investigation:** Can't determine what happened or scope of breach
- **Compliance:** Fails audit requirements (PCI, HIPAA, SOC2)
- **Accountability:** Can't prove who did what, when

## 📊 Common Failure Patterns

| Failure | Impact |
|---------|--------|
| No auth event logging | Can't detect brute-force attacks |
| Missing access logs | Can't identify data exfiltration |
| Logs stored locally | Attacker can delete evidence |
| No alerting | Attacks go unnoticed in real-time |
| Short retention | Can't investigate historical breaches |
| Sensitive data in logs | Credentials/PII exposure |
| Vague error messages | Can't diagnose issues |

## 🔬 What to Log (Security Events)

```python
# Essential security events to log:
security_events = [
    "login_success",
    "login_failure",
    "logout",
    "password_change",
    "password_reset_request",
    "mfa_enrollment",
    "mfa_failure",
    "privilege_escalation",
    "admin_action",
    "access_denied",
    "data_export",
    "api_rate_limit_exceeded",
    "input_validation_failure",
]

# Log format example
log.info(
    "AUTH_FAILURE",
    user=username,
    ip=request.remote_addr,
    user_agent=request.user_agent,
    reason="invalid_password",
    timestamp=datetime.utcnow().isoformat()
)
```

## 🔍 Log Format Best Practices

| Field | Purpose |
|-------|--------|
| Timestamp (UTC) | When it happened |
| Event type | What happened |
| User/Session ID | Who did it |
| Source IP | Where from |
| Resource accessed | What was affected |
| Outcome | Success/failure |
| User agent | Client fingerprinting |

## 🛡️ Prevention

| Control | Implementation |
|---------|---------------|
| Centralized logging | Ship to SIEM (ELK, Splunk) |
| Log auth lifecycle | All login/logout/changes |
| Immutable storage | Write-once, off-host |
| Real-time alerting | Brute-force, privilege escalation |
| Retention policy | 90+ days minimum |
| Log review | Regular analysis, dashboards |
| No sensitive data | Never log passwords, tokens, PII |

## 🎤 Interview STAR Example
> **Situation:** Post-breach investigation revealed we had no visibility into what the attacker accessed.
> **Task:** Implement comprehensive security logging and monitoring.
> **Action:** Deployed centralized logging to Elastic SIEM. Created log standards for all applications. Built dashboards for auth events, access patterns. Set up alerts for brute-force (>10 failures/min), privilege escalation, and off-hours admin access.
> **Result:** MTTD reduced from "never" to 15 minutes. Successfully detected and blocked credential stuffing attack within 3 minutes of start.

## 🔗 Related Concepts
- [[Security Information and Event Management system (SIEM)]]
- [[Detection Engineering]]
- [[Blue Teaming]]
- [[Incident Response]]
- [[ELK Stack]]
- [[Splunk]]

## 📚 References
- OWASP Logging Cheat Sheet
- OWASP Security Logging and Monitoring Failures