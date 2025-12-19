---
tags:
  - "#cybersecurity/web-sec/client-side"
---

A type of security vulnerability typically found in web applications. It allows attackers to inject malicious scripts into web pages viewed by other users. These scripts can then steal sensitive information, like user's cookies, session tokens, or other sensitive data.

XSS is a web application vulnerability that lets attackers (or evil bunnies) inject malicious code (usually JavaScript) into input fields that reflect content viewed by other users (e.g., a form or a comment in a blog). When an application doesn't properly validate or escape user input, that input can be interpreted as code rather than harmless text. This results in malicious code that can steal credentials, deface pages, or impersonate users. 

# Types of XSS
## Reflected XSS
Reflected variants when the injection is immediately projected in a response. Imagine a toy search function in an online toy store, you search via:

`https://trygiftme.thm/search?term=gift`

But imagine you send this to your friend who is looking for a gift for their nephew (please don't do this):

`https://trygiftme.thm/search?term=<script>alert( atob("VEhNe0V2aWxfQnVubnl9") )</script>`

If your friend clicks on the link, it will execute code instead.

### Exploit
To exploit reflected XSS, we can use test payloads to check if the app runs the code injected. If you want to test more advanced payloads, there are [cheat sheets](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) online that you can use to craft them. For now, we'll pick the following payload:

`<script>alert('Reflected Meow Meow')</script>`

Inject the code by adding the payload to the search bar and clicking " **Search Messages**". If it shows the alert text, we have confirmed reflected XSS. So, what happened?

- The search input is reflected directly in the results without encoding
- The browser interprets your HTML/JavaScript as executable code
- An alert box appeared, demonstrating successful XSS execution

## Stored XSS
A Stored XSS attack occurs when malicious script is saved on the server and then loaded for every user who views the affected page. Unlike Reflected XSS, which targets individual victims, 

Stored XSS becomes a "set-and-forget" attack, anyone who loads the page runs the attacker’s script.

### Normal Comment Submission
```http
POST /post/comment HTTP/1.1
Host: tgm.review-your-gifts.thm

postId=3
name=Tony Baritone
email=tony@normal-person-i-swear.net
comment=This gift set my carpet on fire but my kid loved it!
```
The server stores this information and displays it whenever someone visits that blog post.

### Malicious Comment Submission (Stored XSS Example)
If the application does not sanitize or filter input, an attacker can submit JavaScript instead of a comment:
```http
POST /post/comment HTTP/1.1
Host: tgm.review-your-gifts.thm

postId=3
name=Tony Baritone
email=tony@normal-person-i-swear.net
comment=<script>alert(atob("VEhNe0V2aWxfU3RvcmVkX0VnZ30="))</script> + "This gift set my carpet on fire but my kid loved it!"
```

Because the comment is saved in the database, every user who opens that blog post will automatically trigger the script.

This lets the attacker run code as if they were the victim in order to perform malicious actions such as: 

- Steal session cookies
- Trigger fake login popups
- Deface the page



# Protecting against XSS

Each service is different, and requires a well-thought-out, secure design and implementation plan, but key practices you can implement are:

- **Disable dangerous rendering raths:** Instead of using the `innerHTML` property, which lets you inject any content directly into HTML, use the `textContent` property instead, it treats input as text and parses it for HTML.
- **Make cookies inaccessible to JS:** Set session cookies with the [HttpOnly](https://owasp.org/www-community/HttpOnly), [Secure](https://owasp.org/www-community/controls/SecureCookieAttribute), and [SameSite](https://owasp.org/www-community/SameSite) attributes to reduce the impact of XSS attacks.
- **Sanitise input/output and encode:**

- In some situations, applications may need to accept limited HTML input—for example, to allow users to include safe links or basic formatting. However it's critical to sanitize and encode all user-supplied data to prevent security vulnerabilities. Sanitising and encoding removes or escapes any elements that could be interpreted as executable code, such as scripts, event handlers, or JavaScript URLs while preserving safe formatting.

To exploit XSS vulnerabilities, we need some type of input field to inject code. There is a search section, let's start there.
