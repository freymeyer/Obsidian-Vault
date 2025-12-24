---
tags:
  - "#infrastructure/cloud/aws"
  - "#cybersecurity/iam/access-control"
  - "#interview/concepts"
aliases:
  - STS
  - Security Token Service
  - AssumeRole
---

# AWS STS

> **One-liner:** AWS's service for generating temporary security credentials—the key to role assumption and privilege escalation.

## 🎯 What Is It?
AWS Security Token Service (STS) is a web service that enables you to request **temporary, limited-privilege credentials** for [[AWS IAM]] users or for users that you authenticate (federated users). These credentials are short-lived and automatically expire.

## 🤔 Why It Matters
STS is the mechanism behind **role assumption**—a legitimate feature that attackers abuse for privilege escalation. If a user has `sts:AssumeRole` permissions, they can potentially pivot to roles with higher privileges.

## 🔬 How It Works

### Temporary Credentials
When you assume a role, STS returns:
```json
{
  "Credentials": {
    "AccessKeyId": "ASIARZPUZDIK...",      // Starts with ASIA (temporary)
    "SecretAccessKey": "wJalrXUtnFEMI...",
    "SessionToken": "FQoGZXIvYXdzEBY...",  // Required for temp creds
    "Expiration": "2025-12-24T12:00:00Z"   // Auto-expires
  },
  "AssumedRoleUser": {
    "AssumedRoleId": "AROA...:session-name",
    "Arn": "arn:aws:sts::123456789012:assumed-role/RoleName/session"
  }
}
```

### Key Differences: Long-term vs Temporary
| Aspect | Long-term (User) | Temporary (STS) |
|--------|------------------|-----------------|
| Access Key prefix | `AKIA...` | `ASIA...` |
| Session Token | Not used | Required |
| Expiration | Never (until rotated) | 15 min - 12 hours |
| Revocation | Delete/deactivate key | Wait for expiration |

### Common STS API Calls
| API | Purpose | Use Case |
|-----|---------|----------|
| `GetCallerIdentity` | Who am I? | Verify credentials work |
| `AssumeRole` | Get temp creds for a role | Cross-account, privilege escalation |
| `GetSessionToken` | MFA-protected temp creds | Require MFA for sensitive ops |
| `AssumeRoleWithSAML` | Federated auth (SAML) | Enterprise SSO |
| `AssumeRoleWithWebIdentity` | Federated auth (OIDC) | Mobile apps, web apps |

## ⚔️ Offensive Techniques

### Verify Current Identity
```bash
aws sts get-caller-identity
```
Output:
```json
{
  "UserId": "AIDAEXAMPLE",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/sir.carrotbane"
}
```

### Assume a Role (Privilege Escalation)
```bash
# Step 1: Assume the target role
aws sts assume-role \
  --role-arn arn:aws:iam::123456789012:role/bucketmaster \
  --role-session-name attack-session

# Step 2: Export the temporary credentials
export AWS_ACCESS_KEY_ID="ASIA..."
export AWS_SECRET_ACCESS_KEY="secret..."
export AWS_SESSION_TOKEN="FQoGZX..."

# Step 3: Verify new identity
aws sts get-caller-identity
# Now shows: assumed-role/bucketmaster/attack-session
```

### Finding Assumable Roles
```bash
# List all roles and check trust policies
aws iam list-roles

# Look for roles that trust your current user/account
# In the AssumeRolePolicyDocument:
{
  "Principal": {"AWS": "arn:aws:iam::123456789012:user/sir.carrotbane"},
  "Action": "sts:AssumeRole",
  "Effect": "Allow"
}
```

### Attack Chain Example
```
┌─────────────────┐    sts:AssumeRole    ┌─────────────────┐
│  Low-priv User  │ ─────────────────►   │  bucketmaster   │
│  sir.carrotbane │                      │     (Role)      │
│  - List IAM     │                      │  - S3 FullAccess│
│  - AssumeRole   │                      │  - Data Access  │
└─────────────────┘                      └─────────────────┘
```

## 🛡️ Detection & Prevention

### How to Detect
- **CloudTrail events:**
  - `AssumeRole` — Role assumption attempts
  - `GetCallerIdentity` — Credential verification (recon)
- **Anomaly detection:**
  - Unusual role assumptions
  - Assumptions from unexpected source IPs
  - Assumptions outside business hours

### CloudTrail Log Example
```json
{
  "eventName": "AssumeRole",
  "userIdentity": {
    "arn": "arn:aws:iam::123456789012:user/sir.carrotbane"
  },
  "requestParameters": {
    "roleArn": "arn:aws:iam::123456789012:role/bucketmaster",
    "roleSessionName": "attack-session"
  },
  "sourceIPAddress": "1.2.3.4"
}
```

### How to Prevent
- **Restrict `sts:AssumeRole`** — Don't grant on `Resource: "*"`
- **External ID requirement** — For cross-account roles
- **MFA requirement** — Condition in trust policy
- **Source IP restrictions** — Limit where roles can be assumed from
- **Session duration limits** — Minimize token lifetime

### Trust Policy with MFA Requirement
```json
{
  "Effect": "Allow",
  "Principal": {"AWS": "arn:aws:iam::123456789012:user/admin"},
  "Action": "sts:AssumeRole",
  "Condition": {
    "Bool": {"aws:MultiFactorAuthPresent": "true"}
  }
}
```

## 🎤 Interview Angles

### Common Questions
- "What is STS and when would you use it?"
- "How does AssumeRole work?"
- "What's the difference between AKIA and ASIA access keys?"
- "How would you detect role abuse?"

### STAR Story
> **Situation:** Found leaked AWS credentials in a public GitHub repo.
> **Task:** Assess the blast radius and demonstrate potential impact.
> **Action:** Used `get-caller-identity` to verify creds. Enumerated [[AWS IAM]] policies and found `sts:AssumeRole` on `*`. Listed roles and assumed `admin-role` with full access. Documented complete privilege escalation chain.
> **Result:** Demonstrated that leaked low-privilege creds led to full account compromise. Client implemented credential rotation, secrets scanning, and restricted AssumeRole permissions.

## ✅ Best Practices
- Never allow `sts:AssumeRole` on `Resource: "*"`
- Use external IDs for cross-account access
- Require MFA for sensitive roles
- Set minimum necessary session duration
- Monitor `AssumeRole` calls in CloudTrail

## 🔗 Related Concepts
- [[Amazon Web Services (AWS)]]
- [[AWS IAM]]
- [[AWS S3]]
- [[AWS CLI]]
- [[Privilege Escalation]]
- [[Authorization]]

## 📚 References
- [AWS STS Documentation](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html)
- [AssumeRole Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html)
