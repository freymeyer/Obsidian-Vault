---
tags:
  - "#infrastructure/cloud/aws"
  - "#cybersecurity/iam/access-control"
  - "#interview/concepts"
aliases:
  - IAM
  - AWS Identity and Access Management
---

# AWS IAM

> **One-liner:** AWS's identity and access management service that controls who can do what across your cloud infrastructure.

## 🎯 What Is It?
AWS Identity and Access Management (IAM) is the service that manages authentication and [[Authorization]] for AWS resources. It determines **who** (users, roles, services) can perform **what actions** (API calls) on **which resources** (S3 buckets, EC2 instances, etc.).

## 🤔 Why It Matters
IAM misconfiguration is one of the most common causes of cloud security incidents. Overly permissive policies, leaked credentials, and improper role trust relationships have led to massive breaches at companies like Toyota, Accenture, and [[Capital One breach|Capital One]].

## 🔬 How It Works

### IAM Components

```
┌──────────────────────────────────────────────────────────────┐
│                        AWS IAM                               │
├──────────────┬──────────────┬──────────────┬────────────────┤
│    USERS     │    GROUPS    │    ROLES     │   POLICIES     │
│  (Identity)  │ (Collection) │ (Assumable)  │ (Permissions)  │
├──────────────┼──────────────┼──────────────┼────────────────┤
│ Has password │ Contains     │ Temporary    │ JSON document  │
│ Has keys     │ multiple     │ credentials  │ Defines what   │
│ Long-term    │ users        │ No password  │ is allowed     │
└──────────────┴──────────────┴──────────────┴────────────────┘
```

### IAM Users
A user represents a single identity with permanent credentials:
- **Console password** — For web UI access
- **Access keys** — For programmatic/CLI access
- Can have **inline** or **attached** policies

### IAM Groups
Logical collection of users for easier permission management:
```
Group: "Developers"
  └── Users: alice, bob, charlie
  └── Attached Policy: DeveloperAccess
```

### IAM Roles
Temporary identity that can be **assumed** by users, services, or external accounts:
- No permanent credentials
- Provides temporary credentials via [[AWS STS]]
- Used for: cross-account access, EC2 instances, Lambda functions

### IAM Policies
JSON documents defining permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::my-bucket/*",
      "Principal": {"AWS": "arn:aws:iam::123456789012:user/alice"}
    }
  ]
}
```

| Element | Description |
|---------|-------------|
| **Effect** | Allow or Deny |
| **Action** | What API calls are permitted |
| **Resource** | Which AWS resources (ARN) |
| **Principal** | Who the policy applies to |
| **Condition** | When the policy applies (optional) |

### Policy Types

| Type | Description | Use Case |
|------|-------------|----------|
| **Inline** | Embedded directly in user/role/group | One-off, specific permissions |
| **Managed (AWS)** | Pre-built by AWS | Common use cases (ReadOnlyAccess) |
| **Managed (Custom)** | Created by you | Reusable across identities |

## ⚔️ Offensive Enumeration

### Enumerate with [[AWS CLI]]
```bash
# List all users
aws iam list-users

# List groups for a user
aws iam list-groups-for-user --user-name TARGET

# List inline policies
aws iam list-user-policies --user-name TARGET

# Get inline policy details
aws iam get-user-policy --user-name TARGET --policy-name POLICY_NAME

# List attached (managed) policies
aws iam list-attached-user-policies --user-name TARGET

# List roles
aws iam list-roles

# List role policies
aws iam list-role-policies --role-name ROLE_NAME
aws iam get-role-policy --role-name ROLE_NAME --policy-name POLICY_NAME
```

### Common Privilege Escalation Paths
1. **sts:AssumeRole** — Assume a role with higher privileges
2. **iam:PassRole** — Pass a privileged role to a service you control
3. **iam:CreateAccessKey** — Create keys for another user
4. **iam:AttachUserPolicy** — Attach AdministratorAccess to yourself

## 🛡️ Detection & Prevention

### How to Detect
- **CloudTrail** — Monitor `iam:*` API calls
- **IAM Access Analyzer** — Find overly permissive policies
- **Config Rules** — Alert on policy changes

### How to Prevent
- Implement **least privilege** — Only grant necessary permissions
- Use **roles** instead of users for applications
- Require **MFA** for sensitive operations
- Regularly **audit** and remove unused credentials
- Use **permission boundaries** to limit maximum permissions

## 🎤 Interview Angles

### Common Questions
- "What's the difference between IAM users and roles?"
- "Explain inline vs managed policies."
- "How would you audit IAM permissions?"
- "What's a trust policy vs permission policy?"

### STAR Story
> **Situation:** During a cloud pentest, found an AWS access key in a GitHub repo.
> **Task:** Enumerate the compromised account's permissions and potential impact.
> **Action:** Used the key to call `get-caller-identity`, then enumerated policies. Found the user could `sts:AssumeRole` on a role with S3 full access. Assumed the role and listed buckets containing sensitive data.
> **Result:** Demonstrated full credential compromise chain. Client rotated all keys and implemented secrets scanning in CI/CD.

## ✅ Best Practices
- Delete root access keys
- Enable MFA on all accounts
- Use groups for permission assignment
- Implement permission boundaries
- Regular access reviews with IAM Access Analyzer

## 🔗 Related Concepts
- [[Amazon Web Services (AWS)]]
- [[AWS STS]]
- [[AWS S3]]
- [[AWS CLI]]
- [[Authorization]]
- [[Role-Based Access Control (RBAC)]]
- [[Privilege Escalation]]

## 📚 References
- [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [IAM Policy Evaluation Logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
