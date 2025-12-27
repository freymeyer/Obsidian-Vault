---
tags:
  - "#ctf/thm/writeup"
  - "#learning/lab"
status: In Progress
difficulty: Easy
link: https://tryhackme.com/room/ohsint
---

# OhSINT

> **Room Link:** https://tryhackme.com/room/ohsint

## 🧠 Knowledge Inbox (Unfamiliar Things)
*Quickly list things you don't know here. Define them LATER.*
- [ ] [[ExifTool]] - extract metadata from the image
- [ ] [[EXIF Metadata]] - what metadata fields can leak
- [ ] [[GitHub OSINT]] - pivoting from repos/commits to identity
- [ ] [[WiGLE]] - mapping a [[SSID vs BSSID|BSSID]] to a location
- [ ] [[View Page Source]] - finding hints/secrets in HTML
- [ ] [[WordPress]] - common CMS footprints during recon

## 📝 Lab Journal
*Chronological log of actions, commands, and findings.*

### Task 1: OhSint
What information can you possibly get with just one image file?

Note: This challenge was updated on 2024-02-01. If you are following any older walkthroughs, expect a small change. Additionally, the file is also available on the AttackBox, under the /Rooms/OhSINT directory.

## 🚩 Flags
| Task | Question                                     | Flag                   | How I found it                                                                                                                                                     |
| ---- | -------------------------------------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1    | What is this user's avatar of?               | `cat`                  | Found the twitter username `@OWoodflint` by finding the copyright inside exiftool of the image from the challenge                                                  |
| 1    | What city is this person in?                 | `London`               | Found the city in his github repo in https://github.com/OWoodfl1nt/people_finder                                                                                   |
| 1    | What is the SSID of the WAP he connected to? | `UnileverWiFi`         | Found the ssid inside wigle using his twitter post 'From my house I can get free wifi ;D Bssid: B4:5D:50:AA:86:41 - Go nuts!' specifically the BSSID               |
| 1    | What is his personal email address?          | `OWoodflint@gmail.com` | Found the email in his github repo in https://github.com/OWoodfl1nt/people_finder                                                                                  |
| 1    | What site did you find his email address on? | `Github`               | Found the account in google by searching OWoodflint                                                                                                                |
| 1    | Where has he gone on holiday?                | `New York`             | Found where he had gone holiday off to in his wordpress from his repo https://github.com/OWoodfl1nt/people_finder, and in home page there's a text said `New york` |
| 1    | What is the person's password?               | `pennYDr0pper.!`       | Found the person's password in the wordpress source page in the home page                                                                                          |


## 🎓 Key Takeaways
- [[ExifTool]] can reveal pivot points (e.g., author/copyright fields) from a single image via [[EXIF Metadata]].
- [[GitHub OSINT]] is a fast pivot for location clues and contact info when a username is discovered.
- A Wi‑Fi [[SSID vs BSSID|BSSID]] can be mapped to an approximate location using [[WiGLE]].
- [[View Page Source]] is worth checking for hidden hints, credentials, and breadcrumbs (common in [[WordPress]] sites).
