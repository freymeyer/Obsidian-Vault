---
tags:
  - "#cybersecurity/ics"
  - "#protocols/modbus"
aliases:
  - Modbus Protocol
---

# Modbus

## What is Modbus?

Modbus is the communication protocol that industrial devices use to talk to each other. Created in 1979 by Modicon (now Schneider Electric), it's one of the oldest and most widely deployed industrial protocols in the world. Its longevity isn't due to sophisticated features—quite the opposite. Modbus succeeded because it's simple, reliable, and works with almost any device.

Think of Modbus as a basic request-response conversation:
- **Client** (your computer): "PLC, what's the current value of register 0?"
- **Server** (the PLC): "Register 0 currently holds the value 1."

This simplicity makes Modbus easy to implement and debug, but it also means security was never a consideration. There's no authentication, no encryption, no authorisation checking. Anyone who can reach the Modbus port can read or write any value.

## Modbus Data Types

Modbus organises data into four distinct types, each serving a specific purpose in industrial automation:

| Type | Purpose | Values | Example Use Cases |
|---|---|---|---|
| **Coils** | Digital outputs (on/off) | 0 or 1 | Motor running? Valve open? Alarm active? |
| **Discrete Inputs** | Digital inputs (on/off) | 0 or 1 | Button pressed? Door closed? Sensor triggered? |
| **Holding Registers** | Analogue outputs (numbers) | 0-65535 | Temperature setpoint, motor speed, zone selection |
| **Input Registers** | Analogue inputs (numbers) | 0-65535 | Current temperature, pressure reading, flow rate |

The distinction between inputs and outputs is important. **Coils** and **Holding Registers** are writable—you can change their values to control the system. **Discrete Inputs** and **Input Registers** are read-only—they reflect sensor measurements that you observe but cannot directly modify.

## Modbus Addressing

Each data point in Modbus has a unique **address**. When you want to read or write a specific value, you reference it by its address number.

**Critical detail:** Modbus addresses start at 0, not 1. This zero-indexing catches many beginners off guard. When documentation mentions "Register 0," it literally means the first register, not the second.

## Modbus TCP vs Serial Modbus

Originally, Modbus operated over serial connections using RS-232 or RS-485 cables. Devices were physically connected in a network, and this physical isolation provided a degree of security—you needed physical access to the wiring to intercept or inject commands.

Modern industrial systems use **Modbus TCP**, which encapsulates the Modbus protocol inside standard TCP/IP network packets. Modbus TCP servers listen on **port 502** by default.

This network connectivity brings enormous benefits—remote monitoring, easier integration with business systems, and centralised management. But it also exposes these historically isolated systems to network-based attacks.

## The Security Problem

Modbus has no built-in security mechanisms:

- **No authentication:** The protocol doesn't verify who's making requests. Any client can connect and issue commands.
- **No encryption:** All communication happens in plaintext. Anyone monitoring network traffic can see exactly what values are being read or written.
- **No authorisation:** There's no concept of permissions. If you can connect, you can read and write anything.
- **No integrity checking:** Beyond basic checksums for transmission errors, there's no cryptographic verification that commands haven't been tampered with.
