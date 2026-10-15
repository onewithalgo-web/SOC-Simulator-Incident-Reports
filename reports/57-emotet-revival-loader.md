# Emotet Revival: Triage the November Loader

**Platform:** SOC Simulator | **Tools:** SIEM, Email, Firewall | **Duration:** ~1h | **Verdict:** True Positive — 7/7 tasks

## Scenario

An operations coordinator opened a thread-hijacked remittance spreadsheet and enabled its macro. A trusted Windows utility quietly fetched a loader from a compromised website, and the workstation began beaconing to addresses nobody recognized. The case traces the rebuilt November Emotet loader from a hijacked mail thread through a `regsvr32` download-and-register, an encrypted epoch C2 pool, and a Cobalt Strike second stage.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Delivery | `accounts@storfeld-paper-supply[.]com` — thread hijack from a real correspondent |
| Macro handoff | `regsvr32.exe` |
| Loader source | `https://storfeld-paper-supply[.]com/wp-content/uploads/2026/05/J90563.dll` |
| Primary beacon | `45.168.118[.]27:443` |
| Second stage | `strand-finance-update[.]com` |
| Evasion classification | **T1218.010** — Regsvr32 |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.001 – Spearphishing Attachment |
| Execution | T1204.002 – User Execution; T1059.005 – Visual Basic; T1059.001 – PowerShell |
| Defense Evasion | T1218.010 – Regsvr32; T1218.011 – Rundll32 |
| Command and Control | T1071.001 – Web Protocols; T1573.001 – Symmetric Cryptography; T1571 – Non-Standard Port |

## Takeaway

Thread hijacking is what makes Emotet's delivery hard to train against: the message is a genuine reply in a real conversation, from a correspondent the recipient has emailed before, and the sending domain is the correspondent's own — because they were compromised first. The loader was hosted in that same domain's WordPress uploads directory, so URL reputation also passed. What remains is the endpoint behaviour: Office spawning `regsvr32` to register a DLL fetched over HTTP.
