# OneNote Attachment to RAT: A Guided First Investigation

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** Email, XDR | **Duration:** ~15m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A contracts employee at Glacierline Freight opened an attachment and clicked a button inside it. Minutes later the workstation was running a binary on no software inventory and talking to a host no business process uses. The investigation walks the mail gateway records, the file artifacts, and the endpoint process tree in order.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Delivery sender | `billing@mrassociattes[.]com` — typosquatted supplier domain |
| File opened | `Contract_02_21_Copy#909.one` — OneNote container |
| Execution chain from the click | `cmd.exe` spawned from the OneNote embedded object |
| Downloaded loader | `COIm.jpg` — a PE masquerading as an image |
| Disguise classification | **T1027** — Obfuscated Files or Information |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.001 – Spearphishing Attachment |
| Execution | T1204.002 – User Execution: Malicious File; T1059.001 – PowerShell |
| Defense Evasion | T1027 – Obfuscated Files or Information |

## Takeaway

The `.one` container was the whole trick: it carries embedded executable objects but is not a macro-enabled Office document, so the macro-blocking controls most organizations deployed after 2022 did not apply to it. A `.jpg` extension on a file that the process tree shows being executed is the cheapest high-fidelity detection in the chain.
