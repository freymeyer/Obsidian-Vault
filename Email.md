[[Ray Tomlinson]] contributed for inventing the concept of emails and the @ symbol.
The invention of the email dates back to the 1970s for [ARPANET](https://www.britannica.com/topic/ARPANET)
## Email Address

1. **User Mailbox (or Username)**
2. **@**
3. **Domain**
## Email Delivery
There are 3 specific protocols involved to facilitate the outgoing and incoming email messages, and they are briefly listed below.
- [[Simple Mail Transfer Protocol (SMTP)]] - It is utilized to handle the sending of emails. 
- [[Post Office Protocol (POP3)]] - Is responsible transferring email between a client and a mail server. 
- [[Internet Message Access Protocol (IMAP)]] - Is responsible transferring email between a client and a mail server.

| Server/Port       | POP / IMAP                                                                                         |                            |
| ----------------- | -------------------------------------------------------------------------------------------------- | -------------------------- |
| Outgoing hostname | smtp.dreamhost.com                                                                                 |                            |
| Port 587          | Insecure Transport (Upgraded to a secure connection using STARTTLS)                                |                            |
| Port 25           | Outdated and not recommended. username/password authentication MUST be enabled if using this port. |                            |
| Server/Port       | POP                                                                                                | IMAP                       |
| Incoming hostname | pop.dreamhost.com                                                                                  | imap.dreamhost.com         |
| Secure Port       | Port 995 (SSL enabled)                                                                             | Port 993 (SSL enabled)     |
| Insecure Port     | Port 110 (SSL not enabled)                                                                         | Port 143 (SSL not enabled) |

![[Pasted image 20251209203614.png]]
Explanation:
1. Alexa composes an email to Billy (`billy@johndoe.com`) in her favorite email client. After she's done, she hits the send button.
2. The **SMTP** server needs to determine where to send Alexa's email. It queries **DNS** for information associated with `johndoe.com`. 
3. The **DNS** server obtains the information `johndoe.com` and sends that information to the **SMTP** server. 
4. The **SMTP** server sends Alexa's email across the Internet to Billy's mailbox at `johndoe.com`.
5. In this stage, Alexa's email passes through various **SMTP** servers and is finally relayed to the destination **SMTP** server. 
6. Alexa's email finally reached the destination **SMTP** server.
7. Alexa's email is forwarded and is now sitting in the local **POP3/IMAP** server waiting for Billy. 
8. Billy logs into his email client, which queries the local **POP3/IMAP** server for new emails in his mailbox.
9. Alexa's email is copied (**IMAP**) or downloaded (**POP3**) to Billy's email client.

## Email Headers
- the email **header** (information about the email, such as the email servers that relayed the email)
- the email **body** (text and/or HTML formatted text)

The syntax for email messages is known as the [[Internet Message Format (IMF)]]

1. **From** - the sender's email address
2. **Subject** - the email's subject line
3. **Date** - the date when the email was sent
4. **To** - the recipient's email address

## Email Body

HTML is what makes it possible to add these elements to an email.

To view an email's HTML code is the same approach shown below, but it may vary depending on the webmail client.

The following example is an HTML formatted email from "Netflix" with an attachment. The web client is Yahoo!

1. The email body has an image.
2. The email attachment is a PDF document.

- **Content-Type** is **application/pdf**. 
- **Content-Disposition** specifies it's an **attachment**. 
- **Content-Transfer-Encoding** tells us it's **base64** encoded. 

With the base64 encoded data, you can decode it and save it to your machine.

Warning: When interacting with attachments, proceed with caution and make sure you don't double-click an email's attachment by accident.   

**Note**: Headers specific to 'content' can be found in various locations within an email message source code, and they're not only associated with attachments. For example, **Content-Type** can be **text/html,** and **Content-Transfer-Encoding** can have other values, such as **8bit**.


## [[Business Email Compromise]]

