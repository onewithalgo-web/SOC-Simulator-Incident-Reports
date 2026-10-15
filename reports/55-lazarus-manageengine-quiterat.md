# Lazarus: ManageEngine RCE to QuiteRAT Espionage

**Platform:** SOC Simulator | **Tools:** SIEM, XDR, Firewall | **Duration:** ~55m | **Verdict:** True Positive — 6/6 tasks

## Scenario

An internet-facing ManageEngine ServiceDesk Plus appliance at Caldermoor Networks, a regional ISP, fell to the unauthenticated SAML RCE (CVE-2022-47966). Code ran as the appliance service account, which curled down a QuiteRAT first-stage implant, profiled the host and domain, installed itself as an auto-start Windows service, and beaconed to a single HTTPS C2 carrying control and stolen data alike before pulling a CollectionRAT second stage. North-Korea-nexus activity.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Entry point | `45.154.91[.]18` |
| First-stage payload | `notify.exe` — QuiteRAT |
| Persistence | `AcresUpdateSvc` — auto-start Windows service |
| Command channel | `45.154.91[.]44` |
| Persistence classification | **T1543.003** — Windows Service |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Execution | T1059.001 – PowerShell |
| Command and Control | T1105 – Ingress Tool Transfer; T1071.001 – Web Protocols; T1572 – Protocol Tunneling |
| Discovery | T1082 – System Information Discovery; T1087.002 – Domain Account Discovery |
| Persistence | T1543.003 – Windows Service |

## Takeaway

Entry and command-and-control sat in the same /24, which for a state-aligned actor is a reuse pattern worth pivoting on rather than simply blocking. The target choice is the strategic point: an ISP's service-desk appliance is both a network-management system and a route into downstream customers, so the espionage value is not what is on the appliance but what the appliance can reach. A service named like an update component is the persistence artefact to hunt.
