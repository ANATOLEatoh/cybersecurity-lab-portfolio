# Digital Forensics Investigation Report
## Case: M57-Patents Insider Threat Investigation
### Suspect: Terry (terry@m57.biz)

**Analyst:** ANATOLEatoh  
**Date:** March 2026  
**Case Number:** M57-001  
**Severity:** Critical  
**Status:** Completed  
**Tool Used:** Autopsy 4.22.1  
**Evidence File:** terry-2009-12-11-001.E01  

---

## Executive Summary
A forensic examination of a disk image taken from 
suspect Terry's workstation on 2009-12-11 revealed 
strong evidence of intellectual property theft and 
deliberate anti-forensic activity. The investigation 
identified external data exfiltration via a portable 
hard drive, suspicious external email communications, 
access to patent-related documents, and the use of 
evidence-wiping software on the day of imaging.

---

## Case Background
M57-Patents is a company specialising in outsourced 
patent searching. The company operated from 
November 13th to December 12th 2009. During this 
period, confidential patent information was suspected 
to have been leaked by an insider. A forensic image 
of Terry's workstation was acquired on 2009-12-11 
for investigation.

---

## Evidence Acquired
- **Disk Image:** terry-2009-12-11-001.E01
- **Image Format:** EnCase E01
- **Source:** Terry's workstation hard drive
- **Acquisition Date:** 2009-12-11
- **Analysis Tool:** Autopsy 4.22.1
- **RAM Image:** charlie-2009-12-02.mddramimage
  (available for memory forensics)

---

## Methodology
1. Disk image loaded into Autopsy 4.22.1
2. Automated ingest modules run including:
   - Recent Activity
   - Email Parser
   - File Type Identification
   - Web Artifacts
3. Manual review of all artifact categories
4. Timeline analysis of key events
5. Keyword searches for patent-related terms

---

## Findings

### Finding 1 — External Hard Drive Connection
**Severity: Critical**

A LaCie Rugged Triple Interface Mobile Hard Drive 
was connected to Terry's workstation twice on 
2009-12-11 — the same day the forensic image 
was taken:

- First connection: 2009-12-11 02:24 GMT
- Second connection: 2009-12-11 16:45 GMT

30 total USB device connections were recorded 
on the system. The LaCie portable hard drive 
is a high-capacity external storage device 
commonly used for data exfiltration. Its 
connection on the day of imaging is highly 
suspicious.

**MITRE ATT&CK:** T1052 — Exfiltration Over 
Physical Medium

---

### Finding 2 — Anti-Forensic Activity
**Severity: Critical**

Three files were found in the Recycle Bin:

1. `savings276.exe` — deleted 2009-12-10 16:39
   Path: C:\Users\terry\Documents\Downloads\
   An unknown executable deleted the day before 
   imaging.

2. `$RTZJMGWR` — deleted 2009-12-09 17:55
   A program shortcut removed from the Start Menu.

3. **CCleaner shortcut** — deleted 2009-12-11 16:02
   Path: C:\Users\terry\Desktop\CCleaner.lnk
   CCleaner is a tool used to wipe browser history,
   temporary files, and traces of system activity.
   Its deletion on the exact day of imaging strongly
   suggests Terry used it to destroy evidence 
   before the investigation.

**MITRE ATT&CK:** T1070 — Indicator Removal on Host

---

### Finding 3 — Suspicious External Communications
**Severity: High**

Email analysis revealed Terry communicated with 
external personal email addresses:

- **ghost.wisp@live.com** — personal Hotmail address
- **t93940@gmail.com** — personal Gmail address

Key email found: Terry sent email to 
ghost.wisp@live.com and tech.nora@hotmail.com 
on 2009-12-02 — subject "Re: GGood for You"

An email with subject **"ADDITIONAL GUIDANCE ON 
PATENT SEARCHING"** was found dated 2009-11-18, 
directly referencing the company's core 
confidential work.

**MITRE ATT&CK:** T1048 — Exfiltration Over 
Alternative Protocol

---

### Finding 4 — Recent Document Access
**Severity: High**

40 recently accessed documents were identified. 
Key suspicious files include:

| File | Path | Date Accessed |
|------|------|---------------|
| 42.zip | C:\Users\terry\Documents\Personal\ | 2009-11-24 |
| Log | C:\Users\terry\Documents\Work\ | 2009-12-03 |
| ATT00022.jpg | C:\Users\terry\Pictures\ | 2009-12-09 |
| urls.txt | C:\Users\terry\Desktop\web\ | 2009-12-04 |
| Resume.doc | C:\Users\terry\Documents\Personal\ | N/A |
| vnc-4_1_2-x86_win32.zip | Downloads | N/A |
| ABCTech_RECEIPT | C:\Users\terry\Documents\Work\ | Multiple |

Notable findings:
- **42.zip** — archive file in Personal folder, 
  likely used to package files for exfiltration
- **Resume.doc** — Terry was job searching while 
  employed, suggesting intent to leave
- **vnc-4_1_2-x86_win32.zip** — VNC remote access 
  software downloaded, potential for remote 
  exfiltration
- **ABCTech_RECEIPT** — ABCTech appears to be an 
  external company, multiple accesses recorded
- Daily log files maintained: 2009-12-02 through 
  2009-12-10 suggesting systematic documentation

**MITRE ATT&CK:** T1005 — Data from Local System

---

### Finding 5 — Email Volume and Patterns
**Severity: Medium**

158 emails were recovered from Terry's mailbox. 
Analysis revealed:

- Regular communication with charlie@m57.biz 
  (colleague/manager)
- Multiple emails to external Gmail and Hotmail 
  addresses
- Email referencing ABCTech — external company
- Communication patterns show increased external 
  email activity in final weeks of employment

**MITRE ATT&CK:** T1114 — Email Collection

---

### Finding 6 — Installed Software
**Severity: Medium**

76 installed programs identified. Notable entries:

- **CCleaner** — evidence wiping tool (shortcut 
  later deleted)
- **VNC** — remote access software (downloaded 
  but not standard business software)
- **OpenOffice** — document editing suite

---

## Timeline of Suspicious Activity

| Date | Time | Activity |
|------|------|----------|
| 2009-11-18 | Unknown | Email: Additional Guidance on Patent Searching |
| 2009-11-24 | 21:40 | Accessed 42.zip in Personal folder |
| 2009-12-02 | Unknown | Daily log file created |
| 2009-12-02 | 21:31 | Email sent to ghost.wisp@live.com |
| 2009-12-03 | 17:27 | Accessed Work\Log file |
| 2009-12-04 | 20:31 | Accessed urls.txt on Desktop |
| 2009-12-09 | 17:55 | Program shortcut deleted from Start Menu |
| 2009-12-09 | 22:04 | Accessed ATT00022.jpg (email attachment) |
| 2009-12-10 | 16:39 | savings276.exe deleted from Downloads |
| 2009-12-11 | 02:24 | LaCie hard drive connected (first time) |
| 2009-12-11 | 16:02 | CCleaner shortcut deleted |
| 2009-12-11 | 16:45 | LaCie hard drive connected (second time) |

---

## Analysis and Conclusions

The forensic evidence presents a consistent 
pattern of insider threat behaviour:

**Phase 1 — Reconnaissance (November 2009)**
Terry accessed patent-related documents and 
began external email communications. The 
presence of a Resume.doc suggests Terry was 
planning to leave the company.

**Phase 2 — Data Collection (Late November — 
Early December 2009)**
Terry systematically accessed work documents, 
maintained daily logs, downloaded a zip archive 
(42.zip) in the Personal folder, and communicated 
with external email addresses including a 
potential recipient at ghost.wisp@live.com.

**Phase 3 — Exfiltration (December 9-11 2009)**
Terry connected a LaCie portable hard drive 
twice on the final day, deleted evidence using 
CCleaner, and removed a suspicious executable 
(savings276.exe) from the Downloads folder. 
The systematic deletion of evidence on the day 
of imaging is a strong indicator of consciousness 
of guilt.

---

## Recommendations

1. **Immediate** — Preserve original E01 image 
   with verified hash values for chain of custody
2. **Immediate** — Analyse contents of 42.zip 
   if recoverable
3. **Immediate** — Subpoena records for 
   ghost.wisp@live.com and t93940@gmail.com
4. **Short term** — Attempt recovery of CCleaner 
   wiped artifacts using file carving tools
5. **Short term** — Analyse LaCie hard drive 
   if recovered
6. **Long term** — Implement DLP (Data Loss 
   Prevention) solution to monitor file transfers
7. **Long term** — Enable email monitoring for 
   external forwarding of sensitive documents

---

## Tools Used
- **Autopsy 4.22.1** — Primary forensic analysis
- **Digital Corpora M57-Patents Dataset** — 
  Evidence source
- **Autopsy Artifact Modules** — Email parser, 
  recent activity, web artifacts

## MITRE ATT&CK Summary

| Technique | ID | Evidence |
|-----------|-----|---------|
| Data from Local System | T1005 | Patent docs accessed |
| Email Collection | T1114 | 158 emails analysed |
| Exfiltration over Physical Medium | T1052 | LaCie HDD connected |
| Indicator Removal on Host | T1070 | CCleaner used |
| Exfiltration over Alternative Protocol | T1048 | External email |

---

## Screenshots
[Attach your Autopsy screenshots here]
- Recent Documents list
- Recycle Bin contents
- USB Device Attached list
- Email list showing external communications

---

*Investigation conducted as part of May DFIR 
training — BSc Cybersecurity & Forensic Computing*
*University of Portsmouth — Placement Preparation 2025*
