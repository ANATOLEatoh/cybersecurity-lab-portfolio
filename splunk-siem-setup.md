# Splunk SIEM Setup

## Objective
Set up a working SIEM environment to collect and 
analyse Windows security logs in real time.

## Architecture
- Splunk Enterprise installed on host machine
- Splunk Universal Forwarder installed on Windows VM
- Logs forwarded to host via port 9997
- VirtualBox Host-Only Network: 192.168.56.1

## Configuration Steps
1. Enabled receiving port 9997 in Splunk settings
2. Installed Universal Forwarder on Windows VM
3. Created inputs.conf to forward Security, 
   System and Application logs
4. Added firewall rule to allow port 9997

 Evidence of Success
- 1,853 Windows events ingested into Splunk
- Security, System and Application logs confirmed flowing
- Host: DESKTOP-J6U95L2 visible in Splunk

Searches Run

 Successful Logins (EventCode 4624)
index=* source="WinEventLog:Security" EventCode=4624
Result: 11 successful login events detected

 Failed Logins (EventCode 4625)
index=* source="WinEventLog:Security" EventCode=4625
Result: 3 failed login attempts detected

 Brute Force Detection Query
index=* source="WinEventLog:Security" EventCode=4625
| stats count by Account_Name, host
| where count > 2

 Key Takeaways
- A SIEM collects logs from endpoints via forwarders
- EventCode 4624 = successful login
- EventCode 4625 = failed login  
- Multiple 4625s from same account = 
  brute force indicator
- This detection logic is used in real SOC environments
```

---

## Step 3 — Commit the File

Scroll down to the bottom of the page. You'll see a **"Commit changes"** box.

In the first box type:
```
Add Splunk SIEM setup documentation
```
Leave everything else as default and click the green **"Commit changes"** button.

---

## Step 4 — Add Your Screenshots

Now go back to your repository main page. Click **"Add file"** then select **"Upload files"**.

Drag and drop all the screenshots you took tonight — the 1,853 events screen, the 4624 results, the 4625 results, and the detection query result.

In the commit message type:
```
Add Splunk lab screenshots
