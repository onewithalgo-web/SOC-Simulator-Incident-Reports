# GootLoader: SEO Poisoning to Domain Control

**Platform:** SOC Simulator | **Tools:** SIEM, XDR, Firewall | **Duration:** ~1h | **Verdict:** True Positive — 7/7 tasks

## Scenario

A paralegal searching for a contract template clicked a poisoned search result, and a quiet endpoint script alert turned into a domain controller compromise by nightfall. The chain runs from a drive-by ZIP through an obfuscated JavaScript loader, scheduled-task and Run-key persistence, a Cobalt Strike beacon, and a SystemBC proxy to the domain controller.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Poisoned download source | `forum.harborlinepta[.]org` — compromised legitimate forum |
| Script execution | `wscript.exe` |
| Beacon channel | `cdn.bridge-metrics-sync[.]com` |
| Relay channel | `45.166.0[.]20` — SystemBC proxy |
| Domain-control logon | `adm-kowalczyk` |
| Persistence classification | **T1547.001** — Registry Run Keys |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566 – Phishing; T1204.002 – User Execution |
| Execution | T1059.007 – JavaScript; T1059.001 – PowerShell |
| Defense Evasion | T1140 – Deobfuscate/Decode Files |
| Persistence | T1547.001 – Registry Run Keys; T1053.005 – Scheduled Task |
| Credential Access / Lateral Movement | T1550.002 – Pass the Hash; T1021.001 – RDP; T1570 – Lateral Tool Transfer |
| Discovery | T1087.002 – Domain Account Discovery; T1018 – Remote System Discovery |

## Takeaway

GootLoader's delivery is patient infrastructure work — compromised legitimate sites ranked for narrow professional search terms, so the victim arrives by their own intent and the domain carries real reputation. The escalation from "a script ran" to "the domain controller is compromised" happened inside one working day, which is the argument for treating script-engine alerts on professional-services endpoints as urgent rather than informational.
