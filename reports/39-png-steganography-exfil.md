# Hidden in the Pixels: LSB Steganography Exfil From an Infected Endpoint

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall, XDR, Query | **Duration:** ~30m | **Verdict:** True Positive — 7/7 tasks

## Scenario

A LummaC2/Vidar-style infostealer smash-and-grab. A finance analyst was tricked by a fake CAPTCHA (ClickFix) into pasting an `mshta` command into the Run box, which staged hidden PowerShell, downloaded the Lumma stealer, and harvested browser credentials, cookies, and wallet artifacts. To smuggle the loot out, the operator LSB-encoded the stolen blob into an oversized PNG and uploaded it to a public image host — off the command-and-control channel entirely.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Initial execution vector | **T1218.005** — Mshta |
| Delivery host | `check.foquh[.]icu` |
| Stealer payload hash | `3508d299c44d2bfb9b875b5bb395e269632cea7ea44fe7ab96357d22fd37ad48` |
| Data-concealment technique | **T1027.003** — Steganography |
| Carrier file | `brand-assets-q1.png` |
| Exfiltration separate from the command channel | `103.75.182[.]108` |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Spearphishing Link |
| Execution | T1204.001 – Malicious Link; T1059.001 – PowerShell; T1218.005 – Mshta |
| Defense Evasion | T1027.003 – Steganography; T1140 – Deobfuscate/Decode Files |
| Credential Access | T1555.003 – Credentials from Web Browsers |
| Command and Control | T1071.001 – Web Protocols; T1105 – Ingress Tool Transfer |
| Exfiltration | T1567 – Exfiltration Over Web Service |

## Takeaway

Splitting exfiltration from command-and-control defeats the usual correlation: the C2 host carries almost no data, and the host that carries the data is a public image service with a clean reputation. DLP inspecting file type sees a valid PNG. The one honest signal left is size — a brand-asset PNG an order of magnitude larger than anything else the marketing pipeline produces, uploaded by a finance workstation.
