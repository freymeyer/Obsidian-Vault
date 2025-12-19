---
tags:
  - "#cybersecurity/web-sec/injection"
---

SQL Injection (SQLi) relies on attacking the database that sits behind a website and occurs when applications build queries through string concatenation instead of using parameterized queries, allowing attackers to alter the intended SQL command and access or manipulate data. In 2023, an SQLi vulnerability in MOVEit, a file-transfer software, was exploited, affecting over 2,700 organizations, including U.S. government agencies, the BBC, and British Airways.

**LOG ENTRIES**  
198.51.100.22 - - [03/Oct/2025:09:03:11 +0100] "POST /login.php HTTP/1.1" 200 642 "-" "python-requests/2.31.0" "username=alice%27+OR+1%3D1+--+-&password=test"  
  
---  
  The PHP code stores user and password inputs as strings, which can be used to inject SQL if the inputs are manipulated.
  
The variables `$user` and `$pass` are set using `??`, which treats them as if they are strings. If the user's input is a number (e.g., `123`), it can be converted to a string, which can then be used in an SQL query like `SELECT * FROM users WHERE username = ?`. This is a classic example of **SQL injection**.

3. **Best practices for preventing similar issues**:  
- Use **prepared statements** or **parameterized queries** to prevent SQL injection.  
- Sanitize all user inputs (e.g., using `filter_input` or `htmlspecialchars`) before storing them.  
- Avoid using `??` and instead use `isset()` or `empty()` to check for values.  
  
4. **Tools and techniques for code security testing**:  
- Use **OWASP ZAP** or **SonarQube** to detect SQL injection vulnerabilities.  
- Implement **input validation** and **output encoding** to prevent XSS or other attacks.
### 1. Analysis of the Logs  
**2.1. Log Entry Details**  
- **IP Address**: 198.51.100.22 – A user’s IP address.  
- **Time and Date**: 2025 October 3rd at 09:03.  
- **URL**: "/login.php" – A vulnerable web application endpoint.  
- **Username**: "alice" – A user attempting to log in.  
- **SQL Injection**: The password parameter contains `"username=alice%27+OR+1%3D1+--+-&password=test"`, indicating an SQL injection attempt.