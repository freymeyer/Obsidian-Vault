---
tags:
  - "#cybersecurity/detection-engineering/testing"
aliases:
  - ART
  - Red Canary Atomic Red Team
---

# Atomic Red Team

> One-liner: A library of small, testable adversary technique executions mapped to MITRE ATT&CK, used to validate detections and controls.

## 🎯 What Is It?
Open-source tests that safely emulate specific TTPs ("atomics"). Each test includes prerequisites, execution steps, and cleanup. Useful for purple teaming, detection gap analysis, and SIEM/EDR validation.

## 🤔 Why It Matters
- Proves whether detections actually fire in your environment.
- Enables continuous regression testing of controls.
- Standardises TTP coverage across teams.

## 🔬 How It Works
### Core Principles
1. Map tests to ATT&CK technique IDs (e.g., T1486).
2. Keep tests minimal and reproducible.
3. Measure outcomes (alerts, logs, telemetry).

### Technical Deep-Dive
- Runner: `Invoke-AtomicTest` PowerShell module on Windows; YAML test specs.
- Telemetry targets: [[Sysmon]], Windows Event Log, EDR, network sensors.
- CI/CD option: schedule atomics in non-prod with guardrails.

## 🛡️ Detection & Prevention
### How to Detect
- Build detections from known telemetry signatures per atomic.

### How to Prevent / Mitigate
- Use atomics to validate prevent/contain policies (ASR, SRP, EDR).

## 🎤 Interview Angles
- "How do you use Atomic Red Team to validate a ransomware detection?"

## ✅ Best Practices
- Tag tests with owner, coverage, and success criteria.
- Capture event IDs and artifacts as acceptance criteria.

## ❌ Common Misconceptions
- Atomics are not malware; they are safe, minimal emulations when used per guidance.

## 🔗 Related Concepts
- [[MITRE ATT&CK]]
- [[Detection Engineering]]
- [[Sysmon]]

## 📚 References
- https://github.com/redcanaryco/atomic-red-team
