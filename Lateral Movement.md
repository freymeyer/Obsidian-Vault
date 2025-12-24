---
tags:
  - "#cybersecurity/attacks/post-exploitation"
  - "#cybersecurity/blue-team/detection"
  - "#interview/concepts"
aliases:
  - Pivoting
  - Network Propagation
---

# Lateral Movement

> **One-liner:** Techniques attackers use to move through a network after initial compromise, accessing additional systems and escalating privileges.

## 🎯 What Is It?
Lateral Movement occurs during the **Actions on Objectives** stage of the [[Cyber Kill Chain]], where attackers expand their foothold by moving from the initially compromised system to other systems within the network. The goal is to find valuable data, gain higher privileges, or reach specific targets.

## 🔬 How It Works

```
Initial Compromise → Lateral Movement → Target Systems
        │                   │                │
   Workstation A     ──────►│         Domain Controller
        │            Use stolen│              ▲
        │            creds or  │              │
        │            exploits ─┼─────────────►│
        │                     │        File Server
        │                     │              ▲
        │                     └─────────────►│
        │                           Database Server
        └─────────────────────────────────────►
```

## 📊 Common Techniques

### Credential-Based Movement

| Technique | Description | Tools |
|-----------|-------------|-------|
| **Pass-the-Hash (PtH)** | Use NTLM hash without knowing password | Mimikatz, pth-toolkit |
| **Pass-the-Ticket (PtT)** | Use Kerberos tickets | Mimikatz, Rubeus |
| **Overpass-the-Hash** | Convert NTLM to Kerberos ticket | Mimikatz |
| **Golden Ticket** | Forged Kerberos TGT | Mimikatz |
| **Silver Ticket** | Forged service ticket | Mimikatz |

### Protocol-Based Movement

| Protocol | Technique | Detection Focus |
|----------|-----------|-----------------|
| **SMB** | PsExec, SMBExec | Service creation, admin shares |
| **WinRM** | PowerShell Remoting | WinRM connections, event logs |
| **RDP** | Remote Desktop | Login events, network traffic |
| **SSH** | Key-based access | Auth logs, key usage |
| **WMI** | Remote execution | WMI event logs |
| **DCOM** | Distributed COM | Process creation |

### Common Commands
```powershell
# PsExec
psexec \\target -u domain\user cmd.exe

# WinRM
Enter-PSSession -ComputerName target

# WMI
wmic /node:target process call create "cmd.exe"

# RDP
mstsc /v:target
```

## 🛡️ Detection & Prevention

### How to Detect
- **Authentication logs** - 4624 (logon), 4625 (failed logon) events
- **Service creation** - Event 7045 (new service installed)
- **Process creation** - Sysmon Event 1 with network context
- **Network connections** - SMB, WinRM, RDP from unusual sources
- **Admin share access** - Access to C$, ADMIN$
- **Lateral movement patterns** - Multiple systems accessed quickly

### Key Detection Queries
```
# Windows Event Log - Remote Logon
EventID=4624 AND LogonType IN (3, 10)

# Sysmon - PsExec-like behavior
EventID=1 AND Image CONTAINS "PSEXESVC"

# Network - SMB lateral movement
tcp.port == 445 AND smb.path CONTAINS "$"
```

### How to Prevent / Mitigate
- **Credential hygiene** - Don't reuse local admin passwords (LAPS)
- **Network segmentation** - Limit communication between workstations
- **Privileged Access Workstations (PAWs)** - Isolate admin access
- **[[Multi-Factor Authentication (MFA)|MFA]]** - For remote access and privileged actions
- **Disable unnecessary protocols** - SMBv1, WMI, DCOM if not needed
- **[[Endpoint detection and response (EDR)|EDR]]** - Detect credential theft and movement

## 🎤 Interview Angles

### Common Questions
- "What is lateral movement and how would you detect it?"
- "Explain Pass-the-Hash and how to prevent it"
- "What logs would you check for lateral movement?"

### STAR Story
> **Situation:** SOC detected unusual SMB traffic between workstations after hours.
> **Task:** Investigate potential lateral movement and contain the threat.
> **Action:** Analyzed Windows Security logs and found a single account accessing multiple systems via admin shares. Correlated with Sysmon logs showing PsExec service installation. Isolated affected systems and reset compromised credentials.
> **Result:** Contained attacker before they reached domain controller. Implemented network segmentation and LAPS to prevent future lateral movement via shared credentials.

## ✅ Best Practices
- Implement least privilege for all accounts
- Use unique local admin passwords (LAPS)
- Segment networks to limit blast radius
- Monitor authentication logs actively
- Deploy [[Honeypot|honeypots]] to detect internal movement
- Regular credential rotation

## 🔗 Related Concepts
- [[Cyber Kill Chain]]
- [[Persistence (Cyber Security)]]
- [[Privilege Escalation]]
- [[Credential Dumping]]
- [[Pass-the-Hash]]

## 📚 References
- MITRE ATT&CK - Lateral Movement Techniques
- Microsoft Securing Privileged Access
