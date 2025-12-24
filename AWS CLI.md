---
tags:
  - "#infrastructure/cloud/aws"
  - "#tools/cli"
  - "#interview/tools"
aliases:
  - AWS Command Line Interface
  - aws cli
---

# AWS CLI

> **One-liner:** The command-line tool for interacting with AWS services—essential for cloud enumeration, automation, and attack execution.

## 🎯 What Is It?
The AWS Command Line Interface (CLI) is a unified tool to manage AWS services from the terminal. It provides direct access to AWS APIs, enabling automation, scripting, and—from an attacker's perspective—comprehensive enumeration of cloud resources.

## 🤔 Why It Matters
The AWS CLI is the primary tool for:
- **Red Team:** Cloud enumeration, credential abuse, data exfiltration
- **Blue Team:** Incident response, log analysis, security automation
- **DevOps:** Infrastructure automation, deployment scripts

## 🔬 How It Works

### Installation
```bash
# Linux
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip && sudo ./aws/install

# macOS
brew install awscli

# Windows
# Download MSI from AWS website
```

### Credential Configuration
Credentials are stored in `~/.aws/credentials`:
```ini
[default]
aws_access_key_id = AKIAIOSFODNN7EXAMPLE
aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

[profile-name]
aws_access_key_id = AKIA...
aws_secret_access_key = ...
```

Or via environment variables:
```bash
export AWS_ACCESS_KEY_ID="AKIA..."
export AWS_SECRET_ACCESS_KEY="..."
export AWS_SESSION_TOKEN="..."  # For temporary credentials
```

### Command Structure
```
aws <service> <action> [--options]

Examples:
aws s3 ls                           # List S3 buckets
aws iam list-users                  # List IAM users
aws sts get-caller-identity         # Who am I?
```

## ⚔️ Essential Commands for Security

### Identity & Reconnaissance
```bash
# Verify credentials / Who am I?
aws sts get-caller-identity

# Get account info
aws sts get-access-key-info --access-key-id AKIA...
```

### [[AWS IAM]] Enumeration
```bash
# List users
aws iam list-users

# List groups
aws iam list-groups

# List roles
aws iam list-roles

# Get user's groups
aws iam list-groups-for-user --user-name USERNAME

# Get user's inline policies
aws iam list-user-policies --user-name USERNAME
aws iam get-user-policy --user-name USERNAME --policy-name POLICY

# Get user's attached policies
aws iam list-attached-user-policies --user-name USERNAME

# Enumerate role policies
aws iam list-role-policies --role-name ROLE
aws iam get-role-policy --role-name ROLE --policy-name POLICY
```

### [[AWS STS]] - Role Assumption
```bash
# Assume a role
aws sts assume-role \
  --role-arn arn:aws:iam::ACCOUNT:role/ROLE_NAME \
  --role-session-name SESSION_NAME

# Then export the returned credentials
export AWS_ACCESS_KEY_ID="ASIA..."
export AWS_SECRET_ACCESS_KEY="..."
export AWS_SESSION_TOKEN="..."
```

### [[AWS S3]] Operations
```bash
# List all buckets
aws s3api list-buckets
aws s3 ls

# List bucket contents
aws s3api list-objects --bucket BUCKET_NAME
aws s3 ls s3://bucket-name

# Download object
aws s3api get-object --bucket BUCKET --key FILE OUTPUT
aws s3 cp s3://bucket/file ./file

# Check public access (no auth)
aws s3 ls s3://bucket --no-sign-request
```

### EC2 Enumeration
```bash
# List instances
aws ec2 describe-instances

# List security groups
aws ec2 describe-security-groups

# List snapshots
aws ec2 describe-snapshots --owner-ids self
```

### Secrets & Parameters
```bash
# List secrets
aws secretsmanager list-secrets

# Get secret value
aws secretsmanager get-secret-value --secret-id NAME

# List SSM parameters
aws ssm describe-parameters
aws ssm get-parameter --name PARAM --with-decryption
```

## 🛡️ Detection & Prevention

### How to Detect CLI Usage
Monitor CloudTrail for:
- Unusual API calls (especially `iam:*`, `sts:*`)
- High volume of `List*` operations (enumeration)
- API calls from unexpected IPs
- Access outside normal hours

### Suspicious Activity Patterns
| Pattern | Indication |
|---------|------------|
| `get-caller-identity` | Credential testing |
| Rapid `list-*` calls | Enumeration |
| `assume-role` to new roles | Privilege escalation |
| `get-object` from sensitive buckets | Data exfiltration |

### How to Prevent Abuse
- Restrict programmatic access to necessary users
- Use [[Multi-Factor Authentication (MFA)]] for CLI access
- Implement IP allowlisting for API access
- Set up CloudTrail alerts for suspicious patterns

## 📊 Quick Reference Card

| Task | Command |
|------|---------|
| Who am I? | `aws sts get-caller-identity` |
| List users | `aws iam list-users` |
| List buckets | `aws s3 ls` |
| Assume role | `aws sts assume-role --role-arn ARN --role-session-name NAME` |
| Download file | `aws s3 cp s3://bucket/file ./file` |
| Get secret | `aws secretsmanager get-secret-value --secret-id NAME` |

## 🎤 Interview Angles

### Common Questions
- "How do you configure AWS CLI credentials?"
- "What's the first command you run with found AWS creds?"
- "How would you enumerate an AWS account?"
- "What's the difference between `aws s3` and `aws s3api`?"

### STAR Story
> **Situation:** Found AWS access keys in a config file during a pentest.
> **Task:** Determine the credentials' permissions and potential impact.
> **Action:** Used `get-caller-identity` to verify creds worked. Ran through IAM enumeration commands to map permissions. Found `sts:AssumeRole` capability, assumed admin role, documented full access path.
> **Result:** Demonstrated credential exposure led to full account compromise. Client implemented secrets management and credential rotation.

## ✅ Best Practices
- Never hardcode credentials in scripts
- Use `--profile` for multiple accounts
- Enable MFA for CLI access
- Use `--query` and `--output` to filter results
- Store credentials in AWS SSO or credential managers

## 🔗 Related Concepts
- [[Amazon Web Services (AWS)]]
- [[AWS IAM]]
- [[AWS S3]]
- [[AWS STS]]

## 📚 References
- [AWS CLI Documentation](https://docs.aws.amazon.com/cli/)
- [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html)
