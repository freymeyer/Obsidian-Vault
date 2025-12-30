---
dg-publish: true
tags:
  - "#cybersecurity/telemetry/sysmon"
aliases:
  - Sysmon Event 11
---

# Sysmon Event ID 11 — File Create

> One-liner: Sysmon event for file creation operations, including path, process, and user context.

## 🎯 What Is It?
Event 11 logs when a file is created or overwritten. Useful for tracking ransomware note drops, suspicious script writes, and LOLBAS payload staging.

## 🤔 Why It Matters
- Detects ransomware behaviors (e.g., ransom note creation).
- Illuminates staging in temp/user-writable directories.

## 🔬 How It Works
### Core Fields (common)
- `Image` (creating process), `User`, `TargetFilename`, `CreationUtcTime`.

### Detection Ideas
- Alert on creation of `*README*.txt`, `*DECRYPT*.html` in mass.
- Monitor execution + write in `%AppData%`, `%Temp%`, Downloads.

## 🛡️ Detection & Prevention
### How to Detect
- Correlate with Process Create (Event 1) and File Rename (Event 2).

### How to Prevent / Mitigate
- SRP/AppLocker/WDAC to block untrusted processes from writing/executing in user paths.

## 🔗 Related Concepts
- [[Sysmon]]
- [[Atomic Red Team]]
- [[Software Restriction Policies]]
