---
tags:
  - "#networking/protocols"
---

Simple Mail Transfer Protocol (SMTP) is a protocol used to send the email to an SMTP server, more specifically to a Mail Submission Agent (MSA) or a Mail Transfer Agent (MTA).

Port 587 with STARTTLS is recommended.

|Server/Port|POP / IMAP|
|---|---|
|Outgoing hostname|smtp.dreamhost.com|
|Port 587|Insecure Transport (Upgraded to a secure connection using STARTTLS)|
|Port 25|Outdated and not recommended. username/password authentication MUST be enabled if using this port.|