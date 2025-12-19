---
tags:
  - "#cybersecurity/red-team/metasploit"
---


- Auxiliary - Any supporting module, such as scanners, crawlers and fuzzers, can be found here.
- Encoders - Encodes the exploit and payload in the hope that a signature-based antivirus solution may miss them.
- Evasion - Evasion will directly attempt to evade the antivirus software with more or less success
- [[Exploit]] - Neatly organized by target system
- NOPs - does nothing, often used as a buffer to achieve consistent payload sizes
- [[Payload]] - codes that will run on the target system.
	- Adapters -  wraps single payloads to convert them into different formats.
	- Singles - do not need to download an additional component to run.
	- Stagers - Responsible for setting up a connection channel between Metasploit and the target system.
	- Stages - Downloaded by the stager. This will allow you to use larger sized payloads.