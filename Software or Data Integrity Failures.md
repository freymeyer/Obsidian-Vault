---
tags:
  - "#cybersecurity/web-sec/owasp"
  - "#interview/concepts"
aliases:
  - Data Integrity Failures
  - Software Integrity Failures
severity: High
owasp: A08
---

# Software or Data Integrity Failures

> **One-liner:** Code and data integrity not verified, allowing attackers to inject malicious updates, tamper with data, or exploit insecure deserialization.

## 🎯 What Is It?
This is **A08** of [[OWASP]]. These failures occur when applications don't verify the integrity of software updates, critical data, or CI/CD pipelines—allowing attackers to inject malicious code.

## 💥 Why It Matters (Impact)
- **Confidentiality:** Backdoors expose all data
- **Integrity:** Malicious code injected into production
- **Availability:** Ransomware via update mechanism

## 📊 Types of Integrity Failures

| Type | Description | Example |
|------|-------------|---------|
| Insecure CI/CD | Pipeline allows code injection | Compromised GitHub Action |
| Unsigned updates | No verification of software updates | SolarWinds attack |
| Insecure deserialization | Untrusted data deserialized | Java RCE via ObjectInputStream |
| CDN compromise | Third-party scripts modified | Magecart attacks |
| Missing SRI | No Subresource Integrity checks | Tampered JavaScript |

## 💰 Real-World Example: SolarWinds (2021)

> Attackers compromised SolarWinds' build process, inserting malicious code into a trusted software update. 18,000+ organizations automatically installed the backdoored update, including US government agencies.

## 🔬 Vulnerable vs Secure Examples

### Insecure Deserialization
```java
// ❌ VULNERABLE: Deserializing untrusted data
ObjectInputStream ois = new ObjectInputStream(userInput);
Object obj = ois.readObject();  // RCE if malicious payload!

// ✅ SECURE: Use allowlists, avoid native deserialization
// Use JSON with strict schema validation instead
ObjectMapper mapper = new ObjectMapper();
mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, true);
```

### Subresource Integrity (SRI)
```html
<!-- ❌ VULNERABLE: No integrity check -->
<script src="https://cdn.example.com/lib.js"></script>

<!-- ✅ SECURE: SRI hash verification -->
<script src="https://cdn.example.com/lib.js"
        integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxFgVSRqf4RMCVqQ"
        crossorigin="anonymous"></script>
```

## 🛡️ Prevention

| Control | Implementation |
|---------|---------------|
| Code signing | Sign all releases, verify signatures |
| CI/CD security | Protect pipelines, review changes |
| Dependency verification | Use lock files, verify checksums |
| SRI for CDN | Hash-based integrity for external scripts |
| Avoid native deserialization | Use safe formats (JSON with schema) |
| SBOM | Software Bill of Materials for tracking |

## 🔍 CI/CD Security Checklist

| Control | Description |
|---------|-------------|
| ☐ Branch protection | Require reviews, no direct pushes |
| ☐ Signed commits | GPG signing required |
| ☐ Pipeline as code | Version-controlled, reviewed |
| ☐ Secrets management | No hardcoded credentials |
| ☐ Artifact signing | Sign build outputs |
| ☐ Dependency scanning | Check for vulnerable packages |

## 🎤 Interview STAR Example
> **Situation:** Company had no verification for third-party JavaScript libraries loaded from CDNs.
> **Task:** Protect against supply chain attacks via compromised CDN resources.
> **Action:** Implemented SRI hashes for all external scripts. Set up automated SRI generation in build pipeline. Added CSP headers to restrict script sources. Created dependency update process with security review.
> **Result:** Protected against CDN compromise. Detected attempted Magecart-style attack when SRI check failed—blocked malicious script execution.

## 🔗 Related Concepts
- [[Software Supply Chain Failures]]
- [[DevSecOps]]
- [[Cryptographic Failure]]

## 📚 References
- OWASP Software and Data Integrity Failures
- SRI Hash Generator: https://www.srihash.org/
- SLSA Framework: https://slsa.dev/
