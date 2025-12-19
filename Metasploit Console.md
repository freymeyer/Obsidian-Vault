---
tags:
  - "#cybersecurity/red-team/metasploit"
---

The Metasploit console (msfconsole) can be used just like a regular command-line shell, as you can see below. The first command is `ls` which lists the contents of the folder from which Metasploit was launched using the `msfconsole` command.

```
help
history
set
```

Msfconsole is managed by context; this means that unless set as a global variable, all parameter settings will be lost if you change the module you have decided to use. 

In the example below, we have used the ms17_010_eternalblue exploit, and we have set parameters such as `RHOSTS`.

If we were to switch to another module (e.g. a port scanner), we would need to set the RHOSTS value again as all changes we have made remained in the context of the ms17_010_eternalblue exploit.

Once you type the `use exploit/windows/smb/ms17_010_eternalblue` command, you will see the command line prompt change from msf6 to “msf6 exploit(windows/smb/ms17_010_eternalblue)”. 

The "EternalBlue" is an exploit allegedly developed by the U.S. National Security Agency (N.S.A.) for a vulnerability affecting the SMBv1 server on numerous Windows systems.

The [[Server Message Block (SMB)]] is widely used in Windows networks for file sharing and even for sending files to printers. [[EternalBlue]] was leaked by the cybercriminal group "[[Shadow Brokers]]" in April 2017. In May 2017, this vulnerability was exploited worldwide in the [[WannaCry]] ransomware attack.

Using an exploit
```
msf6 > use exploit/windows/smb/ms17_010_eternalblue
```
Linux commands within a context
```
msf6 exploit(windows/smb/ms17_010_eternalblue) > ls
```
Show options
```
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options
```
Options for a post-exploitation module
```
msf6 post(windows/gather/enum_domain_users) > show options
```
The show payloads command
```
msf6 exploit(windows/smb/ms17_010_eternalblue) > show payloads
```
The back command
```
msf6 exploit(windows/smb/ms17_010_eternalblue) > back
```
The info command
```
msf6 exploit(windows/smb/ms17_010_eternalblue) > info
```
The search command
```
msf6 > search ms17-010
```
Search by module type
```
msf6 > search type:auxiliary telnet
```