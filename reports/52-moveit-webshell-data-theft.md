# Transfer Denied: The MOVEit Web Shell

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~30m | **Verdict:** True Positive — 6/6 tasks

## Scenario

An internet-facing MOVEit Transfer server was breached through an unauthenticated SQL injection. A stealthy web shell, gated by a secret HTTP header, enumerated and stole stored files and planted a rogue account. The whole case is reconstructed from web request logs.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Foothold | `5.252.189[.]41` |
| Implant | `human2.aspx` |
| Implant's secret | `X-siLock-Comment` — header required to activate the shell |
| Backdoor account | `Health Check Service` |
| Theft destination | `209.127.18[.]66` |
| Access classification | **T1190** — Exploit Public-Facing Application |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Persistence | T1505.003 – Web Shell; T1078 – Valid Accounts |
| Collection | T1213 – Data from Information Repositories |
| Exfiltration | T1041 – Exfiltration Over C2 Channel |
| Defense Evasion | T1070 – Indicator Removal |

## Takeaway

The header gate is what makes this shell survive casual inspection: without `X-siLock-Comment` the page returns a benign response, so a scanner or an analyst browsing the URL sees nothing. That moves the detection from content to request metadata — an uncommon custom header arriving at an MFT endpoint. The rogue account named like a monitoring service is the second-order persistence and the thing most likely to be missed during cleanup.
