---
tags:
  - "#cybersecurity/cryptography/keys"
---

Closely related to key rotation is the concept of critical backup and recovery. This involves creating secure "copies" of cryptographic keys to avoid losing access to keys in case of deletion or corruption. Effective backup and recovery strategies ensure that keys can be restored securely and controlled, maintaining the integrity and availability of encrypted information.

Note: Sometimes, old or replaced keys must be "archived" rather than destroyed (see Key Destruction section). Archiving is necessary for decrypting old communications or data, should the need arise. Secure key archiving ensures that keys are preserved to prevent unauthorised access while still being retrievable for authorised purposes. Sometimes, "purge periods" are set so that when you delete a key, you can still retrieve it after x amount of days before full deletion.
