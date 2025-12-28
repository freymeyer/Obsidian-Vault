---
tags:
  - "#cybersecurity/forensics/evidence-handling"
aliases:
  - CoC
  - Chain of Custody
---

# Chain of Custody

> One-liner: A documented trail showing the collection, control, transfer, analysis, and disposition of evidence.

## 🎯 What Is It?
Chain of custody (CoC) records who handled evidence, when, where, how, and why, ensuring integrity and admissibility in legal or internal proceedings.

## 🤔 Why It Matters
- Preserves evidence credibility and authenticity.
- Enables reproducible forensic workflow and accountability.
- Reduces legal risk and challenges.

## 🔬 How It Works
### Core Principles
1. Minimal handling and documented control.
2. Tamper-evident storage and unique identifiers.
3. Separation of duties and secure transport/storage.

### Technical Deep-Dive
- Evidence forms/logs: item ID, description, time/date, handler, purpose, signatures.
- Cryptographic hashes for images/files; verified at each transfer.
- Storage: sealed bags, lockers, encrypted drives, restricted access.

## 🛡️ Detection & Prevention
### How to Detect
- N/A — process control, not a detection signal.

### How to Prevent / Mitigate
- Use standard CoC templates and digital systems.
- Train staff; audit CoC logs regularly.
- Apply hashing and media handling SOPs.

## 🎤 Interview Angles
- "Walk me through CoC when imaging a suspect workstation."
- "How do you maintain CoC for memory captures?"

## ✅ Best Practices
- Hash-before/after copies; log all transfers.
- Use write blockers for disk imaging.
- Keep CoC with the evidence at all times.

## ❌ Common Misconceptions
- Screenshots or logs alone don’t establish CoC; documentation and control are required.

## 🔗 Related Concepts
- [[Incident Response]]
- [[Disk imaging]]
- [[Sandboxes]]

## 📚 References
- NIST SP 800-101 (Guidelines on Mobile Device Forensics)
