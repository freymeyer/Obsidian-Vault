---
tags:
  - "#cybersecurity/red-team/password-attacks"
---

- The **strength of protection** depends almost entirely on the password. Short or common passwords can be guessed; long, random passwords are far harder to break.
- Different file formats use different algorithms and key derivation methods. For example, PDF encryption and ZIP encryption differ in details (how the key is derived, salt use, number of hash iterations). That affects how easy or hard cracking is.
- Many consumer tools still support legacy or weak modes (particularly older ZIP encryption). That makes some encrypted archives much easier to attack than modern, well-implemented schemes.
- Encryption protects data confidentiality only. It does not prevent someone with access to the encrypted file from trying to guess the password offline.
- Encryption makes the contents unreadable unless the correct password is known. If the password is weak, an attacker can simply try likely passwords until one works.
## Methods
- [[Dictionary Attacks]]
- [[Mask Attacks]]

## Tools
- [[pdfcrack]]
- [[fcrackzip]]
- [[John the Ripper]]
- [[hashcat]]

## Password Cracking Detection (Blue-teaming)
Offline cracking does not hit login services, so lockouts and failed logon dashboards stay quiet. We can detect the work where it runs, on endpoints and jump boxes. The important signals to monitor include:
### Process creation
Password cracking has a small set of well-known binaries and command patterns that we can look out for. A mix of process events, file activity, GPU signals, and network touches tied to tooling and wordlists. Our goal is to make the activity obvious without drowning in noise.

- Binaries and aliases: `john`, `hashcat`, `fcrackzip`, `pdfcrack`, `zip2john`, `pdf2john.pl`, `7z`, `qpdf`, `unzip`, `7za`, `perl` invoking `pdf2john.pl`.
- Command‑line traits: `--wordlist`, `-w`, `--rules`, `--mask`, `-a 3`, `-m` in Hashcat, references to `rockyou.txt`, `SecLists`, `zip2john`, `pdf2john`.
- Potfiles and state: `~/.john/john.pot`, `.hashcat/hashcat.potfile`, `john.rec`.

It's worth noting that on Windows systems, Sysmon Event ID 1 captures process creation with full command line properties, while on Linux, `auditd`, `execve`, or EDR sensors capture binaries and arguments.

### GPU and Resource Artefacts
GPU cracking is loud. Sudden high utilisation on hosts can be picked up and would need to be investigated.

- `nvidia-smi` shows long‑running processes named `hashcat` or `john`.
- High, steady GPU utilisation and power draw while the fan curve spikes.
- Libraries loaded: `nvcuda.dll`, `OpenCL.dll`, `libcuda.so`, `amdocl64.dll`.

### Network Hints
Offline cracking does not need the network once wordlists are present. Yet most operators fetch lists and tools first.

- Downloads of large text files named `rockyou.txt`, or Git clones of popular wordlist repos.
- Package installs, for example `apt install john hashcat`, detected by EDR package telemetry.
- Tool updates and driver fetches for GPU runtimes.

### Unusual File Reads
Repeated reads of files such as wordlists or encrypted files would need analysis

### Detections
[[Sysmon]]
```EventID=1
(ProcessName="C:\Program Files\john\john.exe" OR
 ProcessName="C:\Tools\hashcat\hashcat.exe" OR
 CommandLine="*pdf2john.pl*" OR
 CommandLine="*zip2john*")
```

[[Linux]] audit rules, temporary for an investigation
```bash
auditctl -w /usr/share/wordlists/rockyou.txt -p r -k wordlists_read
auditctl -a always,exit -F arch=b64 -S execve -F exe=/usr/bin/john -k crack_exec
auditctl -a always,exit -F arch=b64 -S execve -F exe=/usr/bin/hashcat -k crack_exec
```

[[Sigma]] style rule, Windows process create for cracking tools
```yaml
title: Password Cracking Tools Execution
id: 9f2f4d3e-4c16-4b0a-bb3a-7b1c6c001234
status: experimental
logsource:
  product: windows
  category: process_creation
detection:
  selection_name:
    Image|endswith:
      - '\john.exe'
      - '\hashcat.exe'
      - '\fcrackzip.exe'
      - '\pdfcrack.exe'
      - '\7z.exe'
      - '\qpdf.exe'
  selection_cmd:
    CommandLine|contains:
      - '--wordlist'
      - 'rockyou.txt'
      - 'zip2john'
      - 'pdf2john'
      - '--mask'
      - ' -a 3'
  condition: selection_name or selection_cmd
level: medium
```

## Response Playbook
1. Isolate the host if malicious activity is detected. If it is a lab, tag and suppress.
2. Capture triage artefacts such as process list, process memory dump, `nvidia-smi` sample output, open files, and the encrypted file.
3. Preserve the working directory, wordlists, hash files, and shell history.
4. Review which files were decrypted. Search for follow‑on access, lateral movement or exfiltration.
5. Identify the origin and intent of the activity. Was this authorised? If not, escalate to the IR team.
6. Remediate the activity, rotate affected keys and passwords, and enforce MFA for accounts.
7. Close with education and correct placement of tools into approved sandboxes.