---
tags:
  - "#meta"
---

# 📘 Vault Style Guide

> Standards for maintaining a clean, consistent, and interview-ready vault.

---

## 📁 Note Naming Conventions

| Type | Format | Example |
|------|--------|---------|
| MOC | `0XX Name MOC.md` | `011 🛡️ Blue Team & SOC Operations MOC.md` |
| Concept | `Title Case.md` | `Authentication.md` |
| Tool | `Tool Name.md` | `Nmap.md` |
| CVE | `CVE-YYYY-XXXXX.md` | `CVE-2024-21413.md` |
| Vulnerability | `Full Name.md` | `Cross-Site Scripting (XSS).md` |
| Acronym Note | `Full Name (ACRONYM).md` | `Endpoint detection and response (EDR).md` |

---

## 🏷️ Tag Hierarchy

Use this standardized hierarchy for all notes:

```
#cybersecurity/
├── blue-team/
│   ├── detection
│   ├── siem
│   ├── incident-response
│   └── soc
├── red-team/
│   ├── recon
│   ├── exploitation
│   ├── post-exploitation
│   └── c2
├── web-sec/
│   ├── injection
│   ├── auth
│   ├── access-control
│   └── owasp
├── cve/
├── malware/
├── forensics/
├── iam/
│   └── access-control
└── crypto/

#tools/
├── scanning
├── exploitation
├── forensics
└── defense

#interview/
├── concepts
├── scenarios
└── star-stories

#learning/
├── study-note
├── ctf
└── project

#moc
```

### Tag Rules
- Always include the `#` in frontmatter: `- "#cybersecurity/web-sec"`
- Use lowercase with hyphens: `#cybersecurity/blue-team` ✅
- Never: `#CyberSecurity/BlueTeam` ❌

---

## 📝 Note Structure Standards

### Every Note Should Have

1. **Frontmatter** with tags and aliases
2. **One-liner definition** (blockquote format)
3. **What Is It?** section
4. **Related Concepts** section with links
5. **References** section (even if empty placeholder)

### Vulnerability Notes Must Include

- [ ] Severity / OWASP ranking
- [ ] Impact (CIA triad affected)
- [ ] Code examples (vulnerable + secure)
- [ ] Detection methods
- [ ] STAR story for interviews
- [ ] Prevention/remediation table

### CVE Notes Must Include

- [ ] Summary table (CVSS, affected products)
- [ ] Interview quick-hits section
- [ ] Detection signatures (YARA/Sigma)
- [ ] Related vulnerability type link

---

## 🎯 STAR Format for Interviews

Use this format in vulnerability and concept notes:

```markdown
## 🛠️ Real-World Example (STAR Format)
> **Situation:** [Context - what was happening]
> **Task:** [Your responsibility]
> **Action:** [Specific steps YOU took]
> **Result:** [Measurable outcome]
```

**Tips:**
- Be specific with numbers: "reduced by 95%" not "improved significantly"
- Focus on YOUR actions, not the team's
- Keep each section to 1-2 sentences
- Prepare 5-7 stories that cover different skills

---

## 🔗 Linking Best Practices

| Do ✅ | Don't ❌ |
|-------|---------|
| `[[Authentication]]` | Mentioning without linking |
| `[[Privilege Escalation#Horizontal]]` | Linking to wrong sections |
| `**Related:** [[A]], [[B]]` | Orphan notes with no links |
| Link to existing notes | Creating duplicates |

### Orphan Note Check
Regularly run Graph View and look for disconnected nodes.

---

## 📋 Using Templates

1. Open Command Palette (`Ctrl+P`)
2. Type "Template"
3. Select "Insert template"
4. Choose appropriate template:
   - **Vulnerability Template** — XSS, SQLi, IDOR, etc.
   - **CVE Template** — Specific CVE entries
   - **Tool Template** — Nmap, Burp, Wireshark, etc.
   - **Concept Template** — Defense in Depth, Zero Trust, etc.

---

## ✅ Quality Checklist Before "Done"

- [ ] Frontmatter has correct tags
- [ ] One-liner definition exists
- [ ] At least 3 internal links
- [ ] STAR story (for interview-relevant notes)
- [ ] Code examples have language tags (```python, ```bash)
- [ ] Tables are formatted correctly
- [ ] No broken links (check in preview mode)

---

## 🔄 Maintenance Tasks

### Weekly
- [ ] Review unlinked mentions
- [ ] Check for orphan notes in graph
- [ ] Add content to 2-3 stub notes

### Monthly
- [ ] Tag audit (search for typos)
- [ ] Update Interview Prep MOC with new content
- [ ] Review recent CVEs for relevance
