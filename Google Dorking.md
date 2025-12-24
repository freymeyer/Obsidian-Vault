---
tags:
  - "#cybersecurity/red-team/reconnaissance"
  - "#interview/concepts"
aliases:
  - Google Hacking
  - Google Fu
  - Dorks
---

# Google Dorking

> **One-liner:** Using advanced Google search operators to discover sensitive information, misconfigurations, and exposed files on the internet.

## 🎯 What Is It?
Google Dorking (also called Google Hacking) is a passive [[Reconnaissance (Cyber Security)|reconnaissance]] technique that uses advanced search operators to find information that shouldn't be publicly accessible—exposed credentials, sensitive documents, vulnerable servers, and misconfigurations.

## 🔬 How It Works

### Common Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `site:` | Limit to specific domain | `site:target.com` |
| `filetype:` | Search specific file types | `filetype:pdf` |
| `intitle:` | Search page titles | `intitle:"index of"` |
| `inurl:` | Search URL content | `inurl:admin` |
| `intext:` | Search page body | `intext:password` |
| `ext:` | File extension | `ext:sql` |
| `cache:` | Cached version | `cache:target.com` |
| `-` | Exclude term | `-site:www.target.com` |

### Example Dorks

```
# Find exposed configuration files
site:target.com filetype:env OR filetype:yml OR filetype:config

# Find login pages
site:target.com inurl:login OR inurl:admin

# Find exposed directories
site:target.com intitle:"index of" 

# Find exposed SQL files
site:target.com filetype:sql "password"

# Find exposed credentials
site:target.com filetype:log "password"

# Find WordPress config files
filetype:txt inurl:wp-config

# Find exposed .git directories
intitle:"index of" ".git"
```

## 📊 Common Targets

| Target | Dork Example |
|--------|--------------|
| **Config files** | `filetype:env DB_PASSWORD` |
| **SQL dumps** | `filetype:sql "INSERT INTO" password` |
| **Log files** | `filetype:log inurl:password` |
| **Backup files** | `filetype:bak OR filetype:old` |
| **Admin panels** | `inurl:admin intitle:login` |
| **Open directories** | `intitle:"index of" parent directory` |
| **Exposed .git** | `intitle:"index of" ".git"` |

## 🛡️ Detection & Prevention

### How to Detect
- **Undetectable by target** - Queries go to Google, not your server
- Use Google Alerts for your domain + sensitive terms
- Check Google Search Console for indexed sensitive content

### How to Prevent / Mitigate
- Proper `robots.txt` configuration (but not security!)
- Remove sensitive files from web roots
- Use `.htaccess` to block directory listings
- Regular external audits using dorks against your own domain
- Authentication on all sensitive endpoints

```
# robots.txt (tells crawlers what NOT to index)
User-agent: *
Disallow: /admin/
Disallow: /backup/
Disallow: *.sql
```

⚠️ **Warning:** `robots.txt` is not a security control—it's a suggestion to crawlers. Attackers can still access these paths directly.

## 🎤 Interview Angles

### Common Questions
- "What is Google Dorking and how would you use it in a pentest?"
- "How can an organization protect against Google Dorking?"
- "What sensitive information have you found using dorks?"

### STAR Story
> **Situation:** Performing external reconnaissance for a client assessment.
> **Task:** Identify exposed sensitive data without active scanning.
> **Action:** Used Google dorks to search for `site:client.com filetype:sql`, `site:client.com filetype:env`, and directory listing dorks.
> **Result:** Found an exposed database backup containing user credentials. Client was unaware the file was publicly indexed.

## ✅ Best Practices
- Always get authorization before dorking a target
- Use Exploit-DB's Google Hacking Database (GHDB) for reference
- Combine with other [[Open Source Intelligence (OSINT)|OSINT]] techniques
- Document all findings with screenshots

## 🔗 Related Concepts
- [[Reconnaissance (Cyber Security)]]
- [[Open Source Intelligence (OSINT)]]
- [[Cyber Kill Chain]]

## 📚 References
- [Google Hacking Database (GHDB)](https://www.exploit-db.com/google-hacking-database)
- OWASP - Google Hacking
