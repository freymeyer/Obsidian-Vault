---
dg-publish: true
tags:
  - "#cybersecurity/architecture/design-principles"
  - "#interview/concepts"
aliases:
  - Security by Design
  - Secure Architecture
---

# Secure by Design

> **One-liner:** Building security controls into systems from the start, rather than adding them as an afterthought.

## 🎯 What Is It?
Secure by Design is a proactive security philosophy where systems, applications, and networks are architected with security as a foundational requirement—not a feature bolted on later. It shifts security left in the development lifecycle.

**Core principle:** It's exponentially cheaper and more effective to build security in than to retrofit it.

## 📊 Security by Design vs. Security by Default

| Secure by Design | Security by Default |
|------------------|-------------------|
| Architectural approach | Configuration approach |
| Eliminates vulnerabilities | Minimizes attack surface |
| Example: Input validation in code | Example: Disable unnecessary services |
| Design phase | Deployment phase |

## 🏗️ Key Principles

### 1. Least Privilege
- Minimal permissions by default
- Zero trust architecture
- Role-based access control (RBAC)

### 2. Defense in Depth
- Multiple layers of security controls
- Assume breach mentality
- Fail securely

### 3. Fail Securely
- Graceful degradation without exposing data
- Default-deny policies
- Error messages don't leak sensitive info

### 4. Separation of Duties
- No single user has complete control
- Approval workflows for critical actions
- Segregated environments (dev/test/prod)

### 5. Complete Mediation
- Verify every access request
- No bypasses for "trusted" users
- Consistent enforcement at all layers

### 6. Open Design
- Security through design, not obscurity
- Peer-reviewed cryptography
- Public security documentation

## 🛠️ Implementation Across Layers

| Layer | Secure by Design Practice |
|-------|--------------------------|
| **Network** | [[Network Segmentation]], micro-segmentation, zero trust |
| **Application** | Input validation, parameterized queries, secure APIs |
| **Data** | Encryption at rest/transit, tokenization, data classification |
| **Identity** | MFA enforced, passwordless, attribute-based access |
| **Infrastructure** | Immutable infrastructure, hardened base images |
| **Cloud** | Private by default, IAM least privilege, logging enabled |

## 🔐 Secure by Design in Practice

### Example: Web Application
```
❌ Insecure Approach:
1. Build app with admin panel
2. Deploy to production
3. Add authentication later
4. Bolt on WAF after breach

✅ Secure by Design:
1. Define threat model (STRIDE)
2. Design auth/authz from start
3. Input validation in all functions
4. Secure defaults (HTTPS-only, HSTS)
5. Logging/monitoring built-in
6. Security testing in CI/CD
```

### Example: Network Architecture
```
❌ Flat Network:
All systems on 10.0.0.0/24, full connectivity

✅ Secure by Design:
├── DMZ (Public-facing)
├── Application Tier (restricted egress)
├── Database Tier (no internet)
└── Management Network (jump host only)
```

## 💰 Why It Matters (ROI)

| Phase | Cost to Fix | Time to Fix |
|-------|------------|------------|
| **Design** | $1 | 1x (baseline) |
| **Development** | $10 | 5x |
| **Testing** | $100 | 10x |
| **Production** | $1,000 | 30x |
| **Post-Breach** | $10,000+ | Weeks/months |

## 🛡️ Detection & Prevention

### How to Validate Secure Design
- **Threat modeling** — STRIDE, PASTA frameworks
- **Architecture review** — Security team sign-off before build
- **Automated checks** — IaC security scanning (tfsec, Checkov)
- **Penetration testing** — Test design assumptions
- **Red team exercises** — Adversarial validation

### Red Flags (Insecure Design)
- Authentication added as "Phase 2"
- "We'll secure it before launch"
- Security team only involved in production
- No threat model exists
- Hardcoded secrets in code

## 🎤 Interview Angles

### Common Questions
- "What's the difference between Secure by Design and Security by Default?"
- "How would you implement Secure by Design in a microservices architecture?"
- "Why does Secure by Design provide better ROI than retrofitting security?"

### STAR Story Template
**Situation:** Legacy app being refactored—opportunity to rebuild securely
**Task:** Champion Secure by Design to prevent repeat vulnerabilities
**Action:** Led threat modeling, enforced security gates in CI/CD, implemented zero trust
**Result:** Zero critical vulns at launch (vs. 15 in old version), 60% faster incident response

## 🚨 Common Anti-Patterns

| Anti-Pattern | Why It Fails | Secure Alternative |
|-------------|--------------|-------------------|
| "Security slows us down" | Technical debt compounds | Security in sprint DoD |
| "We're not a target" | Every org is a target | Design for breach scenario |
| "PCI firewall" (perimeter-only) | Inside threats ignored | Zero trust, microsegmentation |
| "We'll patch later" | Vulns exploited immediately | Secure defaults, hardening |

## ✅ Best Practices

- **Start with threat model** — Know your adversaries before designing
- **Security gates** — Architecture review before dev, pentests before prod
- **Use secure frameworks** — Django/Rails vs. raw PHP for web apps
- **Default deny** — Whitelist approach to access control
- **Immutable infrastructure** — Replace, don't patch
- **Document decisions** — Security ADRs (Architecture Decision Records)

## ❌ Common Misconceptions

- **"It's too expensive"** — Costs 10-100x more to fix post-deployment
- **"Only for new projects"** — Can incrementally apply to refactors
- **"Just follow a checklist"** — Requires threat-specific analysis

## 🔗 Related Concepts

- [[Threat Modeling]] — Foundation for secure design decisions
- [[Zero Trust Architecture]] — Network-level secure by design
- [[Defense in Depth]] — Layered security strategy
- [[Least Privilege]] — Access control design principle
- [[DevSecOps]] — Operationalizing secure by design in CI/CD
- [[Security Architecture]] — Enterprise-level secure design
- [[OWASP]] — A06: Insecure Design (opposite of this principle)

## 📚 References

- NIST Secure Software Development Framework (SSDF)
- OWASP SAMM (Software Assurance Maturity Model)
- NCSC Secure by Design principles
- Saltzer & Schroeder: Design Principles for Security (1975)
