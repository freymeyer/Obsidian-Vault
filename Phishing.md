---
tags:
  - "#cybersecurity/forensics/threat-intel"
---

Phishing is a subset of [[Social Engineering]] in which the communication medium is mostly messages. 

Modern phishing focuses on precision and persuasion. Messages are carefully crafted, often mimicking real people, portals, or internal processes to trick even the most cautious users.

At one point, the most common phishing attacks happened via 

- email
- spread phishing to short text messages (smishing)
- voice calls (vishing)
- QR codes (quishing)
- and social-media direct messages. 

The attacker’s purpose is to make the target user click, open, or reply to a message so that the attacker can steal information, money, or access.

Common intentions behind phishing messages:

- **Credential theft:** Tricking users into revealing passwords or login details.
- **Malware delivery:** Disguising malicious attachments or links as safe content.
- **Data exfiltration:** Gathering sensitive company or personal information.
- **Financial fraud:** Persuading victims to transfer money or approve fake invoices.

Anti-phishing mnemonics written as S.T.O.P
- Before acting on an email
	- **Suspicious**?
	- **Telling** me to click something?
	- **Offering** me an amazing deal?
	- **Pushing** me to do something now?
- Reminds users to follow the instructions
	- Slow down. Scammers run on your adrenaline.
	- Type the address yourself. Don't use the message's link.
	- Open nothing unexpected. verify first.
	- Prove the sender. Check the real From address/number, not just the display name.
### Types of Phishing
- [[Spam]] - unsolicited junk emails sent out in bulk to a large number of recipients. The more malicious variant of [[Spam]] is known as **MalSpam**. 
- **[Phishing](https://www.proofpoint.com/us/threat-reference/phishing)** -  - [[Email]] sent to a target(s) purporting to be from a trusted entity to lure individuals into providing sensitive information. 
- **[Spear phishing](https://www.proofpoint.com/us/threat-reference/spear-phishing) -** takes phishing a step further by targeting a specific individual(s) or organization seeking sensitive information.  
- **[Whaling](https://www.rapid7.com/fundamentals/whaling-phishing-attacks/)** - is similar to spear phishing, but it's targeted specifically to C-Level high-position individuals (CEO, CFO, etc.), and the objective is the same. 
- [**Smishing**](https://www.proofpoint.com/us/threat-reference/smishing) - takes phishing to mobile devices by targeting mobile users with specially crafted text messages. 
- [**Vishing**](https://www.proofpoint.com/us/threat-reference/vishing) - is similar to smishing, but instead of using text messages for the social engineering attack, the attacks are based on voice calls.
### Characteristics of Phishing
- The **sender email name/address** will masquerade as a trusted entity (**[email spoofing](https://www.proofpoint.com/us/threat-reference/email-spoofing)**)
- The email subject line and/or body (text) is written with a **sense of urgency** or uses certain keywords such as **Invoice**, **Suspended**, etc. 
- The email body (HTML) is designed to match a trusting entity (such as Amazon)
- The email body (HTML) is poorly formatted or written (contrary from the previous point)
- The email body uses generic content, such as Dear Sir/Madam. 
- **Hyperlinks** (oftentimes uses URL shortening services to hide its true origin)
- A [malicious attachment](https://www.proofpoint.com/us/threat-reference/malicious-email-attachments) posing as a legitimate document

## Common Techniques
- [[Impersonation]] - Some attackers may act as a person, department, or even a service to lure users. This is a way to gain credibility by impersonating the recipient manager or an important person within the company.
- [[Social Engineering]] - Social engineering in phishing is the art of manipulating people rather than breaking technology. Attackers craft believable stories, emails, calls, or chat messages that exploit emotions (fear, helpfulness, curiosity, urgency) and real-world context to lure the recipients of a message.
- [[Typosquatting]] & [[Punycode]] - Typosquatting is when an attacker registers a common misspelling of an organisation's domain. For example, `glthub.com` instead of `github.com` (note that there is an `L` instead of an `i`). On the other hand, punycode is a special encoding system that converts Unicode characters (used in writing systems like Chinese, Cyrillic, and Arabic) into ASCII.
- [[Spoofing]] - Email spoofing is another way attackers can trick users into thinking they are receiving emails from a legitimate domain.
- [[Malicious Attachments]] - The most classic way of phishing is attaching malicious files to an email. Usually, these attachments are sent using some sort of social engineering technique in the body.

## Delivery via [[Social Engineer Toolkit]]

## Different Indicators of Phishing
### Cancel your Paypal Order
- **Spoofed email address**
- **URL shortening services**
- **[[Impersonation]] in html a legitimate brand**

