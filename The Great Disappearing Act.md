---
tags:
  - "#ctf/thm/writeup"
  - "#learning/lab"
status: In Progress
difficulty: 
link: 
---

# The Great Disappearing Act

> **Room Link:** 

## 🧠 Knowledge Inbox (Unfamiliar Things)
*Quickly list things you don't know here. Define them LATER.*
- [ ] [[Concept/Tool]] - brief note
- [ ] 

## 📝 Lab Journal
*Chronological log of actions, commands, and findings.*

### Password
#### read-me-please.txt 
From: mcskidy
To: whoever finds this

I had a short second when no one was watching. I used it.

I've managed to plant a few clues around the account.
If you can get into the user below and look carefully,
those three little "easter eggs" will combine into a passcode
that unlocks a further message that I encrypted in the
/home/eddi_knapp/Documents/ directory.
I didn't want the wrong eyes to see it.

Access the user account:
username: eddi_knapp
password: S0mething1Sc0ming

There are three hidden easter eggs.
They combine to form the passcode to open my encrypted vault.

Clues (one for each egg):

1)
I ride with your session, not with your chest of files.
Open the little bag your shell carries when you arrive.
**3ast3r**-1s-c0M1nG
2)
The tree shows today; the rings remember yesterday.
Read the ledger’s older pages.

3)
When pixels sleep, their tails sometimes whisper plain words.
Listen to the tail.

Find the fragments, join them in order, and use the resulting passcode
to decrypt the message I left. Be careful — I had to be quick,
and I left only enough to get help.



~ McSkidy

#### wishlist.txt
Chocolate egg assortments (assorted sizes and flavours)
Hand-painted decorative Easter eggs
Easter basket with plush toys and treats
Seasonal flower bouquet (tulips & daffodils)
Bunny-themed cookie gift boxes
Spring picnic hamper with pastel treats
DIY egg-decorating kit with paints and stencils
Ceramic egg display stand for mantels
Easter egg hunt supply pack (egg fillers & clues)
Pastel ribbon and wrapping set for gifts

#### **mcskidy_note.txt.gpg**
gpg: AES256.CFB encrypted data
gpg: encrypted with 1 passphrase
Congrats — you found all fragments and reached this file.

Below is the list that should be live on the site. If you replace the contents of
/home/socmas/2025/wishlist.txt with this exact list (one item per line, no numbering),
the site will recognise it and the takeover glitching will stop. Do it — it will save the site.

Hardware security keys (YubiKey or similar)
Commercial password manager subscriptions (team seats)
Endpoint detection & response (EDR) licenses
Secure remote access appliances (jump boxes)
Cloud workload scanning credits (container/image scanning)
Threat intelligence feed subscription

Secure code review / SAST tool access
Dedicated secure test lab VM pool
Incident response runbook templates and playbooks
Electronic safe drive with encrypted backups

A final note — I don't know exactly where they have me, but there are *lots* of eggs
and I can smell chocolate in the air. Something big is coming.  — McSkidy

---

When the wishlist is corrected, the site will show a block of ciphertext. This ciphertext can be decrypted with the following unlock key:

UNLOCK_KEY: 91J6X7R4FQ9TQPM9JX2Q9X2Z

To decode the ciphertext, use OpenSSL. For instance, if you copied the ciphertext into a file /tmp/website_output.txt you could decode using the following command:

cat > /tmp/website_output.txt
openssl enc -d -aes-256-cbc -pbkdf2 -iter 200000 -salt -base64 -in /tmp/website_output.txt -out /tmp/decoded_message.txt -pass pass:'91J6X7R4FQ9TQPM9JX2Q9X2Z'
cat /tmp/decoded_message.txt


Well done — the glitch is fixed. Amazing job going the extra mile and saving the site. Take this flag THM{w3lcome_2_A0c_2025}

NEXT STEP:
If you fancy something a little...spicier....use the FLAG you just obtained as the passphrase to unlock:
/home/eddi_knapp/.secret/dir

That hidden directory has been archived and encrypted with the FLAG.
Inside it you'll find the sidequest key.


9117b88e-6528-4c21-ab21-e0f954c1b8c4

{"effective_tier":"guard","ticket_id":"b68a249d-7d62-472b-a0ee-1fc965da4364"}

#### File Recon
- [ ] Desktop
- [x] Documents - notes_on_photos.txt
- [ ] Downloads - IMG_list.txt cat = wallpaper_spring.png, family_holiday.jpg, profile_pic.png, scenery_01.png


### Password = now_you_see_me


THM{There.is.no.EASTmas.without.Hopper}
### Task 1: Introduction
- 

### Task 2: Reconnaissance
- Ran nmap: `nmap -sC -sV <IP>`
- Findings:
    - Port 80: HTTP
    - Port 22: SSH

### Task 3: Exploitation
- 

## 🚩 Flags
| Task | Question | Flag |
|------|----------|------|
| 1 | User Flag | `thm{...}` |
| 2 | Root Flag | `thm{...}` |

## 🎓 Key Takeaways
- Learned how to use [[Tool Name]] for...
- Understood the concept of [[Concept Name]]...
