## Detection
- **Manual testing**:
    - Insert single quote `'`; look for errors.
    - Test boolean logic: `OR 1=1` vs `OR 1=2`.
    - Time-based payloads: delays to notice slow responses.
    - OAST payloads: trigger out‑of‑band interactions.
- **Automation**: Use Burp Scanner for rapid scanning.

## Attack types
### 1. Retrieving hidden data
- Inject comment to remove conditions:
    - `…?category=Gifts'--` drops `AND released = 1`, revealing unreleased items. [PortSwigger+1PortSwigger+1](https://portswigger.net/web-security/sql-injection?utm_source=chatgpt.com

### 2. UNION-based data exfiltration
- Use `UNION SELECT` to merge attacker-selected rows.    
- Steps:
    1. Determine column count using `ORDER BY` or `UNION SELECT NULL,...`.
    2. Find which columns accept strings.
    3. Inject:
        `' UNION SELECT username,password FROM users--` 

### 3. Blind SQL Injection
- When no direct data output:
    - **Boolean-based**: compare conditions to infer data:
        `' AND SUBSTRING((SELECT password FROM Users WHERE username='Administrator'),1,1)>'m`
        
        Repeat until resolved. [PortSwigger+8PortSwigger+8PortSwigger+8](https://portswigger.net/web-security/sql-injection/blind?utm_source=chatgpt.com)
        
    - **Error-based**: use errors to leak data:
        
        sql
        
        CopyEdit
        
        `SELECT CAST((SELECT password FROM users LIMIT 1) AS int)`
        
    
    [PortSwigger+9PortSwigger+9PortSwigger+9](https://portswigger.net/web-security/sql-injection/blind?utm_source=chatgpt.com)
    
    - **Time-based**:
        
        sql
        
        CopyEdit
        
        `'…' OR pg_sleep(10)--` 
        
        Observe 10s delay. [PortSwigger](https://portswigger.net/web-security/sql-injection/blind/lab-time-delays?utm_source=chatgpt.com)
        
    - **Out-of-band (OAST)**: trigger DNS/HTTP to exfiltrate.
