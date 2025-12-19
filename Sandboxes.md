---
tags:
  - "#cybersecurity/forensics/tools"
---
In cyber security, sandboxes are used to execute potentially dangerous code. Think of this as disposable digital play-pens. These sandboxes are safe, isolated environments where potentially malicious applications can perform their actions without risking sensitive data or impacting other systems.

The use of sandboxes is part of the **golden rule in malware analysis**: **never run dangerous applications on devices you care about**.

Most of the time, sandboxes present themselves as virtual machines. Virtual machines are a popular choice for sandboxing because you can control how the system operates and benefit from features such as snapshotting, which allows you to create and restore the machine to various stages of its status. 

To reiterate, it is **imperative** to understand that potentially malicious code and applications should **only be run in a safe, isolated environment**.