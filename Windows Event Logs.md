---
dg-publish: true
tags:
  - "#infrastructure/windows/logging"
aliases:
  - Event Viewer
  - Windows Logs
---

# Windows Event Logs

> One-liner: Built-in Windows logging subsystem capturing system, security, application, and application-specific provider events.

## 🎯 What Is It?
Event Logs store structured records from the OS and applications. Key channels include System, Application, Security, and provider logs like Microsoft-Windows-Sysmon/Operational.

## 🤔 Why It Matters
- Foundation for investigations, auditing, and detections.
- Supports compliance and forensic timelines.
- Feeds SIEM and alerting pipelines.

## 🔬 How It Works
### Core Principles
1. Channels/providers emit events with IDs and fields.
2. Event Log service controls collection and persistence.
3. Forwarding (WEF) centralises logs for analysis.

### Technical Deep-Dive
- Service: `HKLM\SYSTEM\CurrentControlSet\Services\EventLog\Start` → `2` (Automatic) recommended; `4` disables.
- Forwarding: Windows Event Forwarding (WEF) via GPO/Subscriptions.
- Tools: Event Viewer, `Get-WinEvent`, `wevtutil`.

## 🛡️ Detection & Prevention
### How to Detect
- Monitor service state and critical channels available.

### How to Prevent / Mitigate
- Baseline and harden audit policies via GPO.
- Enable [[Sysmon]] for richer telemetry.

## 🔗 Related Concepts
- [[Sysmon]]
- [[Security Information and Event Management system (SIEM)]]
- [[Audit Logon Events]]
