# Ivanti Connect Secure: Auth Bypass to Webshell

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall, XDR, Query | **Duration:** ~55m | **Verdict:** True Positive — 7/7 tasks

## Scenario

An internet-facing Ivanti Connect Secure VPN appliance was exploited through a pair of chained zero-days: an authentication bypass and a command injection. A webshell was resident on the appliance, credentials had been stolen from the system configuration, and the attacker had pivoted inward.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Entry point | `45.112.0[.]44` |
| Code execution on the appliance | confirmed from the same source, `45.112.0[.]44` |
| Planted foothold | `/data/runtime/mtmp/lmx6.py` |
| What was stolen | `/data/runtime/etc/system.conf` — appliance credential store |
| Outbound beacon | `update.global-cdn-node[.]com` |
| Pivot into the internal network | `j.reinhold` |
| Initial access classification | **T1190** — Exploit Public-Facing Application |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application; T1133 – External Remote Services |
| Persistence | T1505.003 – Web Shell; T1078 – Valid Accounts |
| Execution | T1059.004 – Unix Shell |
| Credential Access | T1552.001 – Credentials in Files |
| Command and Control | T1071.001 – Web Protocols |

## Takeaway

The appliance is the whole problem: it terminates VPN sessions, so it holds credentials for the people who connect through it, and it sits outside most organizations' endpoint tooling. Exploitation and hands-on activity came from a single address here, which is unusual and useful — one block would have cut both. The pivot to a real user account is the point at which appliance containment stops being sufficient and the response becomes an identity response.
