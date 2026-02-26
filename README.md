# Cybersecurity Lab Portfolio
### ANATOLEatoh | BSc Cybersecurity & Forensic Computing
### Seeking UK Cybersecurity Placement 2025

## About
Hands-on cybersecurity lab portfolio documenting practical skills built alongside my degree. All exercises performed in a controlled virtual environment built and configured from scratch.

## Important Note
All labs were performed against a Windows 10 VM I built and configured myself — not a pre-made CTF or vulnerable machine. This required understanding both the attacker and defender perspective simultaneously, and involved independent troubleshooting at every stage.

## Lab Environment
- Attacker: Kali Linux — self configured
- Target: Windows 10 VM — self built from scratch
- Server: Ubuntu Server VM — in progress
- SIEM: Splunk Enterprise on host machine
- Network: VirtualBox Host-Only Network 192.168.56.0/24
- Virtualisation: Oracle VirtualBox

## Labs Completed

| # | Lab | Tools Used | Skills Demonstrated |
|---|-----|-----------|-------------------|
| 1 | Lab Setup | VirtualBox | VM configuration, network setup |
| 2 | Network Configuration | ipconfig, ifconfig | IP troubleshooting, connectivity |
| 3 | Nmap Scanning | Nmap | Port scanning, service discovery |
| 4 | SMB Enumeration | smbclient, enum4linux | Service enumeration, recon |
| 5 | SMB Exploitation | Metasploit | Vulnerability exploitation |
| 6 | Splunk SIEM Setup | Splunk, Universal Forwarder | Log ingestion, SIEM architecture |

## SIEM and Detection Work

### Architecture Built
- Splunk Enterprise installed on host machine
- Splunk Universal Forwarder deployed on Windows VM
- Live Windows Security, System and Application logs forwarding via port 9997
- 1,853 real Windows events ingested and searchable

### Detection Queries Built

Successful Login Detection (EventCode 4624)
