---
dg-publish: true
tags:
  - "#cybersecurity/osint"
  - "#cybersecurity/forensics/metadata"
  - "#interview/concepts"
aliases:
  - exiftool
---

# ExifTool

> **One-liner:** A command-line tool for reading, writing, and manipulating metadata (especially EXIF) in files like images and PDFs.

## 🎯 What Is It?
ExifTool is a widely used metadata utility (by Phil Harvey) that can extract embedded metadata from many file formats. In OSINT and DFIR, it’s commonly used to pull out identifiers and context that creators didn’t intend to share (e.g., camera info, timestamps, GPS coordinates, author/copyright fields).

## 🤔 Why It Matters
- **OSINT pivoting:** Metadata can expose usernames, emails, device model, software versions, or locations.
- **Forensics:** Helps validate timelines and determine if a file was edited/processed.
- **Security:** Metadata can leak sensitive operational details (locations, internal usernames, tooling).

## 🔬 How It Works
### Core Principles
1. Many file types support embedded metadata blocks (EXIF/IPTC/XMP).
2. Tools can parse these blocks without “opening” the file in the normal application.
3. Metadata is often copied forward when files are re-shared, resized, or reposted (not always).

### Technical Deep-Dive
Common usage patterns:
```bash
# Dump all metadata
exiftool image.jpg

# Show only GPS-related fields
exiftool -gps:all image.jpg

# Show the field groups (EXIF/IPTC/XMP)
exiftool -G image.jpg

# Output as JSON (useful for scripting)
exiftool -j image.jpg
```

## 🛡️ Detection & Prevention
### How to Detect
- Detect uploads containing EXIF/GPS fields at web gateways or DLP tooling.
- In IR, inspect suspicious images/docs for embedded authoring data.

### How to Prevent / Mitigate
- Strip metadata before sharing externally (e.g., “Remove location data” options, export settings, or metadata-scrubbing tools).
- Enforce secure defaults in MDM/endpoint tooling for camera/location tagging where appropriate.

## 📊 Types/Categories
| Type | Description | Example |
|------|-------------|---------|
| **EXIF** | Camera/photo metadata | GPS, camera make/model |
| **XMP** | Extensible metadata (Adobe/common) | Editing history, creator tool |
| **IPTC** | Publishing/news metadata | Copyright, captioning |

## 🎤 Interview Angles
### Common Questions
- "What kinds of metadata can leak from images?"
- "How would you validate whether an image contains GPS coordinates?"

### STAR Story
> **Situation:** A suspected data leak involved screenshots shared publicly.
> **Task:** Determine if shared files exposed internal identifiers.
> **Action:** Extracted metadata, identified authoring tool/user fields, and recommended metadata stripping controls.
> **Result:** Reduced future leakage and improved sharing guidance.

## ✅ Best Practices
- Treat metadata as **untrusted input** (it can be forged).
- Always record hashes of original files before modifying them.

## ❌ Common Misconceptions
- "Social media always strips metadata" (it depends on the platform and workflow).

## 🔗 Related Concepts
- [[EXIF Metadata]]
- [[Open Source Intelligence (OSINT)]]
- [[Passive Reconnaissance]]

## 📚 References
- https://exiftool.org/
