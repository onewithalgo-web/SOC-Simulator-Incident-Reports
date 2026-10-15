# Remcos RAT: Malicious Invoice Attachment

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** Email, XDR | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

An accounts-payable employee at Harwell Logistics opened what looked like an overdue supplier invoice. Minutes later the workstation was holding a persistent outbound session to an external address on a port nothing else uses. The walkthrough works the mail gateway records, the endpoint file artifacts, and the process tree in order, pulling one indicator from each surface.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Delivery sender | `billing@invoice-remittance-sys[.]net` |
| File executed | `Invoice_HWL-7741_Oct2025.vbs` |
| Execution chain | `wscript.exe` |
| Implant installed | `winhost32.exe` — masquerading as a system binary |
| Execution classification | **T1059.005** — Visual Basic |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.001 – Spearphishing Attachment |
| Execution | T1204.002 – User Execution: Malicious File; T1059.005 – Visual Basic |
| Persistence | T1547.001 – Registry Run Keys / Startup Folder |
| Command and Control | T1219 – Remote Access Software |

## Takeaway

Three signals here are independently weak and jointly conclusive: a lookalike billing domain, `wscript.exe` launched from a mail client's temp path, and a binary named like a Windows component sitting outside `System32`. Remcos is commodity tooling, which makes the non-standard C2 port the most practical single detection — legitimate software rarely holds a long-lived session on an arbitrary high port.
