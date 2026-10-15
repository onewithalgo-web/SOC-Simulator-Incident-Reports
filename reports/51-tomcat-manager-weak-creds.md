# Tomcat Manager Abuse: Weak Credentials to Webshell

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** SIEM | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

An Apache Tomcat server at a retail company had its Manager web application exposed with default credentials. An attacker authenticated, deployed a malicious WAR webshell, ran OS commands through it, and dropped a cryptocurrency miner.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| First successful Manager login | `185.107.0[.]44` — after a run of 401s |
| Deployed WAR | `invoker.war` |
| Webshell command that fetched the miner | `rfmine` |
| Mining pool domain | `stratum.rift-pool[.]net` |
| Initial access classification | **T1078.001** — Valid Accounts: Default Accounts |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application; T1078.001 – Default Accounts |
| Persistence | T1505.003 – Web Shell |
| Impact | T1496 – Resource Hijacking |

## Takeaway

Deploying a WAR through Manager is a supported administrative feature, so nothing in this chain is an exploit — the compromise happened at the moment default credentials were left on an internet-reachable management interface. The transition from a run of 401s to a single 200 on `/manager/html` is the highest-value line in the log, and `stratum` in a DNS query is about as unambiguous as indicators get.
