# Attack → Detection Mapping
## April Offensive Security Training
### ANATOLEatoh | BSc Cybersecurity & Forensic Computing

---

## Overview
This document maps each offensive technique 
practised this month to its corresponding 
defensive detection method. Understanding both 
sides of an attack is essential for SOC analysts 
and penetration testers.

---

## Attack 1 — Network Reconnaissance (Nmap)

### Attack
**Tool:** Nmap  
**MITRE ATT&CK:** T1046 — Network Service Discovery

**Commands Used:**
```bash
nmap -p- 192.168.56.101
nmap -sV -sC 192.168.56.101
nmap -O 192.168.56.101
nmap -A 192.168.56.101
```

**Findings:**
- Target: Windows 10 (version 2004)
- Open ports: 135 (RPC), 139 (NetBIOS), 445 (SMB)
- SMB versions 2 and 3 running
- Message signing optional — potential relay risk
- SMB1 disabled — EternalBlue not applicable

### Detection
**Method:** Network traffic analysis and 
Windows firewall logs

**Splunk Detection Query:**
```
index=* sourcetype=WinEventLog 
| search dest_port=445 OR dest_port=135 
OR dest_port=139
| stats count by src_ip, dest_port
| where count > 10
| sort -count
```

**Indicators of Compromise:**
- Multiple port scan attempts from single IP
- Sequential port connections in short timeframe
- Unusual source IP connecting to admin ports

**Defensive Recommendations:**
- Enable Windows Firewall logging
- Alert on port scans from unknown IPs
- Disable unnecessary services and ports

---

## Attack 2 — SMB Enumeration (Metasploit)

### Attack
**Tool:** Metasploit Framework  
**MITRE ATT&CK:** T1135 — Network Share Discovery

**Modules Used:**
```
auxiliary/scanner/smb/smb_version
auxiliary/scanner/smb/smb_enumshares
auxiliary/scanner/smb/smb_ms17_010
auxiliary/scanner/smb/smb_login
```

**Findings:**
- SMB 3.1.1 confirmed running
- EternalBlue (MS17-010) — NOT vulnerable
  (Windows 10 patched)
- SMB authentication blocking anonymous access
- Credential brute force attempted — unsuccessful

### Detection
**Method:** Windows Security Event Log

**Splunk Detection Query:**
```
index=* source="WinEventLog:Security" 
EventCode=4625
| stats count by src_ip, Account_Name
| where count > 5
| sort -count
```

**Indicators of Compromise:**
- Multiple failed SMB authentication attempts
- EventCode 4625 from external IP
- Anonymous SMB connection attempts

**Defensive Recommendations:**
- Enforce SMB signing to prevent relay attacks
- Disable SMB1 protocol (already done)
- Implement account lockout after failed attempts
- Monitor EventCode 4625 for brute force

---

## Attack 3 — SQL Injection: Hidden Data

### Attack
**Tool:** Burp Suite / Browser URL manipulation  
**Platform:** PortSwigger Web Security Academy  
**MITRE ATT&CK:** T1190 — Exploit Public Facing Application

**Payload:**
```
/filter?category='+OR+1=1--
```

**Result:** All hidden database records exposed
including unreleased products

### Detection
**Method:** Web Application Firewall / Access Logs

**Splunk Detection Query:**
```
index=* sourcetype=access_log
| search uri="*'+OR*" OR uri="*1=1*" 
OR uri="*'--*"
| stats count by src_ip, uri
| where count > 1
```

**Indicators of Compromise:**
- Single quote characters in URL parameters
- SQL keywords in GET/POST parameters
- Unusual volume of data returned from queries

**Defensive Recommendations:**
- Use parameterised queries / prepared statements
- Deploy Web Application Firewall
- Input validation and sanitisation
- Least privilege database accounts

---

## Attack 4 — SQL Injection: Auth Bypass

### Attack
**Tool:** Burp Suite  
**MITRE ATT&CK:** T1078 — Valid Accounts

**Payload:**
```
Username: administrator'--
Password: anything
```

**Result:** Logged in as administrator without 
knowing the password — complete auth bypass

### Detection
**Method:** Application and Authentication Logs

**Splunk Detection Query:**
```
index=* sourcetype=access_log
| search uri="*login*" AND 
(uri="*'--*" OR uri="*'+OR*")
| stats count by src_ip
| where count > 1
```

**Indicators of Compromise:**
- Special characters in login username field
- Successful login after failed attempts
- Admin account login from unusual IP/time

**Defensive Recommendations:**
- Parameterised queries for all login logic
- MFA on administrator accounts
- Alert on admin login from new locations
- Failed login attempt monitoring

---

## Attack 5 — SQL Injection: UNION Attack

### Attack
**Tool:** Burp Suite  
**MITRE ATT&CK:** T1190 — Exploit Public Facing Application

**Payloads Tested:**
```
'+UNION+SELECT+NULL--         (failed - wrong columns)
'+UNION+SELECT+NULL,NULL--    (failed - wrong columns)
'+UNION+SELECT+NULL,NULL,NULL-- (success - 3 columns)
```

**Result:** Determined query returns 3 columns —
foundation for data extraction attacks

### Detection
**Method:** Web Application Firewall / Access Logs

**Splunk Detection Query:**
```
index=* sourcetype=access_log
| search uri="*UNION*" AND uri="*SELECT*"
| stats count by src_ip, uri
| sort -count
```

**Indicators of Compromise:**
- UNION and SELECT keywords in URL parameters
- Multiple similar requests with incrementing 
  NULL values
- Unusual response sizes from database queries

**Defensive Recommendations:**
- WAF rules blocking UNION/SELECT in parameters
- Database query monitoring for UNION statements
- Error handling that doesn't reveal DB structure

---

## Attack 6 — Cross-Site Scripting (XSS)

### Attack
**Tool:** Burp Suite / Browser  
**MITRE ATT&CK:** T1059.007 — JavaScript Execution

**Payload:**
```html
<script>alert(1)</script>
```

**Result:** JavaScript executed in browser 
context — reflected XSS confirmed

### Detection
**Method:** Web Application Firewall / Access Logs

**Splunk Detection Query:**
```
index=* sourcetype=access_log
| search uri="*script*" OR uri="*alert(*" 
OR uri="*<script*"
| stats count by src_ip, uri
| sort -count
```

**Indicators of Compromise:**
- HTML/JavaScript tags in URL parameters
- Script keywords in GET/POST requests
- Unusual characters in search fields

**Defensive Recommendations:**
- Output encoding for all user-supplied data
- Content Security Policy (CSP) headers
- WAF rules blocking script tags in input
- Input validation on all form fields

---

## Summary Table

| Attack | Tool | MITRE ID | Detection Method |
|--------|------|----------|-----------------|
| Network Scan | Nmap | T1046 | Firewall logs, port scan alerts |
| SMB Enumeration | Metasploit | T1135 | EventCode 4625, auth logs |
| SQLi Hidden Data | Burp Suite | T1190 | WAF, access logs |
| SQLi Auth Bypass | Burp Suite | T1078 | Auth logs, login monitoring |
| SQLi UNION Attack | Burp Suite | T1190 | WAF, UNION keyword detection |
| XSS Reflected | Burp Suite | T1059.007 | WAF, script tag detection |

---

## Key Learning
Understanding attacks from both offensive and 
defensive perspectives is the core skill of a 
cybersecurity professional. Every attack leaves 
traces — the defender's job is knowing where 
to look.
