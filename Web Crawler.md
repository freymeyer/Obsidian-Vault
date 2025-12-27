---
tags:
  - "#cybersecurity/web-security"
  - "#infrastructure/web"
  - "#interview/concepts"
aliases:
  - Web Spider
  - Web Bot
  - Search Engine Bot
  - Crawler
---

# Web Crawler

> **One-liner:** An automated bot that systematically browses and indexes web content by following links and collecting information.

## 🎯 What Is It?
A web crawler (also called a spider or bot) is an automated program used by search engines to discover, scan, and index web content. Crawlers start at known URLs (seed pages), extract content and links, then recursively visit discovered URLs to map the web's structure and content.

## 🔬 How It Works

### Crawling Process
```
1. Start with seed URLs (e.g., popular sites, sitemaps)
2. Fetch webpage content via HTTP requests
3. Parse HTML and extract:
   - Text content & keywords
   - Links to other pages
   - Metadata (title, description, headers)
   - Media references
4. Add discovered URLs to crawl queue
5. Store indexed data in search engine database
6. Respect robots.txt directives
7. Repeat for each URL in queue
```

### Example Crawler Behavior
```
Crawler visits: https://example.com
├── Indexes keywords: "Python", "Tutorial", "Web Development"
├── Finds links:
│   ├── https://example.com/about
│   ├── https://example.com/contact
│   └── https://another-site.com (external)
└── Adds all URLs to crawl queue

Next iteration: Crawls /about, /contact, etc.
```

## 🤖 Common Crawlers

| Crawler | Search Engine | User-Agent |
|---------|---------------|------------|
| **Googlebot** | Google | `Mozilla/5.0 (compatible; Googlebot/2.1)` |
| **Bingbot** | Microsoft Bing | `Mozilla/5.0 (compatible; bingbot/2.0)` |
| **Slurp** | Yahoo | `Mozilla/5.0 (compatible; Yahoo! Slurp)` |
| **DuckDuckBot** | DuckDuckGo | `DuckDuckBot/1.0` |
| **Applebot** | Apple | `Mozilla/5.0 (compatible; Applebot/0.1)` |

## 🛡️ Security Implications

### For Defenders
- Crawlers respect [[robots.txt]] directives (but attackers don't)
- Sensitive directories in `robots.txt` = roadmap for attackers
- Crawlers can discover:
  - Backup files (`.bak`, `.old`)
  - Configuration files (`.env`, `.config`)
  - Admin panels
  - Development/staging environments

### For Red Teams
- Use crawler behavior for [[Reconnaissance (Cyber Security)|reconnaissance]]
- [[Google Dorking]] leverages indexed crawler data
- Custom crawlers for targeted enumeration
- Tools: **Burp Spider**, **Scrapy**, **BeautifulSoup**

## 🔧 Crawler Control Mechanisms

### 1. robots.txt
```txt
User-agent: *
Disallow: /admin/
Disallow: /private/
Crawl-delay: 10
```

### 2. Meta Tags
```html
<!-- Prevent indexing this page -->
<meta name="robots" content="noindex, nofollow">
```

### 3. HTTP Headers
```http
X-Robots-Tag: noindex, nofollow
```

### 4. Rate Limiting
- Detect crawler behavior (User-Agent, request patterns)
- Implement rate limits to prevent abuse
- Use CAPTCHA for suspicious traffic

## 🎤 Interview Questions
- **"How do search engine crawlers work?"**
  - Explain seed URLs → fetch → parse → extract links → queue → repeat cycle
- **"What is robots.txt and can it prevent attacks?"**
  - It's a **suggestion**, not security control. Malicious actors ignore it.
- **"How would you prevent sensitive content from being indexed?"**
  - Use authentication, not robots.txt. Apply `noindex` meta tags, remove from sitemap.

## 🎤 Interview STAR Example
> **Situation:** Organization's staging environment appeared in Google search results, exposing unreleased features and test credentials.
> **Task:** Prevent staging servers from being indexed while maintaining development workflow.
> **Action:** Implemented HTTP basic auth on staging. Added `X-Robots-Tag: noindex` headers. Removed staging URLs from sitemap. Verified de-indexing using Google Search Console.
> **Result:** Staging environment removed from search results within 2 weeks. No further exposure of pre-production data.

## ✅ Best Practices
- Never rely on `robots.txt` for security
- Use authentication for sensitive areas
- Monitor for unauthorized crawling (unusual User-Agents)
- Include legitimate resources in [[Sitemap]] for SEO
- Set `noindex` for staging/dev environments

## 🔗 Related Concepts
- [[robots.txt]]
- [[Sitemap]]
- [[Google Dorking]]
- [[Search Engine Optimization (SEO)]]
- [[Reconnaissance (Cyber Security)]]
- [[Open Source Intelligence (OSINT)]]

## 📚 References
- Google Search Central: How Googlebot Works
- OWASP: Web Crawler Guidelines
- Robots Exclusion Protocol (REP)
