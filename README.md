# windows-logging-soc-investigation
# Windows Logging SOC Investigation

A beginner SOC analyst project focused on Windows Event Log analysis, Sysmon investigation, brute-force detection, suspicious process analysis, PowerShell history review, and incident reporting.

## Project Overview
This project documents a Windows log investigation from a SOC analyst perspective. The investigation involved analyzing Windows Security logs, Sysmon logs, suspicious authentication activity, process creation events, network connections, DNS activity, and PowerShell history.

## Skills Demonstrated
- Windows Event Log analysis
- Sysmon log analysis
- Brute-force login investigation
- Suspicious process investigation
- PowerShell history review
- IOC extraction
- Incident reporting

## Key Event IDs Reviewed
- `4624` — Successful logon
- `4625` — Failed logon
- `4634` — Logoff
- `4672` — Special privileges assigned
- `4732` — User added to local group
- `4688` — New process created
- `Sysmon Event ID 1` — Process creation
- `Sysmon Event ID 3` — Network connection
- `Sysmon Event ID 15` — File stream created / downloaded file metadata
- `Sysmon Event ID 22` — DNS query

## Files Included
- `report.md` — Full SOC investigation report
- `iocs.txt` — Extracted indicators of compromise
- `screenshots/` — Supporting screenshots from the lab

## Final Verdict
Suspicious activity was identified, including brute-force login behavior, suspicious downloaded executable activity, network communication, and PowerShell history artifacts.

## Note
This project was completed in a safe lab environment for learning and portfolio purposes.
