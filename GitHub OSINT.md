---
tags:
  - "#cybersecurity/osint"
  - "#cybersecurity/red-team/recon"
aliases:
  - GitHub Recon
  - GitHub Intelligence
---

# GitHub OSINT

> **One-liner:** Using public GitHub profiles, repos, issues, and commits to discover identities, emails, infrastructure, and project context.

## 🎯 What Is It?
GitHub OSINT is the practice of pivoting off publicly available GitHub data (user profiles, repositories, commit metadata, READMEs, issues, and code) to gather intelligence.

Common findings:
- Personal/work emails (commit author fields)
- Usernames reused across platforms
- Internal hostnames/URLs in configs
- API endpoints, tokens (misconfigurations)
- Technology stack and org structure

## 🤔 Why It Matters
- **Recon:** Identify developer identity and attack surface.
- **Defense:** Detect accidental secret exposure and reduce public footprint.
- **Investigations:** Attribute tooling and link aliases.

## 🔬 How It Works
### Core Principles
1. Git history preserves author metadata.
2. Repos often contain environment-specific artifacts (configs, docs).
3. Search is powerful (GitHub search, code search, commit search).

### Technical Deep-Dive
Practical workflow:
- Pivot from username → profile → repos
- Check repo README/issues for breadcrumbs
- Inspect commits for author emails
- Search within a repo for `password`, `token`, `secret`, `apikey`

## 🛡️ Detection & Prevention
### How to Detect
- Run secret scanners on repos (pre-commit + CI).
- Monitor org repos for sensitive strings.

### How to Prevent / Mitigate
- Use `.gitignore` correctly; never commit secrets.
- Rotate secrets immediately if leaked.
- Educate devs on commit metadata exposure.

## 📊 Types/Categories
| Type | Description | Example |
|------|-------------|---------|
| **Identity** | Names/emails/usernames | commit author email |
| **Tech stack** | Languages/frameworks/tools | package manifests |
| **Infrastructure** | URLs/domains/internal hints | config files |

## 🎤 Interview Angles
### Common Questions
- "What can attackers learn from GitHub?"
- "How do you prevent secrets from reaching git history?"

### STAR Story
> **Situation:** A team suspected a token was leaked.
> **Task:** Confirm exposure and reduce recurrence.
> **Action:** Searched history, confirmed leak, rotated token, added pre-commit scanning.
> **Result:** Stopped further abuse and improved SDLC hygiene.

## ✅ Best Practices
- Use secret scanning + protected branches.
- Minimize sensitive data in commit messages and docs.

## ❌ Common Misconceptions
- "Deleting a file removes the secret" (history may still contain it).

## 🔗 Related Concepts
- [[Open Source Intelligence (OSINT)]]
- [[Passive Reconnaissance]]

## 📚 References
- GitHub secret scanning docs
