---
tags:
  - "#cybersecurity/application-security"
  - "#infrastructure/concurrency"
aliases:
  - Race Conditions
  - Concurrency Attacks
---

# Race Condition

> **One-liner:** A vulnerability that occurs when a system's output is dependent on the sequence or timing of other uncontrollable events.

## 🎯 What Is It?
A **Race Condition** happens when two or more operations must execute in the correct sequence to function correctly, but the program does not guarantee that order. In web applications, this typically occurs when multiple concurrent requests access or modify shared resources (like database records, global variables, or file systems) simultaneously.

If the application logic assumes that one action will finish before another starts—but doesn't enforce it—an attacker can exploit the time gap (race window) to manipulate the outcome.

## 🤔 Why It Matters
Race conditions can lead to critical business logic failures, such as:
- **Double Spending:** Spending the same funds twice before the balance updates.
- **Inventory Bypass:** Purchasing more items than are available in stock.
- **Privilege Escalation:** Modifying permissions while a check is in progress.
- **Data Corruption:** Inconsistent states in databases.

## 🔬 How It Works

### Core Principles
1.  **Concurrency:** Multiple processes or threads running at the same time.
2.  **Shared State:** Accessing the same resource (variable, file, DB row).
3.  **Lack of Atomicity:** Operations that should be indivisible are interrupted or interleaved.

### Technical Deep-Dive
The most common scenario is the **Time-of-Check to Time-of-Use (TOCTOU)** flaw:
1.  **Check:** The system checks if a condition is true (e.g., "Is stock > 0?").
2.  **Gap:** A small window of time passes.
3.  **Use:** The system performs the action (e.g., "Deduct stock").

If two requests hit the "Check" phase simultaneously before either reaches the "Use" phase, both pass the check, leading to an invalid state (e.g., stock becomes -1).

## 🛡️ Detection & Prevention

### How to Detect
- **Blackbox Testing:** Sending multiple parallel requests to the same endpoint (e.g., using Burp Suite's "Send group in parallel" or Turbo Intruder).
- **Code Review:** Looking for non-atomic operations on shared resources (e.g., `SELECT` then `UPDATE` instead of a single atomic query).

### How to Prevent / Mitigate
- **Atomic Transactions:** Ensure database operations (Check + Update) happen in a single, indivisible transaction.
- **Locking Mechanisms:** Use pessimistic locking (`SELECT FOR UPDATE`) to prevent other transactions from reading the data until the current one finishes.
- **Idempotency Keys:** Assign unique IDs to requests so the server processes them only once.
- **Rate Limiting:** Limit the number of requests a user can make in a short timeframe to reduce the attack surface.

## 📊 Types/Categories

| Type | Description | Example |
|------|-------------|---------|
| **TOCTOU** (Time-of-Check to Time-of-Use) | Data changes between the check and the usage. | Checking balance -> Withdrawing money (balance changes in between). |
| **Shared Resource** | Multiple systems modify the same data without sync. | Two admins editing the same user config simultaneously. |
| **Atomicity Violation** | A multi-step process is interrupted or interleaved. | Payment succeeds, but order creation fails or is duplicated. |

## 🎤 Interview Angles

### Common Questions
- "What is a Race Condition and how would you test for it?"
- "Explain the difference between a Race Condition and a Deadlock."
- "How do you fix a Race Condition in a database transaction?"

### STAR Story
> **Situation:** We had an e-commerce platform where users could buy limited-edition items.
> **Task:** Prevent users from buying more items than were in stock during high-traffic launches.
> **Action:** I identified a race condition where the stock check and deduction were separate queries. I implemented database-level transactions with row locking to ensure atomicity.
> **Result:** Eliminated overselling issues and ensured inventory integrity during flash sales.
