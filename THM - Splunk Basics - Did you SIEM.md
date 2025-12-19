---
tags:
  - ctf/thm/writeup
  - learning/study-note
---

client_ip = 198.51.100.55

user_agent = wget, zgrab, curl, Go-http-client, Havij, sqlmap, python-requests, Ruby

## **Reconnaissance (The Setup):**

```sourcetype=web_traffic client_ip="198.51.100.55" AND path IN ("/.env", "/*phpinfo*", "/.git*") | table _time, path, user_agent, status```

The result confirms the attacker used low-level tools (`curl`, `wget`) and was met with **404/403/401** status codes.

## **Enumeration (Vulnerability Testing)**
```
sourcetype=web_traffic client_ip="198.51.100.55" AND path="*..*" OR path="*redirect*"

sourcetype=web_traffic client_ip="198.51.100.55" AND path="*..\/..\/*" OR path="*redirect*"  200 | stats  count by path
```

Quite interesting results. Reveals attempts to read system files (`../../*`), showing the attacker moved beyond simple scanning to active vulnerability testing.

## [[SQL Injection]] Attack

```
sourcetype=web_traffic client_ip="198.51.100.55" AND user_agent IN ("*sqlmap*", "*Havij*") | table _time, path, status
```
Above results confirms the use of known SQL injection and specific attack strings like `SLEEP(5)`. A **504** status code often confirms a successful time-based SQL injection attack.

## Exfiltration Attempts
```
sourcetype=web_traffic client_ip="198.51.100.55" AND path IN ("*backup.zip*", "*logs.tar.gz*") | table _time path, user_agent
```
sourcetype=web_traffic client_ip="198.51.100.55" AND path IN ("*backup.zip*", "*logs.tar.gz*") | table _time path, user_agent

## ## [[Ransomware]] Staging & [[Remote Code Execution (RCE)]]

```
sourcetype=web_traffic client_ip="198.51.100.55" AND path IN ("*bunnylock.bin*", "*shell.php?cmd=*") | table _time, path, user_agent, status
```

```
Above results clearly confirm a successful webshell. The attacker has gained full control over the web server and is also able to run commands. This type of attack is called Remote code Execution (RCE). The execution of `/shell.php?cmd=./bunnylock.bin` indicates a ransomware like program executed on the server.
```

## ## Correlate Outbound [[Command and Control (C2)]] Communication

```
sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="198.51.100.55" AND action="ALLOWED" | table _time, action, protocol, src_ip, dest_ip, dest_port, reason
```
This query proves the server immediately established an **outbound** connection to the attacker's C2 IP on the suspicious `DEST_PORT`. The `ACTION=ALLOWED` and `REASON=C2_CONTACT` fields confirm the malware communication channel was active.

## ## Volume of [[Data Exfiltration]]
```
sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="198.51.100.55" AND action="ALLOWED" | stats sum(bytes_transferred) by src_ip
```
The results show a huge volume of data transferred from the compromised webserver to C2 server.

## Conclusion

- **Identity found:** The attacker was identified via the highest volume of malicious web traffic originating from the external IP.
- **Intrusion vector:** The attack followed a clear progression in the web logs (`sourcetype=web_traffic`).
- **Reconnaissance:** Probes were initiated via cURL/Wget, looking for configuration files (`/.env`) and testing path traversal vulnerabilities.
- **Exploitation:** The use of `SQLmap` user agents and specific payloads (`SLEEP(5)`) confirmed the successful exploitation phase.
- **Payload delivery:** The Action on Objective was established by the final successful execution of the command `cmd=./bunnylock.bin` via the webshell.
- **C2 confirmation:** The pivot to the firewall logs (`sourcetype=firewall_logs`) proved the post-exploitation activity. The internal, compromised server (`SRC_IP: 10.10.1.5`) established an outbound C2 connection to the attacker's IP.

