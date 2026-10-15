# PAN-OS GlobalProtect: Command Injection (CVE-2024-3400)

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~45m | **Verdict:** True Positive — 5/6 tasks

## Scenario

An internet-facing Palo Alto Networks firewall published its GlobalProtect VPN portal to the world, and an unauthenticated command-injection flaw turned a single crafted request into code execution on the appliance. The attacker validated the bug with a zero-byte canary, stole the firewall's running configuration, dropped a Python implant that takes commands from the device's own error log, and pivoted off the box into the domain controller.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Way in | `/ssl-vpn/hipreport.esp` |
| Dry run | `185.113.0[.]47` — zero-byte canary to confirm the bug |
| Execution | `185.113.0[.]91` |
| What was stolen | `running-config.xml` |
| Command channel | `update.fw-telemetry-sync[.]net` |
| Pivot classification | **T1210** — Exploitation of Remote Services |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application; T1133 – External Remote Services |
| Execution | T1059.004 – Unix Shell |
| Command and Control | T1071.001 – Web Protocols |
| Exfiltration | T1041 – Exfiltration Over C2 Channel |
| Lateral Movement | T1210 – Exploitation of Remote Services |

## Takeaway

The implant reading its commands out of the appliance's own error log is the detail worth keeping: there is no inbound C2 session to spot, because the operator writes tasking by making the device log something. That inverts the usual egress-monitoring assumption. The canary request from a separate address also shows a deliberate two-stage approach — confirm the vulnerability cheaply from one host, exploit from another — so correlating low-and-slow probes with later activity matters more than scoring any single request.
