# Windows Logging SOC Investigation Report

## Objective
The purpose of this project is to investigate Windows security activity using Windows Event Logs and Sysmon logs from a SOC analyst perspective.

The investigation focuses on identifying suspicious authentication behavior, brute-force activity, successful logins, suspicious process execution, malware download activity, network connections, DNS queries, and PowerShell history artifacts.

## Lab Environment
- Platform: TryHackMe
- Room: Windows Logging for SOC
- Hostname: THM-PC
- Log Sources:
  - Practice-Security.evtx
  - Practice-Sysmon.evtx
  - PowerShell history files

## Investigation Areas

### 1. Windows Security Log Analysis
The Windows Security logs were reviewed to identify login and account activity.

Important Event IDs:
- `4624` — Successful logon
- `4625` — Failed logon
- `4634` — Logoff
- `4672` — Special privileges assigned
- `4732` — User added to a local group

These logs help SOC analysts identify brute-force attempts, successful account compromise, privilege escalation, and suspicious group membership changes.

### 2. Brute-Force Login Investigation
Multiple failed login attempts were reviewed using Event ID `4625`.

Investigation logic:
- Repeated failed logins from the same source may indicate brute-force activity.
- A successful login after repeated failed logins may indicate account compromise.
- The source network address, username, and logon type are important fields during investigation.

### 3. RDP Login Review
Successful login events were reviewed using Event ID `4624`.

A Logon Type of `10` indicates an RDP login.

Important fields reviewed:
- Account Name
- Source Network Address
- Logon Type
- Logon ID

### 4. Group Membership Investigation
Event ID `4732` was reviewed to identify users added to local groups.

This is important because attackers may add a backdoor user to privileged groups for persistence and future access.

Important fields reviewed:
- Member Name / Member SID
- Target User Name
- Target Domain Name
- Subject User Name

### 5. Sysmon Process Creation Analysis
Sysmon Event ID `1` was reviewed to analyze process creation activity.

Important fields reviewed:
- Image
- CommandLine
- ParentImage
- ParentCommandLine
- User
- Hashes

A suspicious executable was observed in the user's Downloads folder.

Suspicious file:
- `ckjg.exe`

File path:
- `C:\Users\sarah.miller\Downloads\ckjg.exe`

### 6. File Download Investigation
Sysmon Event ID `15` was reviewed to identify file stream metadata and download source information.

The downloaded file was linked to a Zone.Identifier stream, which helped identify the source URL.

Downloaded file:
- `ckjg.exe`

Download URL:
- `hxxp://gettsveriff[.]com/bgj3/ckjg.exe`

### 7. Network Connection Analysis
Sysmon Event ID `3` was reviewed to identify outbound network connections from suspicious processes.

Important fields reviewed:
- Image
- Destination IP
- Destination Port
- Destination Hostname

This helped identify possible Command and Control communication.

### 8. DNS Query Analysis
Sysmon Event ID `22` was reviewed to map malicious IP activity to domain activity.

Important fields reviewed:
- QueryName
- QueryResults
- Image
- User

### 9. PowerShell History Review
PowerShell history files were reviewed to identify commands executed by users.

PowerShell history is useful because it can show previously executed commands even if command output is not logged.

Important artifact:
- `ConsoleHost_history.txt`

## Indicators of Compromise

| Type | Value |
|---|---|
| Suspicious File | ckjg.exe |
| Suspicious File Path | C:\Users\sarah.miller\Downloads\ckjg.exe |
| Download URL | hxxp://gettsveriff[.]com/bgj3/ckjg.exe |
| User Account | THM-PC\sarah.miller |
| Hostname | THM-PC |

## Analysis Summary
The investigation showed suspicious activity involving authentication events, Sysmon process execution, file download metadata, network communication, DNS activity, and PowerShell history artifacts.

The suspicious executable `ckjg.exe` was observed in the user's Downloads folder. Sysmon logs helped identify the process activity, the downloaded file source, and related network behavior.

Windows Security logs were useful for reviewing login activity, failed logins, successful logins, RDP logon behavior, and group membership changes.

## Final Verdict
Verdict: Suspicious / Malicious Activity Identified

## Recommended Actions
1. Isolate the affected host from the network.
2. Block the suspicious URL and related domain.
3. Block any malicious IP addresses identified during investigation.
4. Review all successful logins from suspicious source IP addresses.
5. Reset credentials for any breached users.
6. Review privileged group membership changes.
7. Remove suspicious files from the host.
8. Investigate PowerShell history for additional attacker activity.
9. Search SIEM logs for similar activity across other systems.
10. Improve monitoring for Event IDs 4624, 4625, 4732, Sysmon 1, Sysmon 3, Sysmon 15, and Sysmon 22.

## Skills Demonstrated
- Windows Event Log analysis
- Sysmon event investigation
- Brute-force detection
- RDP login analysis
- Suspicious process investigation
- IOC extraction
- PowerShell history review
- SOC-style incident reporting
