---
tags:
  - "#cybersecurity/cryptography/keys"
---

Key revocation is the process of invalidating a key before its scheduled expiration. This might be necessary if the key is compromised, the user is no longer authorised, or the key is suspected of being exposed to unauthorised entities. Revocation mechanisms must be in place to ensure that revoked keys cannot be used for cryptographic operations.
## Revocation Mechanisms
- Use [[Certificate Revocation Lists (CRLs)]] and [[Online Certificate Status Protocol (OCSP)]]: For systems using Public Key Infrastructure (PKI), maintain [CRLs](https://www.techtarget.com/searchsecurity/definition/Certificate-Revocation-List#:~:text=It%20is%20a%20type%20of,the%20CA%20to%20prevent%20tampering.) or use [OCSP](https://www.ibm.com/docs/en/i/7.4?topic=concepts-online-certificate-status-protocol) to check the revocation status of certificates. These mechanisms allow entities to verify whether a certificate associated with a key is still valid.
- **Implement key status checks:** For non-PKI systems, mechanisms should be implemented to check the status of keys before use. This can involve querying a centralised key management system to verify if a key has been revoked.
## Communicating Revocation
- **Notify stakeholders:** When revoking a key, it's important to notify all stakeholders, including users and systems relying on the key for encryption or authentication. This ensures they can take necessary actions, such as switching to a backup key.
- **Update access controls:** Ensure that access controls are updated to reflect the revocation, preventing revoked keys from being used to access systems or data.