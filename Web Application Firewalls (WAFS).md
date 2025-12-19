**Web Application Firewalls (WAFs)** are often the first line of defense for websites and web applications. 

WAFs act as gatekeepers for your web applications, inspecting full request packets, similar to Wireshark but with the ability to decrypt TLS traffic and filter it before it reaches the server.

## Rules
WAFs inspect and decide whether to allow a web request or block it entirely based on predefined rules. Let's examine a few categories of firewall rules.

|Rule Type|Description|Example Use Case|
|---|---|---|
|Block common attack patterns|Blocks known malicious payloads and indicators|Block malicious User-Agents: `sqlmap`|
|Deny known malicious sources|Uses IP reputation, threat intel, or geo-blocking to stop risky traffic|Block IPs from recent botnet campaigns|
|Custom-built rules|Tailored to your specific application’s needs|Allow only GET/POST requests to `/login`|
|Rate-limiting & abuse prevention|Limits request frequency to prevent abuse|Limit login attempts to 5 per minute per IP|
Imagine you notice repeated `GET` requests to `/changeusername` with the User-Agent string `sqlmap/1.9`. SQLMap is an automated tool for detecting and exploiting SQL injection vulnerabilities. After reviewing the network traffic, you see that the requests include SQLi payloads.

You might create a rule to block any User-Agent string matching `sqlmap`:

`If User-Agent contains "sqlmap"`  
`then BLOCK`

This is a rudimentary example and modern WAFs will detect and block known suspicious User-Agents automatically. However, rules like this can be custom-made to suit your specific application or threat scenario, allowing you to block malicious activity without impacting normal site traffic.

## Challenge-Response Mechanisms
WAFs don't always need to block suspicious requests outright. For example, they can challenge requests with CAPTCHA to verify whether they come from a real user rather than a bot. This capability is extremely valuable considering that malicious bot traffic [makes up 37%](https://www.thalesgroup.com/en/worldwide/defence-and-security/press_release/artificial-intelligence-fuels-rise-hard-detect-bots) of global web traffic. This approach is beneficial for firewall rules with a higher chance of blocking legitimate web traffic.

## Integrating Known Indicators and Threat Intelligence
Many modern WAF solutions include built-in rule sets designed to mitigate the [OWASP Top 10](https://owasp.org/www-project-top-ten/) security risks, some of which we have covered previously. WAFs also leverage threat intelligence feeds to automatically block requests from known malicious IP addresses and suspicious User-Agents. They receive regular updates to combat new and emerging threats, including those from known APT groups and recently discovered CVEs. [Check out](https://blog.cloudflare.com/new-waf-intelligence-feeds) how Cloudflare maintains curated IP lists, from sources like botnets, VPNs, anonymizers, and malware, based on global threat intelligence.