# Cleo Harmony MFT: Cl0p Data Theft at Vantex Logistics

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~50m | **Verdict:** True Positive — 7/7 tasks

## Scenario

A Cleo Harmony managed file transfer server in the Vantex Logistics DMZ started reaching external systems it does not normally contact and running processes the change record does not explain. The case is reconstructed from the Cleo web logs, the Windows event logs, the resolver, and the perimeter firewall.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Entry point | `/Cleo/Harmony/autorun` — unauthenticated file-write endpoint |
| Initial exploit source | `45.116.0[.]91` |
| What was planted | `maintenance.xml` — autorun job definition |
| Loader origin | `cdn-updates.vantex-sys[.]net` — lookalike of the victim's own domain |
| Exfiltration destination | `45.116.0[.]117` |
| Persistence classification | **T1078** — Valid Accounts |
| Initial access classification | **T1190** — Exploit Public-Facing Application |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Persistence | T1505.003 – Web Shell; T1078 – Valid Accounts |
| Command and Control | T1071.001 – Web Protocols |
| Exfiltration | T1567 – Exfiltration Over Web Service; T1041 – Over C2 Channel |
| Defense Evasion | T1070 – Indicator Removal |

## Takeaway

The autorun directory is a product feature — Cleo executes what it finds there — so the exploit is really an unauthenticated write to a directory that is designed to run things. The loader domain was built to look like the victim's own infrastructure, which defeats a quick eyeball check of outbound DNS. Cl0p's pattern is theft rather than encryption, so the clock to measure is time-to-exfiltration, and the control that matters is egress restriction from the DMZ, not endpoint protection on the appliance.
