Cybersecurity Lab Portfolio

This repository documents my hands-on cybersecurity training using Kali Linux and Windows virtual machines.

Lab Environment
- Kali Linux attacker machine
- Windows 10 target machine
- VirtualBox (Host-Only Network)

Labs Completed

1. Lab Setup
Set up virtual machines and configured environment.

 2. Network Configuration
Resolved IP issues and established communication between machines.

3. Nmap Scanning
Performed service and port discovery.

 4. SMB Enumeration
Enumerated SMB services and tested access.

 5. SMB Exploitation (In Progress)
Attempting exploitation of SMB vulnerabilities.

 Skills Demonstrated
- Network troubleshooting
- Port scanning (nmap)
- Service enumeration
- SMB interaction
- Documentation and reporting

 Tools Used
- nmap
- smbclient
- enum4linux
- Kali Linux

 SMB Enumeration Lab

Objective
To identify and interact with SMB services on a Windows machine.

Target
192.168.56.101

Tools Used
- nmap
- smbclient
- enum4linux

 Methodology

 Step 1: Port Scanning
nmap -sV 192.168.56.101

 Step 2: SMB Enumeration
enum4linux -a 192.168.56.101

 Step 3: SMB Login Attempt
smbclient -L //192.168.56.101/

 Results
- Ports 135, 139, 445 open
- SMB service detected
- Authentication required

 Errors Observed
- NT_STATUS_ACCESS_DENIED
- NT_STATUS_LOGON_FAILURE

 Key Learning
- SMB often requires authentication
- Enumeration does not guarantee access
- Windows systems are typically hardened by default

 Screenshots
Nmap Scan (screenshots/nmap.png)
SMB Error (screenshots/error.png)
 
 Author
Cybersecurity student actively preparing for placement roles.
