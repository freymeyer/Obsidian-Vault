---
tags:
  - "#infrastructure/web"
  - "#interview/concepts"
aliases:
  - sitemap.xml
  - XML Sitemap
---

# Sitemap

> **One-liner:** An XML file that lists all important pages on a website to help search engine crawlers discover and index content efficiently.

## 🎯 What Is It?
A sitemap (specifically `sitemap.xml`) is a structured file located at `https://example.com/sitemap.xml` that provides [[Web Crawler|search engine crawlers]] with a roadmap of a website's content. It lists URLs, metadata, and relationships to improve crawling efficiency and [[Search Engine Optimization (SEO)|SEO]].

## 🔬 How It Works

### Basic Structure
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  
  <url>
    <loc>https://example.com/</loc>
    <lastmod>2024-01-15</lastmod>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>
  
  <url>
    <loc>https://example.com/products/</loc>
    <lastmod>2024-01-10</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
  
  <url>
    <loc>https://example.com/blog/post-1/</loc>
    <lastmod>2024-01-05</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.6</priority>
  </url>

</urlset>
```

### XML Elements

| Element | Description | Example |
|---------|-------------|---------|
| `<loc>` | Full URL of the page | `https://example.com/page` |
| `<lastmod>` | Last modification date | `2024-01-15` |
| `<changefreq>` | How often content changes | `daily`, `weekly`, `monthly` |
| `<priority>` | Relative importance (0.0-1.0) | `1.0` (highest), `0.5` (medium) |

## 📊 Sitemap Types

### 1. **XML Sitemap** (Most Common)
- Standard format for search engines
- Located at `/sitemap.xml`
- Can be split into multiple files for large sites

### 2. **HTML Sitemap**
- Human-readable page with links
- Helps users navigate
- Located at `/sitemap.html` or `/sitemap/`

### 3. **Image Sitemap**
```xml
<url>
  <loc>https://example.com/photo.html</loc>
  <image:image>
    <image:loc>https://example.com/photo.jpg</image:loc>
    <image:title>Photo Title</image:title>
  </image:image>
</url>
```

### 4. **Video Sitemap**
```xml
<url>
  <loc>https://example.com/video.html</loc>
  <video:video>
    <video:thumbnail_loc>https://example.com/thumb.jpg</video:thumbnail_loc>
    <video:title>Video Title</video:title>
  </video:video>
</url>
```

## 🚨 Security Implications

### ⚠️ Information Disclosure
Sitemaps can reveal:
- Internal URL structure
- Admin/staging environments (if misconfigured)
- API endpoints
- Hidden/unlisted pages
- Development paths

### Red Team Reconnaissance
```bash
# Fetch sitemap
curl https://target.com/sitemap.xml

# Common locations
/sitemap.xml
/sitemap_index.xml
/sitemap1.xml
/sitemap-product.xml

# Look for:
- /admin, /api, /dev paths
- High-value targets (dashboards, reports)
- Parameter patterns for fuzzing
```

### Blue Team Detection
```
Monitor for:
1. Unauthorized changes to sitemap.xml
2. Sitemap poisoning (malicious URLs injected)
3. Excessive sitemap requests (reconnaissance)
4. Sensitive paths accidentally included
```

## 📂 Real-World Examples

### Good Example
```xml
<!-- Public content only -->
<url>
  <loc>https://example.com/</loc>
  <priority>1.0</priority>
</url>
<url>
  <loc>https://example.com/products/</loc>
  <priority>0.8</priority>
</url>
```

### Bad Example (Leaking Sensitive Paths)
```xml
<!-- DON'T DO THIS -->
<url>
  <loc>https://example.com/admin/dashboard/</loc>
</url>
<url>
  <loc>https://example.com/api/internal/users/</loc>
</url>
<url>
  <loc>https://example.com/staging/</loc>
</url>
```

## 🔧 Sitemap Management

### Tools to Generate Sitemaps
- **Yoast SEO** (WordPress plugin)
- **Screaming Frog** (Desktop crawler)
- **xml-sitemaps.com** (Online generator)
- **sitemap.js** (Node.js library)

### Submitting to Search Engines
```
Google Search Console: https://search.google.com/search-console
Bing Webmaster Tools: https://www.bing.com/webmasters

Or add to robots.txt:
Sitemap: https://example.com/sitemap.xml
```

### Sitemap Index (for large sites)
```xml
<!-- sitemap_index.xml -->
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <sitemap>
    <loc>https://example.com/sitemap-products.xml</loc>
  </sitemap>
  <sitemap>
    <loc>https://example.com/sitemap-blog.xml</loc>
  </sitemap>
</sitemapindex>
```

## 🎤 Interview Questions
- **"What is a sitemap and why is it important?"**
  - XML file listing site URLs to help crawlers index content efficiently. Improves SEO by ensuring all pages are discovered.
- **"What's the difference between sitemap.xml and robots.txt?"**
  - Sitemap tells crawlers what TO index (whitelist). robots.txt tells them what NOT to index (blacklist).
- **"Can a sitemap pose security risks?"**
  - Yes, if it exposes sensitive paths like admin panels, APIs, or staging environments. Only include public content.

## 🎤 Interview STAR Example
> **Situation:** Pentest revealed client's sitemap.xml included admin dashboard and internal API endpoints.
> **Task:** Report as finding and recommend remediation.
> **Action:** Documented as Medium severity information disclosure. Recommended removing non-public URLs from sitemap and implementing authentication on admin paths.
> **Result:** Client updated sitemap to exclude sensitive paths. No admin URLs discoverable via search engines post-fix.

## ✅ Best Practices
- Only include **public** pages in sitemap
- Update lastmod dates when content changes
- Keep sitemap under 50MB / 50,000 URLs (use index for larger)
- Submit to Google Search Console & Bing Webmaster Tools
- Reference sitemap in [[robots.txt]]: `Sitemap: /sitemap.xml`
- Monitor for unauthorized modifications
- Exclude staging/dev environments
- Use dynamic generation for large sites

## 🔗 Related Concepts
- [[Web Crawler]]
- [[robots.txt]]
- [[Search Engine Optimization (SEO)]]
- [[Google Dorking]]
- [[Reconnaissance (Cyber Security)]]
- [[Information Disclosure]]

## 📚 References
- [sitemaps.org Protocol](https://www.sitemaps.org/protocol.html)
- [Google: Build and Submit a Sitemap](https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap)
- OWASP: Information Disclosure via Sitemap
