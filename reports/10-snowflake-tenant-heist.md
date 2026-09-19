# Frozen Assets: The Snowflake Tenant Heist

**Platform:** SOC Simulator | **Tools:** SIEM, Cloud | **Duration:** ~1h | **Verdict:** True Positive — 6/6 tasks, 70.83% accuracy

## Scenario

A retailer's Snowflake data warehouse is fronted by single sign-on. A contractor credential, consistent with the scenario's infostealer-log premise, is validated and then used from commercial VPN exits against a tenant with neither MFA nor a network policy. The whole intrusion runs for 96 minutes and is visible in the Snowflake audit trail: 17 of 185 cloud events.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Compromised account | `J_HOLLOWAY` — a contractor identity, MFA not used |
| First exit node | `45.134.142[.]77` — two failed logons at 01:40 and 01:41, then a single-factor success at 01:47:12 |
| Tooling tell | Client application string `FROSTHARVEST`, present on every malicious session and on no legitimate one |
| Staging step | `@LUMINA_PROD.RETAIL.TMP_EXP9920` — a temporary stage created 92 seconds before the unload |
| Exfiltrated table | `LUMINA_PROD.RETAIL.CUSTOMER_PII` — 4,821,663 rows, 2 GiB scanned, gzip CSV, warehouse `WH_ADHOC` |
| Exfiltration classification | **T1567.002** — Exfiltration to Cloud Storage |

Three source addresses were used on one identity inside 76 minutes: `45.134.142[.]77`, `146.70.171[.]22` and `193.32.126[.]40`.

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access / Persistence / Privilege Escalation / Defense Evasion | T1078 – Valid Accounts |
| Discovery | T1087 – Account Discovery; T1580 – Cloud Infrastructure Discovery |
| Collection | T1213 – Data from Information Repositories; T1074 – Data Staged; T1530 – Data from Cloud Storage |
| Exfiltration | T1567.002 – Exfiltration Over Web Service: Exfiltration to Cloud Storage |

All but T1567.002 are taken from the platform's own ATT&CK Cloud panel for this incident.

## Outcome

Closed as **True Positive**, 6/6 tasks. Three detection rules fired: no-MFA logon from an untrusted IP, impossible travel for a data-platform identity, and failed-then-successful logon.

## Takeaway

Every one of the three source addresses is clean on VirusTotal — Datacamp, M247 and 31173 Services are ordinary commercial VPN providers, and renting an exit node is not itself malicious. Reputation data contributed nothing to this case. What exposed it was behaviour around the identity: authentication that skipped MFA, the same account appearing from distant addresses within the hour, a client application string that existed nowhere else in the tenant, and one query that read a customer table in full. A SOC that only alerts on known-bad infrastructure would have missed this entirely.
