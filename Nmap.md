---
tags:
  - "#networking/tools"
  - "#tools/scanning"
---

Nmap (Network Mapper) is a open-source tool used for network discovery and security auditing. It also assists in the exploration of network hosts and services, providing information about open ports, operating systems, and other details.

## Common Query
Basic Nmap Scan
```
nmap 10.48.157.102
```
Specifying Port Range
```
nmap -p- --script=banner 10.48.157.102
```
Scanning [[User Datagram Protocol (UDP)]] Ports
```
nmap -sU 10.48.157.102
```
## Common Ports
| [[Secure Shell (SSH)]]                  | 22       |
| --------------------------------------- | -------- |
| [[Hypertext Transfer Protocol (HTTP)]]  | 20       |
| [[MySQL]]                               | 3306     |
| vsFTPd [[File Transfer Protocol (FTP)]] | 220      |
| [[Domain Name System (DNS)]] Server     | 53 (UDP) |
