# Raspberry Robin: USB Worm to LOLBin Execution

**Platform:** SOC Simulator | **Tools:** XDR, SIEM | **Verdict:** True Positive - 6/6 tasks | **Accuracy:** 91.67%

## Scenario

A staff member at Vantekra Solutions inserted an unknown USB drive. A shortcut (LNK) file on it silently handed execution to `cmd.exe`, which called `msiexec` to fetch a DLL from a compromised host on the internet. `rundll32` loaded the DLL, `fodhelper` provided silent elevation, and a Tor-backed command-and-control channel opened within minutes. Every step used a signed Windows binary. The chain had to be traced through the XDR process tree and Sysmon logs.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Infection source on the USB | `WorkDocs_2026.lnk` |
| External staging host | `185.137.0[.]47` |
| DLL-loading step | **T1218.011** - System Binary Proxy Execution: Rundll32 |
| UAC bypass | **T1548.002** - Abuse Elevation Control Mechanism: Bypass User Account Control |
| C2 destination | `185.137.0[.]91` |
| Worm propagation | **T1091** - Replication Through Removable Media |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access / Lateral Movement | T1091 - Replication Through Removable Media |
| Defense Evasion | T1218.011 - System Binary Proxy Execution: Rundll32 |
| Privilege Escalation / Defense Evasion | T1548.002 - Bypass User Account Control |

## Takeaway

Nothing in this chain is a file that antivirus would flag on name alone: a shortcut, `cmd.exe`, `msiexec`, `rundll32` and `fodhelper` are all signed and expected on a Windows machine. The detection comes from the sequence and the parent-child relationships in the process tree, starting from a shortcut on removable media and ending at an elevated process talking to the internet. It also shows why removable media is still worth controlling, since the infection depended on a person plugging something in.
