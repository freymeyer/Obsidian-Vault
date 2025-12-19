---
tags:
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - Supply Chain Attacks
  - Dependency Vulnerabilities
  - Third-Party Risk
severity: Critical
owasp: A03
---

# Software Supply Chain Failures

> **One-liner:** Compromised dependencies, libraries, or build pipelines that inject malicious code into trusted software.

## 🎯 What Is It?
This is **A03** of [[OWASP]]. Supply chain failures occur when the components you depend on—libraries, frameworks, CI/CD pipelines, or AI models—are compromised, outdated, or unverified.

## 💥 Why It Matters (Impact)
- **Confidentiality:** Backdoors expose all data
- **Integrity:** Malicious code in trusted software
- **Scale:** One compromise affects thousands of downstream users

**Stats:** 742% increase in supply chain attacks (2019-2022)

## 📊 Attack Vectors

| Vector | Description | Example |
|--------|-------------|---------|
| Dependency confusion | Private package name hijacked on public registry | ua-parser-js, event-stream |
| Typosquatting | Malicious package with similar name | `lodahs` instead of `lodash` |
| Compromised maintainer | Attacker gains access to legitimate package | colors.js, faker.js |
| Build pipeline attack | CI/CD system compromised | SolarWinds, Codecov |
| Abandoned packages | Unmaintained packages with vulns | Log4j in legacy systems |

## 💰 Real-World Example: SolarWinds (2021)

> Attackers compromised SolarWinds' build system, inserting malicious code into the Orion update. 18,000+ organizations installed the backdoored update, including US government agencies and Fortune 500 companies. The attack went undetected for 9+ months.

## 🔬 Vulnerable Patterns

```json
// ❌ VULNERABLE: No version pinning
{
  "dependencies": {
    "express": "*",           // Any version - could get malicious update
    "lodash": "^4.0.0"        // Auto-updates to latest 4.x
  }
}

// ✅ SECURE: Exact version pinning + lock file
{
  "dependencies": {
    "express": "4.18.2",      // Exact version
    "lodash": "4.17.21"       // Exact version
  }
}
// + use package-lock.json / yarn.lock
```

## 🔍 How to Detect

```bash
# Check for known vulnerabilities
npm audit
pip-audit
snyk test

# Check for outdated packages
npm outdated
pip list --outdated

# Verify package integrity
npm ci --ignore-scripts  # Uses lock file, no arbitrary scripts
```

## 🛡️ Prevention

| Control | Implementation |
|---------|---------------|
| Lock dependencies | Use lock files (package-lock.json, Pipfile.lock) |
| Pin versions | Exact versions, no wildcards |
| Audit regularly | `npm audit`, Snyk, Dependabot |
| Verify integrity | Check checksums, use SRI |
| Secure CI/CD | Protect build pipelines, sign artifacts |
| SBOM | Maintain Software Bill of Materials |
| Private registries | Host critical packages internally |

## 📋 Supply Chain Security Checklist

| Control | Status |
|---------|--------|
| ☐ Dependencies pinned to exact versions | |
| ☐ Lock files committed and used | |
| ☐ Automated vulnerability scanning | |
| ☐ CI/CD pipeline hardened | |
| ☐ Build artifacts signed | |
| ☐ SBOM generated and maintained | |
| ☐ Private registry for critical packages | |
| ☐ Review process for new dependencies | |

## 🎤 Interview STAR Example
> **Situation:** Log4Shell (CVE-2021-44228) was disclosed; we needed to assess exposure immediately.
> **Task:** Identify all Log4j usage across 200+ services and remediate.
> **Action:** Generated SBOM for all services using automated scanning. Identified 47 services with vulnerable Log4j versions. Prioritized internet-facing services. Deployed WAF rules as immediate mitigation while patching. Coordinated rolling updates over 72 hours.
> **Result:** Zero exploitation despite active attacks. Patched all vulnerable services within 3 days. Implemented continuous dependency scanning to prevent future exposure.

## 🔗 Related Concepts
- [[Software or Data Integrity Failures]]
- [[DevSecOps]]
- [[Vulnerability.md|Vulnerability Management]]

## 📚 References
- OWASP Supply Chain Security
- SLSA Framework: https://slsa.dev/
- Snyk State of Open Source Security
- NIST SSDF (Secure Software Development Framework)
