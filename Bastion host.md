---
tags:
  - infrastructure/cloud
---

Also known as jump servers, are securely configured cloud instances that serve as an isolated access point between the external and internal network resources. They are hardened, monitored, and maintained with strict security controls and can be an ideal environment for secure [[key generation]] due to their isolation and security posture.


**Common Practices**

- **Isolation:** Bastion hosts need to be isolated from the public internet and sometimes internal networks if you need an "air gapped" environment, this minimises exposure to attacks. It also makes keys generated on these hosts less likely to be compromised. You usually access these jump servers with ssh keys or via service/private endpoints used in the backbone networks of cloud providers.
- **Secure access:** Access to bastion hosts is tightly controlled through secure authentication mechanisms, such as SSH keys and [[Multi-Factor Authentication (MFA)]], ensuring that only authorised personnel have access. 
- **Environmental security:** The operating system and applications on bastion hosts are configured with security in mind, including disabling unnecessary services and applying the principle of least privilege.