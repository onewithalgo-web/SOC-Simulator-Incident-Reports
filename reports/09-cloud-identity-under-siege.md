# Cloud Identity Under Siege

**Platform:** SOC Simulator | **Tools:** SIEM, XDR, Firewall, Cloud audit | **Duration:** ~1h | **Verdict:** True Positive — 8/8 tasks, 48.71% accuracy

## Scenario

Nine days of telemetry from a cloud-first Azure estate: Entra ID sign-ins, Windows endpoints, a Kubernetes cluster, an Azure Function, and the storage accounts behind them all report into one SIEM. No pre-seeded alert — the exercise requires establishing a baseline of normal activity first, then finding what didn't fit it, across SIEM, XDR, firewall and cloud panels together.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Baseline signal | Windows Event ID **4624** established as the reliable signal for a clean, successful interactive logon |
| Unexpected inbound connection to app server | `10.0.1.134` |
| Internal origin of the break-glass sign-in | `10.0.2.69` — an emergency admin account used from an unexpected internal host |
| Client used to reach the Kubernetes API server | `kubectl.exe` on node `k8s-node-primary` |
| Staged directory-enumeration output (host `corp-wks-8821`, user `marcus.aurelius`) | `/tmp/graph_objects.json` — a Microsoft Graph directory-enumeration dump |
| Staged Kubernetes secrets file | `/tmp/etcd_secrets.txt` |
| Configuration file exfiltrated off the app server | `.env`, pulled from `srv-app-prod-01` over HTTP |
| Perimeter verdict on the outbound connection | **Allowed** — the perimeter did not block the exfiltration |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access / Credential Access | T1078.004 – Valid Accounts: Cloud Accounts (break-glass account abuse) |
| Discovery | T1526 – Cloud Service Discovery; T1087.004 – Account Discovery: Cloud Account (Graph directory enumeration) |
| Credential Access | T1552.001 – Unsecured Credentials: Credentials in Files (`.env`, etcd secrets) |
| Collection / Exfiltration | T1530 – Data from Cloud Storage; T1041 – Exfiltration Over C2 Channel |

## Outcome

Closed as **True Positive**, 8/8 tasks. This was the broadest-scope operation of the nine — nine days of multi-service telemetry with no seeded starting alert — and the lowest-scoring of the identity/cloud scenarios (48.71%).

## Takeaway

This was the hardest operation to fully resolve: with no starting alert, the volume of plausible-but-irrelevant activity across four service types (identity, endpoint, Kubernetes, serverless) made it easy to under-attribute events to the wrong principal or miss a cross-service link. The finding that the perimeter **allowed** the final exfiltration is the most operationally important line in the case — it means detection, not prevention, was the only control that worked here, which is exactly the gap a SOC analyst is meant to catch and escalate as a control gap, not just an incident finding.
