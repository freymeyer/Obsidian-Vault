---
tags:
  - "#cybersecurity/web-security"
  - "#infrastructure/web"
  - "#interview/concepts"
aliases:
  - robots.txt file
  - Robots Exclusion Protocol
---

# robots.txt

> **One-liner:** A text file at the root of a website that tells search engine crawlers which pages they should or shouldn't index.

## 🎯 What Is It?
`robots.txt` is a publicly accessible file located at `https://example.com/robots.txt` that implements the Robots Exclusion Protocol. It provides instructions to [[Web Crawler|web crawlers]] about which parts of a site should be crawled and indexed.

**⚠️ CRITICAL:** `robots.txt` is NOT a security mechanism—it's a polite suggestion. Malicious actors ignore it and use it as a reconnaissance tool.

## 🔬 How It Works

### Basic Structure
```txt
User-agent: *
Disallow: /admin/
Disallow: /private/
Allow: /public/
Crawl-delay: 10
Sitemap: https://example.com/sitemap.xml
```

### Directives

| Directive | Purpose | Example |
|-----------|---------|---------|
| `User-agent:` | Specify which crawler | `User-agent: Googlebot` |
| `Disallow:` | Block path from crawling | `Disallow: /admin/` |
| `Allow:` | Allow specific path | `Allow: /public/` |
| `Crawl-delay:` | Delay between requests (sec) | `Crawl-delay: 5` |
| `Sitemap:` | Location of sitemap | `Sitemap: /sitemap.xml` |

### Wildcard Matching
```txt
# Block all .pdf files
User-agent: *
Disallow: /*.pdf$

# Block all URLs with "private" anywhere
Disallow: /*private*

# Block query parameters
Disallow: /*?
```

## 🚨 Security Implications

### ❌ Common Misuse
```txt
# DON'T DO THIS - You're giving attackers a roadmap!
User-agent: *
Disallow: /admin/
Disallow: /backup/
Disallow: /config/
Disallow: /database/
Disallow: /.env
Disallow: /api/internal/
```

**Why this is bad:**
- Tells attackers EXACTLY where sensitive content is
- They'll ignore the `Disallow` and visit anyway
- Creates a target list for [[Google Dorking]]

### ✅ Proper Approach
- **Use authentication** for sensitive directories
- Don't list sensitive paths in `robots.txt`
- Use `.htaccess`, firewall rules, or app-level auth

## 🛡️ Blue Team Perspective

### Detection
- Monitor access to paths in `robots.txt` `Disallow` entries
- Alert on suspicious User-Agents ignoring directives
- Log patterns: Crawlers should respect directives; attackers won't

### Defense
```
1. Don't document sensitive paths in robots.txt
2. Use proper access controls (not obscurity)
3. Monitor robots.txt requests for reconnaissance attempts
4. Remove outdated entries that leak information
```

### SIEM Query Example
```spl
# Detect access to disallowed paths
index=web sourcetype=access_combined
| search uri_path="/admin/*" OR uri_path="/backup/*"
| stats count by src_ip, uri_path, user_agent
| where user_agent != "Googlebot" AND user_agent != "Bingbot"
```

## ⚔️ Red Team Perspective

### Reconnaissance Value
```bash
# Fetch robots.txt
curl https://target.com/robots.txt

# Look for juicy targets:
# - /admin, /backup, /config
# - /api/internal, /dev, /staging
# - File patterns: *.sql, *.bak, *.env
```

### Google Dorking
```
# Find sites with sensitive robots.txt entries
inurl:robots.txt intext:admin
inurl:robots.txt intext:backup
inurl:robots.txt intext:password
```

### Tools
- **Burp Suite** — Spider ignores robots.txt
- **dirb/gobuster** — Custom wordlists from robots.txt
- **RobotsDisallowed** — GitHub repo of 500k+ robots.txt files

## 📂 Real-World Examples

### Good Example
```txt
# Prevents duplicate content indexing
User-agent: *
Disallow: /search?
Disallow: /*?sort=
Crawl-delay: 1
Sitemap: https://example.com/sitemap.xml
```

### Bad Example (Information Leakage)
```txt
# Actual leak from a real company (anonymized)
User-agent: *
Disallow: /admin-panel/
Disallow: /old-site-backup/
Disallow: /customer-data/
Disallow: /.git/
```

## 🎤 Interview Questions
- **"What is robots.txt and what is it used for?"**
  - Tells search engine crawlers which paths to crawl/skip. Located at `/robots.txt`. NOT a security control.
- **"Can robots.txt prevent attackers from accessing sensitive directories?"**
  - No! It's a suggestion for legitimate crawlers. Attackers ignore it.
- **"How can robots.txt be used in reconnaissance?"**
  - Lists potentially sensitive paths. Attackers use it as a roadmap for what to target.

## 🎤 Interview STAR Example
> **Situation:** During recon, found target's robots.txt disclosed `/admin-backup/` path containing SQL dumps.
> **Task:** Exploit the misconfiguration as part of authorized pentest.
> **Action:** Accessed the disallowed path directly (ignored robots.txt). Found SQL backup files with customer PII. Documented as **High** severity finding.
> **Result:** Client removed sensitive paths from robots.txt and implemented authentication. Moved backups offline.

## ✅ Best Practices
- Use robots.txt for **SEO**, not security
- Don't list sensitive paths—use authentication instead
- Regularly audit robots.txt for information leaks
- Block duplicate content, not admin panels
- Combine with [[Sitemap]] for better SEO

## 🔗 Related Concepts
- [[Web Crawler]]
- [[Sitemap]]
- [[Google Dorking]]
- [[Search Engine Optimization (SEO)]]
- [[Reconnaissance (Cyber Security)]]
- [[Information Disclosure]]

## 📚 References
- [RFC 9309: Robots Exclusion Protocol](https://www.rfc-editor.org/rfc/rfc9309.html)
- [Google: robots.txt Specifications](https://developers.google.com/search/docs/crawling-indexing/robots/robots_txt)
- OWASP: Information Disclosure via robots.txt
