# PowerShell Download Cradle: Encoded Payload on a Workstation

**Platform:** SOC Simulator | **Tools:** XDR, SIEM | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A finance workstation ran an obfuscated PowerShell command containing a base64-encoded download cradle that fetched and executed a second-stage payload, which then beaconed out to a C2 host. The investigation works the Sysmon and Windows Security event logs alongside the XDR process tree to trace the encoded launch, the staging fetch, the dropped file, and the recurring beacon.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Suspicious PowerShell launch | `-EncodedCommand` |
| Parent process | `WINWORD.EXE` |
| Staging server | `185.157.0[.]58` |
| Dropped payload | `svc.exe` |
| C2 beacon destination | `updates.cdn-svc-net[.]com` |
| Obfuscation classification | **T1027** — Obfuscated Files or Information |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Execution | T1059.001 – PowerShell |
| Defense Evasion | T1027 – Obfuscated Files or Information |
| Command and Control | T1105 – Ingress Tool Transfer; T1071.001 – Web Protocols |

## Takeaway

`WINWORD.EXE` as the parent of `powershell.exe` is the single fact that resolves this case; the encoding is a detail. Office applications have no legitimate reason to spawn a shell, and that relationship is available in Sysmon event 1 without any decoding at all. Script-block logging makes the payload readable after the fact, but the process-ancestry rule is what fires in time to matter.
