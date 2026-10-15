# DarkGate via Microsoft Teams: External Message Lure

**Platform:** SOC Simulator | **Tools:** XDR, SIEM, Email | **Duration:** ~50m | **Verdict:** True Positive — 7/7 tasks

## Scenario

An IT helpdesk coordinator at a logistics firm received an urgent Teams chat from an external "IT support" account. The message linked to a VBS script disguised as a VPN update. Running it kicked off a silent MSI install that dropped a renamed AutoIt3 loader, which decrypted DarkGate in memory, registered a scheduled task for persistence, and began beaconing to an HTTPS C2 server.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| First contact | `it-updates@contoso-helpdesk[.]onmicrosoft[.]com` — external tenant |
| Lure file | `VPN_Config_Update_2026.vbs` |
| Staging server | `45.136.0[.]83` |
| Loader binary | `Au3Help.exe` — renamed AutoIt3 interpreter |
| Persistence | `\WindowsPatchSvc` — scheduled task |
| C2 channel | `darkgate-relay[.]net` |
| Installer-evasion classification | **T1218.007** — Msiexec |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566 – Phishing (Teams external chat) |
| Execution | T1204.002 – User Execution; T1059.005 – Visual Basic |
| Defense Evasion | T1218.007 – Msiexec |
| Persistence | T1053.005 – Scheduled Task |
| Command and Control | T1071.001 – Web Protocols |

## Takeaway

Teams external access is the gap here: the message bypassed every mail-gateway control because it was never email, and the sender's `onmicrosoft.com` tenant lends it a Microsoft-looking address. Disabling external chat from unmanaged tenants removes the delivery path entirely. On the endpoint, an AutoIt interpreter renamed to look like a help binary is the strongest single artefact — legitimate AutoIt deployments do not rename the interpreter.
