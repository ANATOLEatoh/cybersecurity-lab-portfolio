# Network Configuration Lab

## Objective
Enable communication between Kali and Windows VMs.

Problem
- Kali IP: 10.0.2.15 (NAT)
- No communication with Windows

Solution
- Added Host-Only Adapter
- Assigned IP range: 192.168.56.x

Commands Used
ip a
ping 192.168.56.101

 Result
- Successful ping between machines

 Key Learning
- Difference between NAT and Host-Only
- How VM networking works

 Screenshots
(Add IP configs + ping success)
