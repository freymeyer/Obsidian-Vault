---
tags:
  - "#infrastructure/cloud/aws"
  - "#cybersecurity/data-security"
  - "#interview/concepts"
aliases:
  - S3
  - Simple Storage Service
  - S3 Bucket
---

# AWS S3

> **One-liner:** AWS's object storage service—highly scalable, frequently misconfigured, and a goldmine for attackers when exposed.

## 🎯 What Is It?
Amazon Simple Storage Service (S3) is an object storage service that stores data as objects within **buckets**. Think of a bucket as a top-level folder in the cloud where you can store any type of file: images, documents, logs, backups, or entire databases.

## 🤔 Why It Matters
S3 bucket misconfigurations are one of the most common causes of data breaches. Public buckets have exposed:
- Customer PII (Uber, Verizon)
- Source code (Twitch)
- Military files (Pentagon contractor)
- Medical records (countless healthcare orgs)

## 🔬 How It Works

### S3 Architecture
```
┌─────────────────────────────────────────────────────┐
│                    AWS Account                      │
│  ┌─────────────────────────────────────────────┐   │
│  │              S3 Bucket                       │   │
│  │   Name: my-company-data-prod                 │   │
│  │   Region: us-east-1                          │   │
│  │   ┌─────────────────────────────────────┐   │   │
│  │   │ Objects (files)                      │   │   │
│  │   │  ├── documents/invoice.pdf           │   │   │
│  │   │  ├── images/logo.png                 │   │   │
│  │   │  └── backups/db-2024.sql            │   │   │
│  │   └─────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────┘
```

### Access Control Layers
| Layer | Description |
|-------|-------------|
| **IAM Policies** | User/role permissions (who can access) |
| **Bucket Policies** | JSON policies attached to bucket |
| **ACLs** | Legacy object-level permissions |
| **Block Public Access** | Account/bucket-level override |

### Common Permissions
| Permission | Description | CLI Action |
|------------|-------------|------------|
| `s3:ListBucket` | List objects in bucket | `aws s3 ls s3://bucket` |
| `s3:GetObject` | Download objects | `aws s3api get-object` |
| `s3:PutObject` | Upload objects | `aws s3 cp file s3://bucket` |
| `s3:DeleteObject` | Delete objects | `aws s3 rm` |
| `s3:ListAllMyBuckets` | List all buckets in account | `aws s3api list-buckets` |

## ⚔️ Offensive Techniques

### Enumeration Commands ([[AWS CLI]])
```bash
# List all buckets in account
aws s3api list-buckets

# List objects in a bucket
aws s3api list-objects --bucket BUCKET_NAME

# Download a specific object
aws s3api get-object --bucket BUCKET_NAME --key FILE_KEY OUTPUT_FILE

# Alternative: s3 high-level commands
aws s3 ls s3://bucket-name
aws s3 cp s3://bucket-name/file.txt ./file.txt

# Check if bucket is public (no creds)
aws s3 ls s3://bucket-name --no-sign-request
```

### Finding Exposed Buckets
```bash
# Common naming patterns to brute-force
company-backup, company-prod, company-dev
company.com, company-assets, company-data

# Tools for bucket enumeration
# - S3Scanner
# - BucketFinder
# - cloud_enum
```

### Attack Scenario (from lab)
```bash
# 1. Assume role with S3 access
aws sts assume-role --role-arn arn:aws:iam::123456789012:role/bucketmaster \
    --role-session-name attack

# 2. Export temporary credentials
export AWS_ACCESS_KEY_ID="ASIA..."
export AWS_SECRET_ACCESS_KEY="..."
export AWS_SESSION_TOKEN="..."

# 3. List available buckets
aws s3api list-buckets

# 4. List objects in target bucket
aws s3api list-objects --bucket easter-secrets-123145

# 5. Exfiltrate data
aws s3api get-object --bucket easter-secrets-123145 --key cloud_password.txt loot.txt
```

## 🛡️ Detection & Prevention

### How to Detect
- **S3 Access Logs** — Track all bucket access
- **CloudTrail** — Monitor S3 API calls
- **Macie** — Automated PII/sensitive data discovery
- **Config Rules** — Alert on public bucket creation

### How to Prevent
| Control | Implementation |
|---------|----------------|
| Block Public Access | Enable at account AND bucket level |
| Encryption | Enable SSE-S3 or SSE-KMS by default |
| Versioning | Enable for backup/recovery |
| MFA Delete | Require MFA to delete objects |
| Least Privilege | Minimal [[AWS IAM]] permissions |
| VPC Endpoints | Private access without internet |

### Bucket Policy Example (Deny Public)
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Principal": "*",
    "Action": "s3:*",
    "Resource": ["arn:aws:s3:::bucket-name", "arn:aws:s3:::bucket-name/*"],
    "Condition": {
      "Bool": {"aws:SecureTransport": "false"}
    }
  }]
}
```

## 📊 Security Checklist

| ☐ | Control |
|---|---------|
| ☐ | Block Public Access enabled (account-level) |
| ☐ | Block Public Access enabled (bucket-level) |
| ☐ | Server-side encryption enabled |
| ☐ | Access logging enabled |
| ☐ | Versioning enabled for critical buckets |
| ☐ | No wildcard (*) principals in bucket policies |
| ☐ | MFA Delete for sensitive buckets |

## 🎤 Interview Angles

### Common Questions
- "How would you secure an S3 bucket?"
- "What's the difference between bucket policies and IAM policies?"
- "How do you find exposed S3 buckets during a pentest?"
- "What is S3 Block Public Access?"

### STAR Story
> **Situation:** Security audit found 47 S3 buckets, unclear which were public.
> **Task:** Identify exposed buckets and sensitive data at risk.
> **Action:** Used AWS CLI and Config to enumerate all buckets. Found 3 with public read containing PII. Enabled Block Public Access at org level, remediated bucket policies, set up automated alerts.
> **Result:** Zero public buckets within 24 hours. Prevented potential breach affecting 50K+ customer records.

## 🔗 Related Concepts
- [[Amazon Web Services (AWS)]]
- [[AWS IAM]]
- [[AWS STS]]
- [[AWS CLI]]
- [[Security Misconfigurations]]
- [[Data Exfiltration]]

## 📚 References
- [S3 Security Best Practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)
- [S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html)
