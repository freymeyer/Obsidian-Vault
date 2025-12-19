---
tags:
  - "#cybersecurity/cryptography/keys"
---

After keys are generated and before they are distributed, they must be securely stored. Secure key storage is critical to prevent unauthorised access and use. It employs encrypted databases or specialised [[hardware security module (HSM)]], offering robust protection mechanisms. Effective key storage solutions also include access controls and audit logs to ensure only authorised entities can access the keys and any access is correctly recorded for future review.
## Secure Storage Options for Cryptographic Keys

- [[Hardware Security Module (HSM)]]


**Cloud-Based Key Management Services**
- **Examples:** [AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) (KMS), [Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/basic-concepts), and [Google Cloud Key Management Service](https://cloud.google.com/security/products/security-key-management?hl=en) provide managed environments for handling cryptographic keys.
- **Benefits:** These services offer scalability, high availability, and built-in compliance features, allowing for the centralised management of keys with strong security and auditing capabilities.

**Encrypted Databases and Keystores**
- **Use case:** For environments where HSMs or cloud-based solutions are not viable, encrypted databases or key stores can provide a secure storage option.
- **Implementation:** These should be implemented with strong encryption algorithms and access controls, ensuring that only authorised applications and users can access the stored keys.