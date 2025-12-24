---
tags:
  - "#infrastructure/cloud/aws"
  - "#interview/concepts"
aliases:
  - AWS
---

# Amazon Web Services (AWS)

> **One-liner:** The world's largest cloud computing platform, providing on-demand infrastructure, storage, and services—and a prime target for attackers.

## 🎯 What Is It?
Amazon Web Services (AWS) is a comprehensive cloud computing platform by Amazon, offering 200+ services including compute (EC2), storage ([[AWS S3]]), databases, networking, and security tools. Organizations use AWS to host applications, store data, and run infrastructure without managing physical servers.

## 🤔 Why It Matters (Security Perspective)
AWS misconfigurations are a leading cause of data breaches. Understanding AWS security is essential for both **offensive** (finding exposed resources) and **defensive** (securing cloud infrastructure) security roles.

**Notable Breaches:**
- [[Capital One breach]] (2019) — SSRF to AWS metadata, 100M+ records exposed
- Twitch (2021) — Misconfigured server exposed source code
- Uber (2017) — Publicly accessible S3 bucket with sensitive data

## 🔬 How It Works

### Core Security Services
| Service | Purpose | Security Relevance |
|---------|---------|-------------------|
| [[AWS IAM]] | Identity & Access Management | User/role permissions, policies |
| [[AWS S3]] | Object Storage | Public bucket misconfigurations |
| [[AWS STS]] | Temporary Credentials | AssumeRole for privilege escalation |
| EC2 | Virtual Servers | Metadata endpoint attacks (169.254.169.254) |
| CloudTrail | Audit Logging | Tracking API calls for forensics |
| GuardDuty | Threat Detection | Anomaly detection for AWS resources |

### AWS Credential Types
```
┌─────────────────────────────────────────────────────┐
│ Long-Term Credentials                               │
│  • Access Key ID (AKIA...)                          │
│  • Secret Access Key                                │
│  • Used by: IAM Users, programmatic access          │
├─────────────────────────────────────────────────────┤
│ Temporary Credentials (from STS)                    │
│  • Access Key ID (ASIA...)                          │
│  • Secret Access Key                                │
│  • Session Token                                    │
│  • Used by: AssumeRole, federated users             │
└─────────────────────────────────────────────────────┘
```

### Credential Storage
Credentials are typically stored in `~/.aws/credentials`:
```ini
[default]
aws_access_key_id = AKIAIOSFODNN7EXAMPLE
aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)
- **CloudTrail logs** — Monitor for unusual API calls
- **GuardDuty alerts** — Detect credential compromise, unusual access patterns
- **S3 Access Logs** — Track who accessed which buckets
- **IAM Access Analyzer** — Find overly permissive policies

### How to Prevent / Mitigate
- Enable **MFA** on all accounts, especially root
- Use **IAM roles** instead of long-term access keys
- Enable **IMDSv2** (requires token for metadata)
- Block public S3 bucket creation at organization level
- Implement **least privilege** policies
- Rotate credentials regularly

## ⚔️ Offensive Techniques

### Enumeration Commands ([[AWS CLI]])
```bash
# Check current identity
aws sts get-caller-identity

# List IAM users
aws iam list-users

# List S3 buckets
aws s3api list-buckets

# Enumerate user policies
aws iam list-user-policies --user-name TARGET
aws iam list-attached-user-policies --user-name TARGET
```

### Common Attack Paths
1. **Leaked credentials** → enumerate → escalate → exfiltrate
2. **SSRF to metadata** → steal IAM role credentials
3. **Public S3 bucket** → download sensitive data
4. **AssumeRole abuse** → pivot to higher privileges

## 🎤 Interview Angles

### Common Questions
- "How would you secure an AWS environment?"
- "What is the AWS shared responsibility model?"
- "How do IAM policies work?"
- "What's the difference between IAM users and roles?"

### STAR Story
> **Situation:** Cloud security assessment revealed exposed S3 buckets and overly permissive IAM policies.
> **Task:** Identify vulnerabilities and recommend remediation.
> **Action:** Used [[AWS CLI]] to enumerate all buckets, identified 3 with public read access containing PII. Analyzed IAM policies and found users with `sts:AssumeRole` on `*`. Documented findings and created remediation playbook.
> **Result:** Client remediated all findings within 48 hours. Implemented preventive controls blocking future public bucket creation.

## ✅ Best Practices
- Never commit credentials to version control
- Use environment variables or IAM roles for applications
- Enable CloudTrail in all regions
- Set up billing alerts for anomaly detection
- Use AWS Organizations with SCPs for guardrails

## 🔗 Related Concepts
- [[AWS IAM]]
- [[AWS S3]]
- [[AWS STS]]
- [[AWS CLI]]
- [[Server-Side Request Forgery (SSRF)]]
- [[Security Misconfigurations]]
- [[Capital One breach]]

## 📚 References
- [AWS Security Best Practices](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/)
- [AWS CLI Documentation](https://docs.aws.amazon.com/cli/)
- OWASP Cloud Security Testing Guide
