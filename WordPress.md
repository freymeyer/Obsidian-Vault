---
dg-publish: true
tags:
  - "#cybersecurity/web-security/platforms"
aliases:
  - WP
---

# WordPress

> **One-liner:** A popular PHP-based CMS whose themes/plugins and exposed endpoints make it a common target for web attacks and recon.

## 🎯 What Is It?
WordPress is a content management system (CMS) used to build websites and blogs. It is extensible via themes and plugins and typically runs on a LAMP/LEMP-style stack.

## 🤔 Why It Matters
- **Attack surface:** Plugins/themes can introduce vulnerabilities.
- **Recon:** Public pages, RSS feeds, and common paths can leak usernames, versions, and structure.
- **Defense:** Hardening and patch discipline are critical due to ecosystem size.

## 🔬 How It Works
### Core Principles
1. Core WordPress handles routing, content, and admin UI.
2. Plugins add features and often expose new endpoints.
3. Themes control presentation and can include custom PHP/JS.

### Technical Deep-Dive
Common identifiers/paths:
- `/wp-admin/` (admin login)
- `/wp-content/` (themes/plugins/uploads)
- REST API endpoints (often `/wp-json/`)

## 🛡️ Detection & Prevention
### How to Detect
- Monitor for brute force attempts on `/wp-login.php` and `/wp-admin/`.
- Detect plugin exploit attempts and suspicious file uploads.

### How to Prevent / Mitigate
- Keep core/plugins/themes updated.
- Remove unused plugins/themes.
- Enforce MFA for admin accounts.
- Restrict file editing and harden permissions.

## 📊 Types/Categories
| Type | Description | Example |
|------|-------------|---------|
| **Core** | WordPress engine | version updates |
| **Plugin** | Feature extension | forms, SEO tools |
| **Theme** | UI/layout | custom theme |

## 🎤 Interview Angles
### Common Questions
- "Why is WordPress frequently targeted?"
- "What are practical WordPress hardening steps?"

### STAR Story
> **Situation:** A WordPress site had repeated auth attacks.
> **Task:** Reduce compromise risk.
> **Action:** Enabled MFA, patched plugins, limited login attempts, and improved monitoring.
> **Result:** Reduced attack success and improved detection.

## ✅ Best Practices
- Inventory plugins and patch quickly.
- Back up regularly and test restores.

## ❌ Common Misconceptions
- "WordPress is insecure by default" (risk often comes from outdated components and plugins).

## 🔗 Related Concepts
- [[View Page Source]]
- [[Google Dorking]]

## 📚 References
- https://wordpress.org/support/article/hardening-wordpress/
