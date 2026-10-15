# Open SMB Share: Unauthorized Data Access

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A finance file share on an internal Windows server was misconfigured to allow all domain users read access. A workstation account with no finance role connected over SMB and bulk-read payroll records, M&A strategy documents, and board materials. The session was reconstructed from Windows Security audit events and internal firewall logs.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Share connection | `FINANCE-ARCHIVE$` |
| Accessing account | `porter.callahan` |
| Workstation source | `10.114.30[.]42` |
| Sensitive files accessed | 5 |
| Misconfigured permission | `Domain Users` granted read |
| Collection classification | **T1039** — Data from Network Shared Drive |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Discovery | T1135 – Network Share Discovery |
| Collection | T1039 – Data from Network Shared Drive; T1530 – Data from Cloud Storage |
| Initial Access / Privilege Escalation | T1078 – Valid Accounts |

## Takeaway

An insider case with no exploit and no malware — the share's ACL authorized every byte that was read. Detection has to come from behaviour against a baseline of role: a non-finance account opening a finance archive and reading board material is anomalous even though it is permitted. The `$` suffix is also worth noting, since a hidden share is often mistaken for a protected one; it hides the share from browsing and protects nothing.
