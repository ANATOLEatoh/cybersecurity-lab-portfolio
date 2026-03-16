SOC Investigation Case Study
## Suspicious Account Activity on DESKTOP-JGU95L2

**Analyst:** ANATOLEatoh  
**Date:** March 2026  
**Severity:** High  
**Status:** Investigated

---

## Executive Summary
During routine monitoring of Windows Security 
logs via Splunk SIEM, suspicious account activity 
was detected on host DESKTOP-JGU95L2. Investigation 
revealed the creation of two unauthorised user 
accounts (HackerTest and TempUser), followed by 
privilege escalation and repeated enumeration of 
local accounts. This report documents the timeline, 
evidence, analysis and recommendations.

---

## Environment
- **Host:** DESKTOP-JGU95L2
- **OS:** Windows 10
- **SIEM:** Splunk Enterprise
- **Log Source:** Windows Security Event Log
- **Forwarder:** Splunk Universal Forwarder

---

## Detection Method
Splunk SIEM alert triggered on unusual account 
creation and group membership change events. 
Investigation initiated on review of Security 
Event Log data forwarded from endpoint.

---

## Evidence and Timeline

### Phase 1 — Account Creation (EventCodes 4720, 4722, 4724)
Two unauthorised accounts were created on the system:
- **HackerTest** — created 28/02/2026 20:22
- **TempUser** — created 28/02/2026 21:16

EventCode 4720 indicates a new local user account 
was created. EventCodes 4722 and 4724 confirm the 
accounts were enabled immediately after creation.

### Phase 2 — Privilege Escalation (EventCode 4732)
27 EventCode 4732 events were detected indicating 
members being added to privileged security groups. 
This represents a significant escalation of 
privileges beyond standard user access.

Key timestamps:
- 01/03/2026 00:34 — First group membership change
- 14/03/2026 23:14 — Additional group changes
- 15/03/2026 00:49 — Most recent group changes

### Phase 3 — Account Enumeration (EventCode 4798)
476 EventCode 4798 events were recorded showing 
repeated enumeration of local user accounts. This 
behaviour is consistent with an attacker mapping 
out available accounts for lateral movement or 
further exploitation.

Accounts targeted during enumeration:
- HackerTest (97 enumeration events)
- TempUser (enumeration events detected)
- Administrator, Guest, DefaultAccount

### Phase 4 — Sustained Access (EventCode 4624)
262 successful login events recorded across 
multiple accounts and logon types, confirming 
sustained access to the system over multiple days.

---

## Splunk Queries Used

**Account creation detection:**
```
index=* source="WinEventLog:Security" 
(Account_Name="HackerTest" OR Account_Name="TempUser")
| table _time, EventCode, Account_Name, host
| sort _time
```

**Privilege escalation detection:**
```
index=* source="WinEventLog:Security" EventCode=4732
```

**Account enumeration detection:**
```
index=* source="WinEventLog:Security" EventCode=4798
| stats count by Account_Name
| sort -count
```

**Login activity baseline:**
```
index=* source="WinEventLog:Security" EventCode=4624
| stats count by Account_Name, Logon_Type
| sort -count
```

---

## MITRE ATT&CK Mapping

| Technique | ID | Description |
|-----------|-----|-------------|
| Create Account | T1136.001 | Local account HackerTest and TempUser created |
| Account Manipulation | T1098 | Accounts added to privileged groups |
| Account Discovery | T1087.001 | Repeated local account enumeration |
| Valid Accounts | T1078 | Sustained access using created accounts |

---

## Analysis
The sequence of events is consistent with a 
post-compromise account persistence technique. 
An attacker with initial access created two 
backdoor accounts, escalated their privileges 
by adding them to security groups, then 
repeatedly enumerated local accounts — 
likely to identify further targets or 
verify their access.

The sustained pattern of EventCode 4798 over 
multiple days suggests the attacker maintained 
access and continued reconnaissance activity.

---

## Recommendations

1. **Immediate** — Disable and delete HackerTest 
   and TempUser accounts
2. **Immediate** — Review all group membership 
   changes from EventCode 4732 events and revert 
   unauthorised changes
3. **Short term** — Implement alerting rule for 
   EventCode 4720 to notify SOC on any new 
   account creation
4. **Short term** — Implement alerting rule for 
   EventCode 4732 to notify SOC on any privilege 
   escalation
5. **Long term** — Deploy privileged access 
   management solution to restrict local admin 
   account creation
6. **Long term** — Enable MFA for all local 
   administrator accounts

---

## Detection Rules Created

**New Account Alert:**
```
index=* source="WinEventLog:Security" EventCode=4720
| alert if count > 0
```

**Privilege Escalation Alert:**
```
index=* source="WinEventLog:Security" EventCode=4732
| alert if count > 0
```

---

## Screenshots
[Attach your four evidence screenshots here]

---

*Investigation conducted as part of personal 
cybersecurity lab practice — BSc Cybersecurity 
and Forensic Computing, University of Portsmouth*
