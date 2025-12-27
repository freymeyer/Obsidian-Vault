---
tags:
  - "#cybersecurity/osint"
  - "#infrastructure/networking/wireless"
aliases:
  - Wireless Geographic Logging Engine
  - wigle
---

# WiGLE

> **One-liner:** A public database that maps Wi‑Fi networks and cell towers to approximate geographic locations.

## 🎯 What Is It?
WiGLE (Wireless Geographic Logging Engine) aggregates wardriving-style observations of wireless identifiers (e.g., Wi‑Fi BSSIDs) and associates them with latitude/longitude. OSINT practitioners can use it to infer where a given BSSID has been observed.

## 🤔 Why It Matters
- **OSINT geolocation:** A single BSSID can become a pivot to a neighborhood/city.
- **Threat intel / investigations:** Helps attribute where wireless infrastructure has appeared.
- **Privacy:** Broadcasting identifiers can unintentionally leak physical location context.

## 🔬 How It Works
### Core Principles
1. Devices observe a Wi‑Fi network’s **BSSID** (MAC address) and **SSID** (name).
2. Observations are uploaded with coordinates.
3. Queries can return approximate locations where that BSSID was seen.

### Technical Deep-Dive
Common workflow:
- Start with a BSSID (often found in posts, configs, or logs)
- Search it on WiGLE
- Use returned coordinates + map view to narrow location

## 🛡️ Detection & Prevention
### How to Detect
- Review public exposure of wireless identifiers (posts, screenshots, configs).
- Monitor for doxxing-style OSINT using wireless pivots.

### How to Prevent / Mitigate
- Avoid publishing BSSIDs publicly.
- Use safe sharing guidelines for network troubleshooting screenshots/logs.

## 📊 Types/Categories
| Type | Description | Example |
|------|-------------|---------|
| **Wi‑Fi** | Access point observations | BSSID → lat/long |
| **Cell** | Tower observations | Cell ID → region |

## 🎤 Interview Angles
### Common Questions
- "How can a Wi‑Fi BSSID be used for geolocation?"
- "What are the limitations of WiGLE-based geolocation?"

### STAR Story
> **Situation:** A public post included a Wi‑Fi identifier.
> **Task:** Estimate whether it revealed a real-world location.
> **Action:** Queried the identifier in WiGLE and cross-checked map results.
> **Result:** Confirmed the location leak and recommended safer posting practices.

## ✅ Best Practices
- Treat results as **approximate** and time-sensitive.
- Validate with other OSINT sources before concluding.

## ❌ Common Misconceptions
- "WiGLE gives exact live location" (it’s based on past sightings).

## 🔗 Related Concepts
- [[SSID vs BSSID]]
- [[Open Source Intelligence (OSINT)]]

## 📚 References
- https://wigle.net/
