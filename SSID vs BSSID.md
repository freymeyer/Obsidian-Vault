---
tags:
  - "#infrastructure/networking/wireless"
  - "#cybersecurity/osint"
aliases:
  - SSID
  - BSSID
  - Wireless Access Point
  - WAP
---

# SSID vs BSSID

> **One-liner:** SSID is the human-readable Wi‑Fi network name; BSSID is the access point’s MAC address that uniquely identifies the radio.

## 🎯 What Is It?
In 802.11 (Wi‑Fi) networks:
- **SSID** (Service Set Identifier) is the broadcast name you see when joining Wi‑Fi.
- **BSSID** (Basic Service Set Identifier) is typically the MAC address of a specific access point (AP) radio.

A **WAP/AP** (Wireless Access Point / Access Point) is the device providing Wi‑Fi connectivity; one SSID can be served by multiple APs (each with its own BSSID).

## 🤔 Why It Matters
- **OSINT / geolocation:** BSSID can be searched in datasets like [[WiGLE]] to approximate physical location.
- **Threat hunting / IR:** Wireless identifiers show up in endpoint logs, roaming history, and wireless surveys.
- **Security:** Knowing the difference helps spot evil twin attacks and misconfigurations.

## 🔬 How It Works
### Core Principles
1. **SSID** identifies the network name (not unique globally).
2. **BSSID** identifies the specific AP radio (more unique).
3. Enterprise networks often use multiple BSSIDs per SSID (multiple APs/2.4/5GHz bands).

### Technical Deep-Dive
Quick comparison:
- SSID: “CoffeeShopWiFi”
- BSSID: `B4:5D:50:AA:86:41`

## 🛡️ Detection & Prevention
### How to Detect
- Compare expected SSID + expected BSSID(s) during investigations.
- Watch for endpoints connecting to the right SSID but a new/unexpected BSSID (possible evil twin/rogue AP).

### How to Prevent / Mitigate
- Use WPA2-Enterprise/WPA3-Enterprise where possible.
- Train staff not to trust SSID name alone.
- Maintain an inventory of approved APs/BSSIDs for critical environments.

## 📊 Types/Categories
| Type | Description | Example |
|------|-------------|---------|
| **SSID** | Network name | GuestWiFi |
| **BSSID** | AP radio identifier (MAC) | `00:11:22:33:44:55` |

## 🎤 Interview Angles
### Common Questions
- "What’s the difference between SSID and BSSID?"
- "How can attackers abuse SSIDs?"

### STAR Story
> **Situation:** Users reported suspicious Wi‑Fi behavior.
> **Task:** Determine whether a rogue AP was present.
> **Action:** Compared reported SSID/BSSID to the approved AP inventory.
> **Result:** Confirmed a mismatch and removed the rogue device.

## ✅ Best Practices
- Document corporate SSIDs and expected BSSID ranges.
- Prefer certificate-based wireless auth for sensitive environments.

## ❌ Common Misconceptions
- "SSID uniquely identifies an access point" (multiple APs can share an SSID).

## 🔗 Related Concepts
- [[WiGLE]]
- [[Open Source Intelligence (OSINT)]]

## 📚 References
- IEEE 802.11 documentation (high level)
