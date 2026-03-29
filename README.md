# Cybersecurity Lab Portfolio
### ANATOLEatoh | BSc Cybersecurity & Forensic Computing
### Seeking UK Cybersecurity Placement 2025

---

## About
Hands-on cybersecurity lab portfolio documenting practical 
skills built alongside my degree. All exercises performed 
in a controlled virtual environment built and configured 
from scratch. This is not a guided CTF walkthrough — every 
lab was designed, troubleshot, and documented independently.

---

## Important Note
All offensive labs were performed against a Windows 10 VM 
I built and configured myself from scratch — not a pre-made 
vulnerable or CTF machine. This required understanding both 
the attacker and defender perspective simultaneously and 
involved independent troubleshooting at every stage.

---

## Lab Environment
- **Attacker Machine:** Kali Linux — self configured
- **Target Machine:** Windows 10 VM — self built from scratch
- **Server:** Ubuntu Server VM — in progress
- **SIEM:** Splunk Enterprise — installed on host machine
- **Network:** VirtualBox Host-Only Network — 192.168.56.0/24
- **Virtualisation:** Oracle VirtualBox

---

## Labs Completed

| # | Lab | Tools Used | Skills Demonstrated |
|---|-----|-----------|-------------------|
| 1 | Lab Setup | VirtualBox | VM config, network setup |
| 2 | Network Configuration | ipconfig, ifconfig | IP troubleshooting, connectivity |
| 3 | Nmap Scanning | Nmap | Port scanning, service discovery |
| 4 | SMB Enumeration | smbclient, enum4linux | Service enumeration, recon |
| 5 | SMB Exploitation | Metasploit | Vulnerability exploitation |
| 6 | Splunk SIEM Setup | Splunk, Universal Forwarder | Log ingestion, SIEM architecture |

---

## SIEM and Detection Work

### Architecture Built
- Splunk Enterprise installed on host machine
- Splunk Universal Forwarder deployed on Windows VM
- Live Windows Security, System and Application logs
  forwarding to Splunk via port 9997
- 1,853 real Windows events ingested and searchable
- Host-Only network connectivity confirmed between VM and host

### Detection Queries Built

**Successful Login Detection — EventCode 4624**
```bash
index=* source="WinEventLog:Security" EventCode=4624
```

**Failed Login Detection — EventCode 4625**
```bash
index=* source="WinEventLog:Security" EventCode=4625
```

**Brute Force Detection Rule**
```bash
index=* source="WinEventLog:Security" EventCode=4625
| stats count by Account_Name, host
| where count > 2
```

---

## Key Concepts Demonstrated
- SIEM architecture and log ingestion pipelines
- Windows Security Event Log analysis
- Network scanning and service enumeration
- SMB protocol enumeration and exploitation
- Brute force attack detection logic
- Attacker methodology and defender response
- Independent lab design and troubleshooting
- Professional documentation and evidence collection

---

## Tools Used

**Offensive**
- Nmap — port scanning and service discovery
- smbclient — SMB share enumeration
- enum4linux — Linux SMB enumeration tool
- Metasploit Framework — exploitation

**Defensive / SIEM**
- Splunk Enterprise — log analysis and detection
- Splunk Universal Forwarder — log collection agent
- Windows Event Viewer — native log analysis

**Network Analysis**
- Wireshark — packet capture and traffic analysis

**Operating Systems**
- Kali Linux
- Windows 10
- Ubuntu Server (in progress)

**Infrastructure**
- Oracle VirtualBox
- VirtualBox Host-Only Networking

---

## Current Progress

- [x] Home lab built from scratch
- [x] Network scanning and enumeration
- [x] SMB enumeration and exploitation
- [x] Splunk SIEM deployed and receiving live logs
- [x] Brute force detection query built and tested
- [ ] Ubuntu Server VM fully configured
- [x] Windows Event Log deep dive (EventCodes 4624, 4625, 4720, 4732, 7045)
- [ ] Atomic Red Team attack simulation
- [x] SOC investigation case study
- [x] Attack to Detection mapping with MITRE ATT&CK
- [x] Burp Suite web application testing
- [x] Autopsy digital forensics investigation
- [x] Volatility memory forensics
- [x] Full DFIR investigation report

---

## Upcoming Labs
- Windows Event Log analysis and threat hunting
- Atomic Red Team simulated attack detection
- SOC investigation case study write-up
- Web application testing with Burp Suite
- Digital forensics with Autopsy and Volatility
- Full forensic investigation report (PDF)

---

## Repository Structure
- Numbered lab files — 1-lab-setup.md through 6-splunk-siem-setup.md
- commands-cheatsheet.md — quick reference for all tools
- key-concepts.md — theory, definitions and explanations
- screenshots/ — evidence and lab proof folder
- splunk-siem-setup.md — full SIEM documentation

---

## Platforms
- TryHackMe: https://tryhackme.com/manage-account/account-details
- LinkedIn: www.linkedin.com/in/anatole-ogbeiwi-b16a29354
---

*BSc Cybersecurity and Forensic Computing student at University of Portsmouth actively building toward a UK cybersecurity placement in 2025. This repository is updated weekly as new labs are completed.*
