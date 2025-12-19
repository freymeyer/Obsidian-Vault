---
tags:
  - Tool
---
It's a powerful command-line tool for querying DNS servers, helping you troubleshoot issues, verify DNS records (A, MX, CNAME, etc.), check propagation, and see the full resolution path by specifying servers like dig @8.8.8.8 google.com. It's a modern, detailed replacement for nslookup and is standard on Linux/macOS, providing deep insights into DNS resolution steps, unlike simpler tools. 

Getting the key 
```
dig @10.48.157.102 TXT key3.tbfc.local +short
```
