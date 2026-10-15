# The Backdoored Browser Extension: Following the C2 Beacon

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** SIEM, Firewall | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A routine Chrome auto-update silently replaced a legitimate productivity extension on a finance workstation at Halverson Logistics with a trojanized build. The new version carried two extra scripts: one to fetch a configuration file from attacker infrastructure, one to read session cookies and authentication tokens straight out of the browser profile. Nothing was written to disk and everything rode ordinary outbound HTTPS, so the web proxy and firewall logs were the only trail.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Domain the extension phoned home to | `graphqlnetwork[.]pro` |
| Where the stolen cookies were sent | `149.248.2[.]160` — VULTR-hosted drop server |
| Malicious background script | `b0827dc54349b10098a7370ada4ea44ba668b264ccca2db5676be1c32e6cc154` |
| Browser storage key hiding the config | `graphqlnetwork_ext_manage` |
| Cookie theft classification | **T1539** — Steal Web Session Cookie |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Persistence | T1176 – Browser Extensions |
| Command and Control | T1071.001 – Application Layer Protocol: Web Protocols |
| Credential Access | T1539 – Steal Web Session Cookie; T1528 – Steal Application Access Token |

## Takeaway

A compromise with no file to quarantine and no process to kill. The extension ran inside the browser's own trust boundary, so EDR had nothing to flag and the egress looked like ordinary HTTPS. The only durable detection surface was the pair of destinations the browser suddenly started contacting — which is an argument for baselining extension network behaviour, not just extension permissions at install time.
