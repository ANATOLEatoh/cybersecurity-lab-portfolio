Commands Cheat Sheet

 Nmap
-nmap -sV <ip>
-nmap -sS <ip>

SMB
-smbclient -L //<ip>/
-enum4linux -a <ip>

nmap -sV 192.168.56.101
smbclient -L //192.168.56.101 -U vboxuser
crackmapexec smb 192.168.56.101 -u vboxuser -p windows
crackmapexec smb 192.168.56.101 --rid-brute
smbclient //192.168.56.101/C$ -U vboxuser
