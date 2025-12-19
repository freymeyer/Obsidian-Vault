---
tags:
  - "#cybersecurity/cryptography/keys"
---

The final stage in the key management lifecycle is the secure destruction of cryptographic keys. When a key is no longer needed, or its lifecycle has ended, it must be destroyed to ensure it cannot be recovered or used. Proper critical destruction prevents the unauthorised use of old keys and protects against data breaches. 

Note: You usually revoke keys by declaring a key “invalid” before the expiration date, for example, in the case of compromise. Key destruction is safely deleting the key due to the expiration/end of the cryptoperiod assigned. Destruction is not that easy; you need to consider multiple locations, including backups (which are copies stored in various regions for failover). In extreme cases, such as government use, the keys may also be on hardware devices that must be wiped and physically destroyed.

For more information, check out [CSAs](https://cloudsecurityalliance.org/artifacts/key-management-lifecycle-best-practices) best practices.