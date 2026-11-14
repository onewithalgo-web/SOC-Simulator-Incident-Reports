# Scheduled Task Persistence: The License Tool That Phoned Home Every 15 Minutes

**Platform:** SOC Simulator | **Tools:** SIEM, XDR | **Verdict:** True Positive - 5/5 tasks

## Scenario

A dropper dressed up as a software license-activation tool ran on a financial-sector workstation. It registered a Windows Scheduled Task that re-ran a PowerShell stager every 15 minutes, and each run beaconed over HTTPS to an attacker-controlled domain. The evidence was in the event log, the XDR process tree and the proxy records.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Persistence footprint | Scheduled task named `WindowsUpdateChecker` (an update-sounding name chosen to blend in) |
| Parent of the persistence | `NexVault_LicTool_v2.exe`, the fake license tool |
| Stager on disk | `C:\ProgramData\NexVault\winupdcheck.ps1` |
| C2 endpoint | `cdn.updates-nexcore[.]live` |
| Persistence technique | **T1053.005** - Scheduled Task/Job: Scheduled Task |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Persistence / Execution | T1053.005 - Scheduled Task/Job: Scheduled Task |

## Takeaway

Everything about this intrusion was designed to look like maintenance: a task called `WindowsUpdateChecker`, a script under `ProgramData` and a domain that reads like a CDN. The fast way to the truth was to start from the scheduled task and walk back to its parent process rather than judging the name. A recurring beacon like this is also easy to find in the proxy records once you know the interval to look for.
