# RDP Brute Force: Internet-Facing Server Login

**Platform:** SOC Simulator | **Tools:** SIEM | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

An internet-exposed Windows Server running RDP had been receiving a sustained brute-force campaign from an external address. After dozens of failed authentication events, one attempt succeeded and an interactive session was opened.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Anomaly in the authentication log | `45.152.14[.]83` — sustained 4625 failures |
| Target account | `r.castellano` |
| Logon type on success | `10` — RemoteInteractive |
| Moment of compromise | 09:44 |
| Missing control | Account Lockout Policy |
| Technique | **T1110.001** — Brute Force: Password Guessing |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Credential Access | T1110.001 – Password Guessing |
| Initial Access | T1133 – External Remote Services; T1078 – Valid Accounts |
| Lateral Movement | T1021.001 – Remote Desktop Protocol |

## Takeaway

Logon type 10 is what separates this from background internet noise: thousands of failures are weather, one RemoteInteractive success is an incident. The absence of a lockout policy is the finding with the most value to the business, because it turns an unbounded guessing campaign into a matter of time. RDP on a public interface should be behind a gateway or VPN; where that is not possible, lockout plus network-level authentication is the minimum.
