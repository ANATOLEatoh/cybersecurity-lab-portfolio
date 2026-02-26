Cybersecurity Lab Portfolio
 ANATOLEatoh | BSc Cybersecurity & Forensic Computing
Seeking UK Cybersecurity Placement 2025



About
Hands-on cybersecurity lab portfolio documenting 
practical skills built alongside my degree.
All exercises performed in a controlled virtual 
environment built and configured from scratch.



Important Note
All labs were performed against a Windows 10 VM 
I built and configured myself — not a pre-made 
CTF or vulnerable machine. This required 
understanding both the attacker and defender 
perspective simultaneously, and involved 
independent troubleshooting at every stage.



 Lab Environment
- **Attacker:** Kali Linux — self configured
- **Target:** Windows 10 VM — self built and 
  configured from scratch
- **Server:** Ubuntu Server VM — in progress
- **SIEM:** Splunk Enterprise on host machine
- **Network:** VirtualBox Host-Only Network — 
  manually configured (192.168.56.0/24)
- **Virtualisation:** Oracle VirtualBox

---

 Labs Completed

| # | Lab | Tools Used | Skills Demonstrated |
|---|-----|-----------|-------------------|
| 1 | Lab Setup | VirtualBox | VM configuration, network setup |
| 2 | Network Configuration | ipconfig, ifconfig | IP troubleshooting, connectivity |
| 3 | Nmap Scanning | Nmap | Port scanning, service discovery |
| 4 | SMB Enumeration | smbclient, enum4linux | Service enumeration, recon |
| 5 | SMB Exploitation | Metasploit | Vulnerability exploitation |
| 6 | Splunk SIEM Setup | Splunk, Universal Forwarder | Log ingestion, SIEM architecture |

---

 SIEM & Detection Work

Architecture Built
- Splunk Enterprise installed on host machine
- Splunk Universal Forwarder deployed on Windows VM
- Live Windows Security, System and Application 
  logs forwarding to Splunk via port 9997
- 1,853+ real Windows events ingested and searchable

 Detection Queries Built

Successful Login Detection (EventCode 4624)

index=* source="WinEventLog:Security" EventCode=4624


Failed Login Detection (EventCode 4625)

index=* source="WinEventLog:Security" EventCode=4625


Brute Force Detection Rule

index=* source="WinEventLog:Security" EventCode=4625
| stats count by Account_Name, host
| where count > 2
``



 Key Concepts Demonstrated
- SIEM architecture and log ingestion pipelines
- Windows Security Event Log analysis
- Network scanning and service enumeration
- SMB protocol enumeration and exploitation
- Brute force attack detection logic
- Attacker methodology and defender response
- Lab documentation and evidence collection



Tools Used

Offensive
- Nmap (port scanning and service discovery)
- smbclient (SMB share enumeration)
- enum4linux (Linux SMB enumeration tool)
- Metasploit Framework (exploitation)

Defensive / SIEM
- Splunk Enterprise (log analysis and detection)
- Splunk Universal Forwarder (log collection)
- Windows Event Viewer (native log analysis)

Network Analysis
- Wireshark (packet capture and analysis)

Operating Systems
- Kali Linux
- Windows 10
- Ubuntu Server (in progress)

Infrastructure
- Oracle VirtualBox
- VirtualBox Host-Only Networking



 Current Progress

- [x] Home lab built from scratch
- [x] Network scanning and enumeration
- [x] SMB enumeration and exploitation
- [x] Splunk SIEM deployed and receiving live logs
- [x] Brute force detection query built
- [ ] Ubuntu Server VM configured
- [ ] Windows Event Log deep dive (Week 2 March)
- [ ] SOC investigation case study (End of March)
- [ ] Attack → Detection mapping (End of April)
- [ ] Digital Forensics investigation report (End of May)



Upcoming Labs
- Windows Event Log analysis (EventCodes 4624, 
  4625, 4720, 4732, 7045)
- Atomic Red Team attack simulation
- SOC investigation case study
- Burp Suite web application testing
- Autopsy digital forensics investigation
- Volatility memory forensics



 Platforms
- TryHackMe: [add your profile link here]
- LinkedIn: [add your LinkedIn link here]



Repository Structure
- Numbered lab files (1-lab-setup.md etc.)
- commands-cheatsheet.md — reference sheet
- key-concepts.md — theory and definitions
- screenshots/ — evidence folder
- splunk-siem-setup.md — SIEM documentation



BSc Cybersecurity & Forensic Computing student 
actively building toward a UK placement in 2025.
Repository updated weekly.





