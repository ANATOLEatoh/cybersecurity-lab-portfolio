# Memory Forensics Analysis
## Tool: Volatility 3 Framework 2.27.0
## Evidence: charlie-2009-12-02.mddramimage
## Case: M57-Patents Insider Threat Investigation

---

## Overview
Memory forensics analysis performed on a RAM 
image captured from Charlie's workstation on 
2009-12-02 at 00:27:56 UTC. Analysis revealed 
active patent-related automation scripts, 
simultaneous email and browser activity, and 
evidence of memory acquisition from an 
external drive.

---

## System Information

| Property | Value |
|----------|-------|
| OS | Windows XP SP3 |
| Build | 2600.xpsp_sp3_gdr.090804-1435 |
| Architecture | 32-bit (x86) |
| Capture Time | 2009-12-03 00:27:56 UTC |
| Memory Size | 2.0GB |
| Tool Used | Volatility 3 Framework 2.27.0 |

---

## Finding 1 — Patent Automation Script
**Severity: Critical**
**MITRE ATT&CK: T1059.006 — Python**

A Python script named patentauto.py was 
actively running at time of capture:
```
PID: 3208
Parent: cmd.exe (PID 3920)
Command: c:\Python26\python.exe patentauto.py
Started: 2009-12-02 17:03:47 UTC
```

The script name patentauto.py strongly indicates 
automated collection or processing of patent data. 
This is direct evidence of automated insider 
threat activity running at time of memory capture.

---

## Finding 2 — Simultaneous Browser and Email Activity
**Severity: High**
**MITRE ATT&CK: T1114 — Email Collection**

Firefox and Thunderbird were launched 
simultaneously at 2009-12-02 16:54:
```
PID 3628: firefox.exe — Started 16:54:51 UTC
PID 1724: thunderbird.exe — Started 16:54:55 UTC
```

Both applications launched within 4 seconds 
of each other suggesting deliberate simultaneous 
use of browser and email client — consistent 
with accessing web storage and emailing 
documents concurrently.

---

## Finding 3 — Multiple Command Prompt Sessions
**Severity: High**
**MITRE ATT&CK: T1059.003 — Windows Command Shell**

Two separate command prompt sessions were 
active at time of capture:
```
PID 3920: cmd.exe — Started 2009-12-02 17:03:35
  └── PID 3208: python.exe patentauto.py — 
      Started 2009-12-02 17:03:47

PID 1720: cmd.exe — Started 2009-12-03 00:10:13
  └── PID 2220: mdd_1.3.exe — Memory acquisition
      Started 2009-12-03 00:27:55
```

The first cmd.exe launched patentauto.py 
12 seconds after opening. The second cmd.exe 
launched the memory acquisition tool — 
confirming the RAM capture was performed 
from within the system itself.

---

## Finding 4 — Memory Acquisition from External Drive
**Severity: Medium**

The memory acquisition tool ran from 
drive Z: — an external drive:
```
PID 2220: mdd_1.3.exe
Command: z:\mdd_1.3.exe -o z:\charlie-2009-12-02.ram
Started: 2009-12-03 00:27:55 UTC
```

Drive Z: is not a standard Windows drive letter 
indicating an external USB drive was used to 
run the capture tool and store the output — 
consistent with forensic acquisition procedure.

---

## Finding 5 — OpenOffice Active
**Severity: Low**

OpenOffice was running at time of capture:
```
PID 3512: soffice.exe — Started 02:53:39 UTC
PID 3532: soffice.bin — Started 02:53:39 UTC
```

OpenOffice was used to access and potentially 
modify documents on the system.

---

## Full Process List Summary

| PID | Process | Started | Significance |
|-----|---------|---------|--------------|
| 3208 | python.exe (patentauto.py) | 17:03:47 | CRITICAL — Patent automation |
| 3628 | firefox.exe | 16:54:51 | High — Browser activity |
| 1724 | thunderbird.exe | 16:54:55 | High — Email client |
| 3920 | cmd.exe | 17:03:35 | High — Launched python script |
| 1720 | cmd.exe | 00:10:13 | High — Launched memory capture |
| 2220 | mdd_1.3.exe | 00:27:55 | Medium — Memory acquisition |
| 3512 | soffice.exe | 02:53:39 | Low — Document editing |

---

## Volatility Commands Used
```bash
# System information
vol -f charlie.mem windows.info

# Running processes
vol -f charlie.mem windows.pslist

# Command line arguments
vol -f charlie.mem windows.cmdline

# Network connections (XP not supported in Vol3)
vol -f charlie.mem windows.netstat
```

---

## Key Conclusions

The memory analysis of Charlie's workstation 
reveals a pattern consistent with deliberate 
insider threat activity:

1. A patent automation script was actively 
   running — suggesting systematic collection 
   of patent data
2. Email and browser were opened simultaneously 
   — consistent with data exfiltration workflow
3. Command prompts were used to execute scripts 
   rather than standard GUI applications — 
   indicating technical knowledge and deliberate 
   concealment
4. Combined with the Autopsy disk analysis of 
   Terry's machine, the evidence suggests a 
   coordinated insider threat involving 
   multiple employees

---

## MITRE ATT&CK Mapping

| Technique | ID | Evidence |
|-----------|-----|---------|
| Python scripting | T1059.006 | patentauto.py running |
| Windows Command Shell | T1059.003 | Two cmd.exe sessions |
| Email Collection | T1114 | Thunderbird active |
| Automated Collection | T1119 | patentauto.py automation |

---

## Screenshots
[Attach Volatility output screenshots here]

---

*Memory forensics conducted as part of May 
DFIR training — BSc Cybersecurity & 
Forensic Computing*
*University of Portsmouth — Placement 
Preparation 2025*
