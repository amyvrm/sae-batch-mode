639bd61d-8aee-4538-bc37-c630dd63d80f / System-Defined Event Logs Clearing via Wevtutil / medium

## Description
###### Why This Model is Created?

This Model is based on Petya Ransomware Behavior. The malware has the ability to clear Windows Event Logs using wevtutil.
```
wevtutil cl Setup & wevtutil cl System & wevtutil cl Security & wevtutil cl Application & fsutil usn deletejournal /D
```

## Design
###### What's The Logic of This Model?

Wevtutil.exe execution targetting those System-Defined Event Logs (application or security or system) at least twice.  
We only want pure clearing event, so RegEx was used to make sure use we won't catch clearing the logs plus doing a backup (/bu argument).  
Medium riskLevel was chosen because this behavior may be normal, like when doing maintainance or installing a new application.  
**Potential Issue:** Model matching occurrence seems to be limited into 1 filter hit per minute. Might need to split the filter into 3 (each targetting a specific event log). Then the Rule/Model can specifically detect if there are 2 unique hits.


## Testing
###### How To Trigger?

Execute the following commands
```
wevtutil cl application
wevtutil cl security
wevtutil cl system
```

## Investigation and Possible Actions
###### What Should Customer/SOC Do With This Model? Can we add the logic to the model so SOC won't need to manually find it?

1. locate the real parent
- **processCmd** likely contains path of the batch file clearing the logs. If the batch file is expected, then no need to continue investigation. Maybe add it to whitelist.
- The batch file can be downloaded for further checks. Sweep the enviroment for this file.
- If **parentCmd** is task scheduler (taskeng.exe or svchost with -s schedule arguments), SOC needs to find out the process that created it. (winEventId: 100)
- If the processCmd/parentCmd are only interpreters (cmd.exe, PowerShell.exe, wscript, etc..), use **RCA** to find the parent node
- If a suspicious parent was found, **terminate** it, list down everything that it did and sweep the environment for this file.
- If no suspicious parent found (like process and parent were cmd.exe or PowerShell.exe, and grandparent is explorer.exe), then the user account may be compromised. if user comfirm it was not his/her action, **reset the password**. 

