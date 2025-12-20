---
tags:
  - "#cybersecurity/ics"
  - "#cybersecurity/scada"
aliases:
  - SCADA
---

# Supervisory Control and Data Acquisition (SCADA)

## What is SCADA?

SCADA systems are the "command centres" of industrial operations. They act as the bridge between human operators and the machines doing the work. Think of SCADA as the nervous system of a factory—it senses what's happening, processes that information, and sends commands to make things happen.

## Components of a SCADA System

A SCADA system typically consists of four key components:

1. **Sensors & actuators:** These are the eyes and hands of the system. Sensors measure real-world conditions, such as temperature, pressure, position, and weight. Actuators perform physical actions—motors turn, valves open, robotic arms move.
2. **PLCs (Programmable Logic Controllers):** These are the brains that execute automation logic. They read sensor data, make decisions based on programmed rules, and send commands to actuators.
3. **Monitoring systems:** Visual interfaces like CCTV cameras, dashboards, and alarm panels where operators observe physical processes.
4. **Historians:** Databases that store operational data for later analysis. Every package loaded, every drone launched, every system change gets recorded. This historical data helps identify patterns, troubleshoot problems, and reconstruct what happened during incidents.

## Why SCADA Systems Are Targeted

Industrial control systems, such as SCADA, have become increasingly attractive targets for cybercriminals and nation-state actors. Here's why:

- They often run **legacy software** with known vulnerabilities. Many SCADA systems were installed decades ago and never updated.
- **Default credentials** are commonly left unchanged. Administrators prioritise keeping systems running over changing passwords.
- They're designed for **reliability, not security**. Most SCADA systems were built before cyber security was a significant concern. Authentication, encryption, and access controls were afterthoughts at best.
- They control **physical processes**. Unlike attacking a website or stealing data, compromising SCADA systems has real-world consequences. Attackers can cause blackouts, contaminate water supplies, or sabotage production.
- They're often **connected to corporate networks**. The myth of "air-gapped" industrial systems is largely fiction. Most SCADA systems connect to business networks for reporting, remote management, and data integration.
- Protocols like **Modbus lack authentication**. Many industrial protocols were designed for trusted environments. Anyone who can reach the Modbus port (502) can read and write values without proving their identity.
