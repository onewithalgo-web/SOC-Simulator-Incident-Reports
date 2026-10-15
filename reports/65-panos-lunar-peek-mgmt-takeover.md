# Operation Lunar Peek: PAN-OS Mgmt Interface Takeover

**Platform:** SOC Simulator | **Tools:** Firewall, SIEM | **Duration:** ~1h 15m | **Verdict:** True Positive — 8/8 tasks

## Scenario

An internet-exposed PAN-OS firewall management interface was taken over in a single night. An unauthenticated attacker bypassed authentication on the management web interface to gain PAN-OS administrator privileges, chained an authenticated command injection to run commands as root, created a rogue administrator for persistence, and wrote a PHP web shell under the appliance web root — which a separate host then returned to drive interactively.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Where the firewall was reachable from | `160.187.174[.]22` |
| Authentication bypass | **T1190** — Exploit Public-Facing Application |
| Host that ran the exploit chain | `164.92.161[.]148` |
| Persistence account | `support-sync` |
| Web shell indicator | `3C5F9034C86CB1952AA5BB07B4F77CE7D8BB5CC9FE5C029A32C72ADC7E814668` |
| Operator who returned to use the web shell | `167.94.146[.]55` |
| Implant classification | **T1505.003** — Web Shell |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Persistence | T1136 – Create Account; T1505.003 – Web Shell; T1078 – Valid Accounts |
| Execution | T1059 – Command and Scripting Interpreter |

## Takeaway

Three distinct addresses across one night — exposure discovery, exploitation, and the operator returning to use the shell — is an access-broker pattern rather than a single intruder, and it changes the response: blocking the exploit source does nothing about the party who bought the access. The appliance is also the control that would normally contain an incident, so once its management plane is compromised, the firewall's own logs are no longer trustworthy evidence. Management interfaces belong on an out-of-band network, full stop.
