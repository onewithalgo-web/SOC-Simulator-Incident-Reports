# Overnight on the DMZ Wiki

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~50m | **Verdict:** True Positive — 6/6 tasks

## Scenario

Quillon Software publishes its engineering wiki straight to the internet from the DMZ. Overnight the host picked up work nobody rostered: requests that never authenticated, shell activity under a service account, and outbound sessions from a server that normally only talks to its update mirrors.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Foothold | `103.97.176[.]44` — unauthenticated OGNL injection |
| Running identity | `confluence` — the application service account |
| Planted webshell | `noop.jsp` — China Chopper variant |
| Credential theft | `confluence-db-creds.txt` |
| Exfiltration | `156.244.7[.]190` |
| Access classification | **T1190** — Exploit Public-Facing Application |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Execution | T1059.004 – Unix Shell |
| Persistence | T1505.003 – Web Shell |
| Defense Evasion | T1620 – Reflective Code Loading; T1140 – Deobfuscate/Decode Files; T1070 – Indicator Removal |
| Credential Access | T1003 – OS Credential Dumping |
| Command and Control | T1071.001 – Web Protocols |

## Takeaway

China Chopper's whole design goal is a shell small enough to hide in a legitimate-looking file, with the command carried in the POST body rather than the URL — so access logs show an ordinary request to an ordinary-looking JSP. That pushes detection onto the process tree instead: a web application service account spawning a shell is categorically wrong, whatever the request looked like. An engineering wiki is also a credential repository in practice, which is the part of the loss that outlasts the cleanup.
