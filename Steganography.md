---
dg-publish: true
tags:
  - "#cybersecurity/red-team/evasion"
  - "#cybersecurity/forensics"
  - "#interview/concepts"
aliases:
  - Stego
  - Data Hiding
---

# Steganography

> **One-liner:** The art and science of hiding data inside innocuous carrier files (images, audio, video) to conceal its very existence.

## 🎯 What Is It?
Steganography conceals information within other non-secret data, making the hidden message invisible to casual observation. Unlike [[Cryptography & Identity MOC|encryption]], which scrambles data so it's unreadable, steganography hides that there's any secret data at all.

**Key difference:**
- **Cryptography:** "I'm sending a secret message" (visible but unreadable)
- **Steganography:** "There is no message here" (invisible)

## 🤔 Why It Matters

### Red Team / Offensive
- Exfiltrate data without triggering [[Data Loss Prevention (DLP)]] alerts
- Hide malicious payloads inside legitimate-looking images
- Evade network monitoring (hidden C2 communication)

### Blue Team / Defensive
- [[Data Exfiltration]] vector to monitor
- Used by threat actors to smuggle malware
- Requires specialized tools for detection

### Forensics
- Evidence can be hidden inside media files
- Requires steganalysis to detect hidden content
- Part of [[Malware Analysis & Forensics MOC|digital forensics]] investigation

## 🔬 How It Works

### Core Principles
1. **Carrier File:** The innocent-looking file (image, audio, video)
2. **Payload:** The hidden data embedded inside
3. **Embedding Algorithm:** Method used to hide data (LSB, EOF, etc.)
4. **Password/Key:** Optional encryption of hidden data

### Technical Deep-Dive

#### Least Significant Bit (LSB) Injection
```
Original pixel RGB: (11010101, 10101010, 11110000)
Hidden bit: 1

Modified pixel:     (11010101, 10101011, 11110000)
                                      ^
                              Changed LSB
```

The change is imperceptible to human eyes but carries hidden data.

#### Common Embedding Locations
| Method | Description | Detectability |
|--------|-------------|---------------|
| **LSB** | Modify least significant bits of pixels | Low (most common) |
| **EOF (End of File)** | Append data after file end marker | Medium (file size increases) |
| **Metadata** | Embed in [[EXIF Metadata\|EXIF]]/comments | High (easily checked) |
| **Whitespace** | Use invisible characters in text | Medium (unusual patterns) |

### Example: Hiding Text in Image
```bash
# Embed secret.txt inside image.jpg (steghide)
steghide embed -cf image.jpg -ef secret.txt -p MyPassword

# Extract hidden data
steghide extract -sf image.jpg -p MyPassword
```

## 🛠️ Common Tools

| Tool | Purpose | Use Case |
|------|---------|----------|
| **steghide** | Hide/extract data from JPEG/WAV/BMP | General purpose stego |
| **steghide** | LSB steganography for PNG/BMP | CTF challenges |
| **exiftool** | Read/write metadata | Check for hidden data in metadata |
| **binwalk** | Analyze files for embedded data | Forensic analysis |
| **stegsolve** | Analyze images bit-by-bit | CTF / forensics |
| **zsteg** | PNG/BMP stego detection | Detect LSB stego |
| **OpenStego** | GUI stego tool | User-friendly hiding/extraction |

### Installation (Linux)
```bash
# Steghide
sudo apt install steghide

# Binwalk
sudo apt install binwalk

# Zsteg (Ruby gem)
gem install zsteg
```

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)
- **File size anomalies:** Carrier files larger than expected
- **Statistical analysis:** Unusual bit distribution (LSB anomalies)
- **Visual analysis:** Noise or artifacts in images
- **Steganalysis tools:** Automated detection (StegExpose, StegSecret)
- **Network monitoring:** Unexpected image uploads/downloads

### How to Prevent / Mitigate
- Strip metadata from all uploaded files
- Re-encode images on upload (destroys hidden data)
- Implement [[Data Loss Prevention (DLP)]] with stego detection
- Monitor outbound traffic for suspicious media transfers
- Baseline file sizes for known assets

### Detection Commands
```bash
# Check file for embedded data
binwalk image.jpg

# Extract all embedded files
binwalk -e image.jpg

# Analyze PNG/BMP for LSB stego
zsteg image.png

# Check metadata
exiftool image.jpg
```

## 📊 Types/Categories

| Type | Carrier Medium | Example |
|------|----------------|---------|
| **Image Stego** | JPEG, PNG, BMP | Most common in CTFs |
| **Audio Stego** | WAV, MP3, FLAC | Spectrograms, LSB |
| **Video Stego** | MP4, AVI | High capacity, complex |
| **Text Stego** | TXT, HTML, source code | Whitespace, Zero-width chars |
| **Network Stego** | Protocol headers, timing | Covert channels |

## 🎤 Interview Angles

### Common Questions
- **"What is steganography and how does it differ from encryption?"**
  - *"Steganography hides the existence of data within innocuous files like images, while encryption makes data unreadable but still visible. Stego says 'there's nothing here,' crypto says 'you can't read this.'"*

- **"How would you detect steganography in your environment?"**
  - *"Monitor for unusual file uploads, analyze statistical properties of images for LSB anomalies, strip and re-encode all uploaded media to destroy hidden data, and deploy steganalysis tools at network boundaries."*

- **"Give an example of steganography being used maliciously"**
  - *"Threat actors have used steganography to hide [[Command and Control (C2)|C2]] commands inside memes posted on social media, or to exfiltrate data by embedding it in product images uploaded to e-commerce sites."*

### STAR Story
> **Situation:** During a [[Cyber Threat Intelligence (CTI)|threat intelligence]] investigation, we suspected an insider was exfiltrating sensitive documents, but [[Data Loss Prevention (DLP)|DLP]] showed no alerts.
> **Task:** Determine if data exfiltration was occurring through non-traditional channels.
> **Action:** Analyzed outbound network traffic and noticed employee uploading unusually large vacation photos. Used `binwalk` and `steghide` to examine images—discovered embedded compressed archives containing source code. Correlated upload times with file access logs.
> **Result:** Identified insider threat, recovered exfiltrated IP, and implemented image re-encoding at upload to prevent future stego-based exfiltration.

## ✅ Best Practices
- Always re-encode uploaded images server-side (destroys stego)
- Strip all metadata using tools like `exiftool` or `ImageMagick`
- Monitor file size patterns for anomalies
- Educate users that "innocent" images can carry hidden threats
- Include steganalysis in [[Detection Engineering|detection engineering]] playbooks

## ❌ Common Misconceptions
- **"Social media strips all metadata, so stego is impossible"** → Metadata ≠ LSB stego; many platforms preserve pixel data
- **"You need special software to hide data"** → Can be done with basic tools or custom scripts
- **"Stego is only used in CTFs"** → Real-world APTs have used stego for C2 and exfiltration
- **"Encryption is better than stego"** → They serve different purposes and can be combined

## 🔗 Related Concepts
- [[EXIF Metadata]]
- [[Data Exfiltration]]
- [[Data Loss Prevention (DLP)]]
- [[Command and Control (C2)]]
- [[Cryptography & Identity MOC]]
- [[014 🦠 Malware Analysis & Forensics MOC]]
- [[012 ⚔️ Red Team & Offensive Security MOC]]

## 📚 References
- https://en.wikipedia.org/wiki/Steganography
- https://0xrick.github.io/lists/stego/ (Stego tools list)
- SANS: Detecting Steganography
- Steghide documentation: http://steghide.sourceforge.net/
