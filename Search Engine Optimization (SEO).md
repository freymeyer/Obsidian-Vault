---
tags:
  - "#infrastructure/web"
  - "#interview/concepts"
aliases:
  - SEO
  - Search Engine Optimization
  - SEO Ranking
---

# Search Engine Optimization (SEO)

> **One-liner:** The practice of improving a website's visibility and ranking in search engine results through technical and content optimization.

## 🎯 What Is It?
Search Engine Optimization (SEO) is the process of enhancing a website's structure, content, and technical implementation to rank higher in search engine results pages (SERPs). SEO makes sites more attractive to [[Web Crawler|search engine crawlers]] and improves discoverability for users.

## 🔬 How It Works

### Ranking Factors
Search engines use complex algorithms to score and rank websites. Key factors include:

| Factor | Description | Impact |
|--------|-------------|--------|
| **Keywords** | Relevant terms in content | High |
| **Backlinks** | Links from other sites | High |
| **Page Speed** | Load time performance | Medium |
| **Mobile-Friendly** | Responsive design | High |
| **HTTPS** | Secure connection | Medium |
| **Content Quality** | Original, useful content | High |
| **User Experience** | Easy navigation, low bounce rate | Medium |
| **[[Sitemap]]** | XML sitemap for crawlers | Medium |
| **[[robots.txt]]** | Proper crawler directives | Low |

### SEO Categories

#### 1. **On-Page SEO**
- Content optimization (keywords, headers, meta descriptions)
- Internal linking structure
- URL structure
- Image alt text
- Schema markup

#### 2. **Technical SEO**
- Site speed optimization
- Mobile responsiveness
- SSL/TLS certificates
- XML sitemaps
- Canonical URLs
- Structured data

#### 3. **Off-Page SEO**
- Backlink building
- Social signals
- Brand mentions
- Guest posting

## 📊 SEO Tools & Metrics

### Popular Tools
| Tool | Purpose |
|------|---------|
| **Google Search Console** | Monitor search performance, indexing |
| **Google PageSpeed Insights** | Analyze site speed & performance |
| **Ahrefs / SEMrush** | Backlink analysis, keyword research |
| **Screaming Frog** | Technical SEO audits |
| **Lighthouse** | Performance, accessibility, SEO audit |

### Key Metrics
- **Organic Traffic** — Visitors from search engines
- **Keyword Rankings** — Position for target search terms
- **Backlinks** — Number and quality of inbound links
- **Domain Authority (DA)** — Overall site strength score
- **Click-Through Rate (CTR)** — Clicks / Impressions
- **Core Web Vitals** — User experience metrics (LCP, FID, CLS)

## 🚨 Black Hat vs White Hat SEO

### ✅ White Hat (Legitimate)
- Quality content creation
- Natural backlink building
- Proper keyword usage
- Technical optimization

### ❌ Black Hat (Manipulative)
- Keyword stuffing
- Hidden text/links
- Cloaking (showing different content to crawlers)
- Link farms
- Content scraping

**Consequence:** Search engines penalize or ban sites using black hat techniques.

## 🛡️ Security & SEO

### SEO Poisoning (Attacker Technique)
Attackers compromise websites to inject malicious content with high SEO value:
```
1. Hack vulnerable WordPress site
2. Create hidden pages with trending keywords
3. Optimize for search engines
4. Redirect visitors to phishing/malware sites
5. Site ranks for popular terms → drives traffic to attacker
```

**Detection:**
- Monitor for unauthorized pages/content
- Check for suspicious backlinks
- Review robots.txt and sitemap changes
- Use Google Search Console alerts

### Common SEO Vulnerabilities
- **Exposed .git directories** — Leak source code
- **Open redirects** — Used in phishing campaigns
- **XSS in meta tags** — Inject malicious code
- **SQL injection in search** — Database compromise

## 🎤 Interview Questions
- **"What is SEO and why is it important?"**
  - Process of improving search rankings through technical and content optimization. Increases organic traffic and visibility.
- **"What is the difference between on-page and off-page SEO?"**
  - On-page: Content, keywords, internal structure. Off-page: Backlinks, social signals, external reputation.
- **"How can SEO be weaponized by attackers?"**
  - SEO poisoning: Inject malicious content on compromised sites, optimize for search, redirect traffic to phishing/malware.

## 🎤 Interview STAR Example
> **Situation:** Company's blog was compromised. Attacker injected 500+ hidden pages optimized for trending keywords, linking to pharma spam.
> **Task:** Investigate SEO poisoning attack and remediate as SOC analyst.
> **Action:** Used Google Search Console to identify unauthorized indexed pages. Found exploit via outdated WordPress plugin. Removed malicious pages, patched vulnerability, requested Google to de-index spam URLs.
> **Result:** Malicious pages removed from search results within 1 week. Implemented WAF rules and plugin update policy to prevent recurrence.

## ✅ Best Practices
- Use HTTPS and maintain valid certificates
- Create and submit XML [[Sitemap]]
- Optimize page load speed (compress images, minify CSS/JS)
- Implement responsive design for mobile
- Write descriptive meta titles and descriptions
- Use proper heading hierarchy (H1, H2, H3)
- Build natural backlinks through quality content
- Monitor for SEO poisoning and unauthorized content

## 🔗 Related Concepts
- [[Web Crawler]]
- [[robots.txt]]
- [[Sitemap]]
- [[Google Dorking]]
- [[Cross-Site Scripting (XSS)]]

## 📚 References
- [Google Search Central](https://developers.google.com/search)
- [Moz Beginner's Guide to SEO](https://moz.com/beginners-guide-to-seo)
- OWASP: SEO Poisoning
