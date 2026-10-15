# Removable Media: USB Autorun Malware on a Workstation

**Platform:** SOC Simulator | **Tools:** XDR, SIEM, Query | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A USB drive plugged into a Kaldera Systems workstation auto-ran a malicious shortcut that executed a VBScript dropper, planted a persistence Run key, and began beaconing to an external C2 host. Single-host endpoint triage using the XDR process tree and SIEM Sysmon logs.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Infected workstation | `KLD-WKS-14` |
| User who executed the file | `KALDERA\priya.nair` |
| Persistence mechanism | `HKCU\Software\Microsoft\Windows\CurrentVersion\Run\WindowsUpdate` |
| Dropped payload | `svchst.exe` — typosquat of `svchost.exe` |
| C2 domain | `cdn.kreon-updates[.]net` |
| Persistence classification | **T1547.001** — Registry Run Keys |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1091 – Replication Through Removable Media |
| Execution | T1204.002 – User Execution: Malicious File |
| Persistence | T1547.001 – Registry Run Keys |
| Command and Control | T1105 – Ingress Tool Transfer |
| Defense Evasion | T1036.004 – Masquerade Task or Service |

## Takeaway

Two artefacts carry this case on their own: a Run value named `WindowsUpdate` (Windows Update does not use one) and a binary named `svchst.exe` one character from a real system process. Both are cheap, high-confidence detections that need no reputation lookup. Removable-media policy is the preventive control, but the single-character masquerade is worth a standing rule regardless of how the file arrives.
