# FortiOS Bypass to Hunters International Ransomware

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall, XDR | **Duration:** ~30m | **Verdict:** True Positive — 6/6 tasks

## Scenario

Operators consistent with the Hunters International ransomware group abused a FortiOS super-admin authentication bypass on an internet-facing FortiGate, planted rogue accounts, pivoted over the SSL-VPN tunnel by RDP, swept the network with Advanced IP and Port Scanner, and pushed roughly 6.8 GB of share data to a single external host over SFTP before running the encrypter.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Way in | `45.160.10[.]8` — FortiOS super-admin auth bypass |
| Account left behind | `svc-fortigate` — rogue administrator |
| Pivot | `NORWICK-APP-01` — reached by RDP over the SSL-VPN tunnel |
| Data out | `45.160.10[.]27` — ~6.8 GB over SFTP |
| Impact classification | **T1486** — Data Encrypted for Impact |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application; T1133 – External Remote Services |
| Persistence | T1136 – Create Account; T1078.002 – Domain Accounts |
| Lateral Movement | T1021.001 – Remote Desktop Protocol |
| Discovery | T1046 – Network Service Scanning |
| Exfiltration | T1567.002 – Exfiltration to Cloud Storage |
| Impact | T1486 – Data Encrypted for Impact |

## Takeaway

Exfiltration and encryption came from the same /24, hours apart — the operator did not bother with separate infrastructure, which means a single egress block placed at the exfiltration stage would have been worth more than any endpoint control applied at the encryption stage. The rogue FortiGate admin account is the artefact that survives a ransomware rebuild, and it is the first thing to check before restoring anything through that firewall.
