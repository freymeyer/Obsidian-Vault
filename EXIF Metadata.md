---
tags:
  - "#cybersecurity/forensics/metadata"
  - "#cybersecurity/osint"
aliases:
  - EXIF
  - Exchangeable Image File Format
---

# EXIF Metadata

> **One-liner:** Embedded metadata in image files that can record how/when/where a photo was created and which device/software handled it.

## 🎯 What Is It?
EXIF (Exchangeable Image File Format) metadata is a set of standardized fields stored inside many image formats (commonly JPEG). It can include camera make/model, timestamps, exposure settings, orientation, and sometimes GPS coordinates.

## 🤔 Why It Matters
- **Privacy & OPSEC:** EXIF can leak locations, device IDs, or timelines.
- **OSINT:** EXIF provides pivot points (GPS, timestamps, software tags, author fields).
- **Forensics:** Can support (or contradict) claims about when/how an image was produced.

## 🔬 How It Works
### Core Principles
1. EXIF is stored inside the image container, separate from pixel data.
2. Fields are key/value style metadata (some standardized, some vendor-specific).
3. Edits/exports can remove, overwrite, or preserve EXIF depending on tooling.

### Technical Deep-Dive
Typical EXIF fields worth checking:
- **DateTimeOriginal**: when the photo was taken
- **Make/Model**: device identification
- **GPSLatitude/GPSLongitude**: location (if present)
- **Software**: editor/export tool

Extraction example:
```bash
exiftool -G image.jpg
```

## 🛡️ Detection & Prevention
### How to Detect
- Scan public-facing media (websites, social posts, press assets) for GPS and authoring fields.
- In investigations, validate whether EXIF timestamps align with other evidence.

### How to Prevent / Mitigate
- Strip EXIF before external sharing, especially GPS.
- Set camera/app policies to disable location tagging when not needed.

## 📊 Types/Categories
| Type | Description | Example |
|------|-------------|---------|
| **Creation** | When/how created | DateTimeOriginal |
| **Device** | Camera/device info | Make/Model |
| **Location** | Geotagging | GPSLat/GPSLong |
| **Processing** | Edits/exports | Software |

## 🎤 Interview Angles
### Common Questions
- "What is EXIF and what can it leak?"
- "Why are EXIF timestamps not always reliable?"

### STAR Story
> **Situation:** An investigation relied on images as evidence.
> **Task:** Assess reliability of timestamps and geolocation.
> **Action:** Compared EXIF fields to file system times and external corroboration.
> **Result:** Identified inconsistencies and improved the timeline accuracy.

## ✅ Best Practices
- Corroborate EXIF with independent sources.
- Keep originals read-only during analysis.

## ❌ Common Misconceptions
- "EXIF proves an image is authentic" (metadata can be edited).

## 🔗 Related Concepts
- [[ExifTool]]
- [[Open Source Intelligence (OSINT)]]

## 📚 References
- https://www.cipa.jp/e/std/std-sec.html
