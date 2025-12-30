---
dg-publish: true
tags:
  - "#cybersecurity/vulnerabilities/memory"
  - "#interview/concepts"
  - "#cybersecurity/exploits"
aliases:
  - BOF
  - Memory Corruption
---

# Buffer Overflow

> **One-liner:** A memory corruption vulnerability where data written beyond a buffer's boundary overwrites adjacent memory, potentially allowing arbitrary code execution.

## 🎯 What Is It?
A buffer overflow occurs when a program writes more data to a buffer (fixed-size memory block) than it can hold, causing data to overflow into adjacent memory locations. This can overwrite critical data like return addresses, function pointers, or other variables, leading to crashes, privilege escalation, or remote code execution.

**Core concept:** Writing 10 bytes to an 8-byte buffer = 2 bytes overflow into adjacent memory.

## 🤔 Why It Matters

### Impact
- **Arbitrary Code Execution:** Attacker gains control of program execution flow
- **Privilege Escalation:** Execute code with elevated permissions
- **System Compromise:** Full control of vulnerable system
- **Denial of Service:** Crash applications or systems

### Historical Significance
- **Morris Worm (1988):** First major worm exploited buffer overflow
- **Code Red (2001):** Exploited IIS buffer overflow
- **WannaCry (2017):** Exploited [[CVE-2017-0144|EternalBlue]] SMB buffer overflow
- Modern exploits still leverage buffer overflows despite mitigations

## 🔬 How It Works

### Memory Layout (Stack)
```
High Memory
┌──────────────┐
│ Command Args │
├──────────────┤
│ Environment  │
├──────────────┤
│    Stack     │ ← Function calls, local variables
│      ↓       │
├──────────────┤
│      ↑       │
│     Heap     │ ← Dynamic allocation (malloc)
├──────────────┤
│     BSS      │ ← Uninitialized data
├──────────────┤
│    Data      │ ← Initialized data
├──────────────┤
│    Text      │ ← Program code
Low Memory
```

### Stack Frame Structure
```
┌──────────────────┐ ← Higher addresses
│ Function args    │
├──────────────────┤
│ Return address   │ ← Target for overwrite!
├──────────────────┤
│ Saved base ptr   │
├──────────────────┤
│ Local variables  │
│ (buffers here)   │
└──────────────────┘ ← Lower addresses
```

### Exploitation Flow
```c
// Vulnerable code
void vulnerable_function(char *input) {
    char buffer[64];          // 64-byte buffer
    strcpy(buffer, input);    // No bounds checking!
}

int main(int argc, char **argv) {
    vulnerable_function(argv[1]);
    return 0;
}
```

**Attack:**
```
Input: 64 bytes (A's) + return address overwrite + shellcode
       └──Buffer fill──┘ └──Control flow──┘ └──Payload──┘
```

## 📊 Types of Buffer Overflows

| Type | Location | Exploitation | Complexity |
|------|----------|--------------|------------|
| **Stack-based** | Stack memory | Overwrite return address | Medium |
| **Heap-based** | Heap memory | Overwrite function pointers/vtables | High |
| **Integer Overflow** | Any | Cause buffer size miscalculation | Medium |
| **Off-by-one** | Any | Write 1 byte beyond buffer | High |
| **Format String** | Stack | Use format specifiers to write memory | High |

## 🔓 Exploitation Techniques

### 1. Basic Stack Overflow
```python
# Generate payload
payload = b"A" * 64           # Fill buffer
payload += b"BBBB"            # Overwrite saved base pointer
payload += b"\x10\x20\x30\x40" # Overwrite return address (little-endian)
payload += shellcode          # Execute this code

# Send payload
./vulnerable_program "$(python -c 'print payload')"
```

### 2. Return-to-libc (Bypass NX)
```python
# Instead of shellcode, call existing functions
payload = b"A" * 64
payload += p32(libc_system)   # Address of system()
payload += p32(0x41414141)    # Fake return address
payload += p32(libc_binsh)    # Address of "/bin/sh" string
```

### 3. ROP (Return-Oriented Programming)
```python
# Chain together existing code snippets ("gadgets")
payload = b"A" * 64
payload += p32(gadget1)  # pop eax; ret
payload += p32(value)
payload += p32(gadget2)  # pop ebx; ret
payload += p32(value2)
payload += p32(gadget3)  # int 0x80 (syscall)
```

## 🛡️ Detection & Prevention

### How to Detect (Blue Team)

#### Static Analysis
```bash
# Find vulnerable functions
grep -r "strcpy\|strcat\|sprintf\|gets" source_code/

# Use static analyzers
flawfinder source_code.c
cppcheck --enable=all source_code.c
```

#### Dynamic Analysis
```bash
# Fuzz testing with AFL
afl-fuzz -i input_dir -o output_dir ./target_binary @@

# Memory sanitizers
gcc -fsanitize=address program.c -o program
./program
```

#### Runtime Detection
- **Crashes:** Segmentation faults, access violations
- **IDS/IPS signatures:** Known exploit patterns
- **Anomaly detection:** Unusual memory access patterns
- **EDR alerts:** Buffer overflow attempt indicators

### How to Prevent / Mitigate

#### Secure Coding Practices
```c
// ❌ Vulnerable
strcpy(buffer, user_input);

// ✅ Secure
strncpy(buffer, user_input, sizeof(buffer) - 1);
buffer[sizeof(buffer) - 1] = '\0';

// ✅ Better
snprintf(buffer, sizeof(buffer), "%s", user_input);
```

#### Modern Protections

| Protection | Description | Bypass Difficulty |
|------------|-------------|-------------------|
| **DEP/NX** (Data Execution Prevention) | Marks stack as non-executable | Medium (ROP) |
| **ASLR** (Address Space Layout Randomization) | Randomizes memory addresses | Medium (info leak) |
| **Stack Canaries** | Detection value before return address | Medium (leak/overwrite) |
| **RELRO** (Relocation Read-Only) | Makes GOT read-only | High |
| **CFI** (Control Flow Integrity) | Validates control flow transfers | High |
| **SafeSEH** | Validates exception handlers | Medium |

#### Compiler Flags
```bash
# Enable all protections
gcc -fstack-protector-all \    # Stack canaries
    -D_FORTIFY_SOURCE=2 \      # Runtime checks
    -Wl,-z,relro,-z,now \      # Full RELRO
    -fPIE -pie \               # PIE (ASLR for executable)
    program.c -o program
```

## 🔍 Famous Vulnerabilities

| CVE | Target | Year | Impact |
|-----|--------|------|--------|
| **CVE-2017-0144** | Windows SMB ([[EternalBlue]]) | 2017 | WannaCry, NotPetya ransomware |
| **CVE-2019-18634** | sudo | 2019 | Local privilege escalation |
| **CVE-2014-0160** | OpenSSL (Heartbleed) | 2014 | Memory leak (related concept) |
| **CVE-2001-0500** | IIS (Code Red) | 2001 | Remote code execution |

## 🎤 Interview Angles

### Common Questions
- **"What is a buffer overflow and why is it dangerous?"**
  - *"A buffer overflow happens when a program writes more data to a memory buffer than it can hold, overflowing into adjacent memory. This is dangerous because attackers can overwrite critical data like return addresses to redirect program execution to malicious code."*

- **"How do modern protections like ASLR and DEP/NX mitigate buffer overflows?"**
  - *"ASLR randomizes memory addresses so attackers can't predict where their shellcode or target functions are located. DEP/NX marks the stack as non-executable, preventing direct shellcode execution. Together they force attackers to use advanced techniques like ROP chains and information leaks."*

- **"Walk me through exploiting a simple stack-based buffer overflow"**
  - *"First, I'd identify the buffer size using fuzzing or static analysis. Then calculate the offset to the return address. Create a payload that fills the buffer, overwrites the return address with the address of my shellcode (or a ROP chain if NX is enabled), and send it to the vulnerable program. When the function returns, execution jumps to my code."*

- **"What's the difference between stack and heap overflows?"**
  - *"Stack overflows target local variables and return addresses in function call frames, while heap overflows target dynamically allocated memory. Stack overflows typically aim to control execution flow via return addresses. Heap overflows often target function pointers, vtables, or metadata to achieve code execution."*

### STAR Story
> **Situation:** Penetration test revealed a legacy internal application that crashed when submitting long usernames.
> **Task:** Determine if the crash was exploitable and assess risk to the organization.
> **Action:** Used `gdb` with pattern generation to identify the exact offset to the return address (272 bytes). Verified I could control EIP. Checked protections—no ASLR, no NX. Developed proof-of-concept exploit that spawned a shell with elevated privileges. Demonstrated to stakeholders by popping calc instead of malicious payload.
> **Result:** Application was immediately taken offline. Vendor patched the vulnerability within 2 weeks. Recommended input validation, bounds checking, and compiler protections for all internal development going forward.

## ✅ Best Practices
- Use safe functions: `strncpy`, `snprintf`, `fgets` instead of `strcpy`, `sprintf`, `gets`
- Enable all compiler protections (stack canaries, ASLR, DEP/NX)
- Perform input validation and bounds checking
- Use memory-safe languages when possible (Rust, Go)
- Conduct regular code audits and fuzzing
- Deploy [[Intrusion Detection System (IDS)|IDS]]/[[Intrusion Prevention System (IPS)|IPS]] with exploit signatures
- Keep systems patched

## ❌ Common Misconceptions
- **"Buffer overflows are a solved problem"** → Still common in legacy code and C/C++ applications
- **"Modern OSes completely prevent buffer overflows"** → Protections make exploitation harder, not impossible
- **"Only remote services are at risk"** → Local privilege escalation via buffer overflows is common
- **"Overflow must be huge to be exploitable"** → Off-by-one vulnerabilities can be exploited

## 🔗 Related Concepts
- [[Memory Corruption]]
- [[Privilege Escalation]]
- [[Exploit]]
- [[Return-Oriented Programming (ROP)]]
- [[CVE-2017-0144]]
- [[EternalBlue]]
- [[012 ⚔️ Red Team & Offensive Security MOC]]
- [[014 🦠 Malware Analysis & Forensics MOC]]
- [[Reverse Engineering]]

## 📚 References
- MITRE CWE-120: Buffer Overflow
- OWASP: Buffer Overflow
- https://github.com/Crypto-Cat/CTF/blob/main/pwn/binary_exploitation_101/README.md
- "Smashing The Stack For Fun And Profit" - Aleph One
- "The Shellcoder's Handbook"
