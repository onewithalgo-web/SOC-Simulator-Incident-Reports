# Edge Device Exploitation: VPN Zero-Day

**Platform:** SOC Simulator | **Tools:** SIEM, XDR, Firewall, Query | **Duration:** ~55m | **Verdict:** True Positive — 8/8 tasks

## Scenario

A breach targeting exposed edge security appliances. An attacker exploits Ivanti Connect Secure CVE-2025-22457 for unauthenticated code execution, spawns a shell from the gateway web process, retrieves a Linux payload, and attempts to preserve access using edge-device service abuse patterns similar to FortiGate SSL-VPN post-exploitation tradecraft. SIEM, XDR, and firewall evidence are correlated to identify the spawned shell, the callback domain, the local sync script, and the control that stopped the callback.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Sensor that paged the SOC | `sec-ids` |
| Code execution on the gateway | `sh` spawned from the gateway web process |
| Implant staged on the gateway | `payload.elf` |
| Implant's callback host | `vpn-update-cdn[.]invalid` |
| Control that stopped the callback | `Block-Malicious-C2` — firewall policy |
| Attacker's foothold on the gateway | `/usr/sbin/sshd -D` — rogue daemon |
| Pivot onto the LDAP server | `sync-users.sh` |
| Follow-on activity on the DevOps workstation | `Invoke-EdgeConfigAudit.ps1` |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Execution | T1059.004 – Unix Shell |
| Command and Control | T1105 – Ingress Tool Transfer |
| Persistence | T1505.003 – Web Shell; T1078.004 – Cloud Accounts; T1053.003 – Cron |
| Lateral Movement | T1021.004 – SSH |
| Exfiltration | T1041 – Exfiltration Over C2 Channel |

## Takeaway

This is the rare case where a preventive control is visible in the evidence: the C2 block policy stopped the callback, which is worth stating in the report because it is what bounded the incident. The attacker's persistence — a second `sshd` and a plausibly named sync script — is designed to read as administration, and both would pass a cursory process review. On an appliance you cannot install an agent on, the honest detection surface is what the device sends, not what it runs.
