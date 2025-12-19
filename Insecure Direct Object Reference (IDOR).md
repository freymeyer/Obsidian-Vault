---
tags:
  - "#cybersecurity/web-sec/access-control"
---


Type of access control vulnerability that arises when an application uses user-supplied input to access objects directly. The term IDOR was popularized by its appearance in the OWASP 2007 Top Ten.

Alternative name is [[Authorization Bypass]].

**Related Concepts:**

- [[Authentication]]
    
- [[Authorization]]
    
- [[Privilege Escalation#Horizontal privilege escalation]]
    

![Image of IDOR vulnerability architecture diagram](https://encrypted-tbn3.gstatic.com/licensed-image?q=tbn:ANd9GcSfuJmewC9pPRXq8ugwx5DO26-mNh574FaUsB5l4nkFnUBj-VaSd0S91i9ZklfW1S5PrDHywCKgqDQQKvE-s2JKWx4cE58DmylV3xHR7msE7ca5Tvc)

Shutterstock

Explore

## Core Principles

Authorization cannot happen before authentication.

If the application doesn't know who you are, it cannot verify what permissions your user has. If an IDOR doesn't require you to authenticate (login or provide session information), you must fix authentication first before fixing the authorization issue.

## Technical Mechanism (The "Why")

- **The Flawed Query:** The vulnerability exists because the database query looks for the object ID but ignores the user ID.
    
    - _Vulnerable SQL Example:_
        
        > `SELECT person, address, status FROM Packages WHERE packageID = value;`
        
    - _Secure Logic:_ The query _should_ be: `WHERE packageID = value AND ownerID = currentUser`
        
- **Session Management:**
    
    - Authentication isn't just a one-time login; it relies on **Session Information** (cookies/tokens).
        
    - The application must validate this session info against the requested Object ID on _every_ request.
        

## IDOR Variations (The "Disguises")

|**Type**|**Appearance Example**|**Exploitation Method**|
|---|---|---|
|**Simple / Sequential**|`user_id=10`|Change the integer to `11`, `12`, etc.|
|**Encoded**|`id=Mg==`|Decode Base64 (`Mg==` $\rightarrow$ `2`), increment, then re-encode.|
|**Hashed**|`id=c4ca42...`|Identify hash (MD5, SHA1). Requires knowing the input seed to replicate the hash.|
|**Algorithmic / UUID**|`uuid=...`|If **UUID v1** is used, it is time-based. It can be brute-forced if the generation time is known.|

## Practical Discovery Techniques

### Where to Look

- **URL Parameters:** Look for IDs in the address bar (e.g., `?packageID=1001`).
    
- **AJAX/API Requests:** Use browser **Developer Tools (Network Tab)** to spot background requests (e.g., `view_accountinfo`) that might pass user IDs not visible in the URL.
    
- **Local Storage:** Sometimes IDs are stored client-side in the browser's **Storage Tab** (e.g., `auth_user` data entries).
    

### Essential Tools

- **UUID Decoder:** checks if an ID is **UUID v1**.
    
    - _Mechanism:_ UUID v1 consists of the host **MAC address** + current **timestamp**.
        
    - _Vulnerability:_ Since the MAC is static and time is linear, randomness is an illusion.
        
- **Hash Identifier:** Helps identify if a random string is MD5, SHA1, etc., allowing for potential rainbow-table attacks.
    
- **Burp Suite / Zap:** For capturing and modifying requests (Repeater).
    

### UUID Prediction Math

If the app uses UUID v1, and you know the generation time window (e.g., "between 20:00 - 21:00"), you can generate all possible UUIDs for that range.

- _Math:_ $60 \text{ minutes} \times 60 \text{ seconds} = 3,600$ seeds to brute force.
    

## Testing Workflow (Iterate & Observe)

1. **Capture:** Inspect HTTP requests (GET, POST, PUT, DELETE).
    
2. **Identify:** Locate parameters that look like references (`id`, `user`, `account`, `file`).
    
3. **Decode:** If the value looks complex, try Base64 decoding or Hash Identification.
    
4. **Alter:**
    
    - Increment/Decrement numbers.
        
    - Swap UUIDs with another user's UUID (if known).
        
    - Pollute parameters (e.g., change `?id=10` to `?id=10&id=11`).
        
5. **Observe:** Did the server return data? Did it return _different_ data?
    

## Mitigation (How to Fix)

- **Mandatory Access Control:** Do not rely on hiding IDs. The server must check: _"Does the logged-in user have permission to view this specific object?"_
    
- **Indirect Object References (Map Approach):** Instead of exposing the database Key (e.g., `1001`) to the user, use a temporary session-based map.
    
    - _Example:_ User asks for `package #1`.
        
    - _Server Map:_ `User's #1` $\rightarrow$ `Database ID #1001`.
        
- **Unpredictable IDs:** Use **UUID v4** (completely random) instead of sequential numbers or UUID v1.
    
    - _Note:_ Random IDs are a defense-in-depth measure, not a replacement for access checks.
        
- **Logging:** Monitor for failed access attempts (e.g., User A trying to access User B's data multiple times).