# VPN Brute Force: Credential Attack on the Remote-Access Portal

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A password-spray campaign targeted the Halcyon Freight SSL-VPN portal from two rotating source IPs, submitting credentials across many accounts to stay under per-account lockout thresholds. One account eventually matched, and the operator opened an active tunnel session.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Attack pattern | **T1110.003** — Password Spraying |
| Accounts targeted | 10 |
| Account where the spray succeeded | `d.volkov` |
| IP that opened the active session | `185.159.0[.]209` |
| Movement over the tunnel | `HALCYON-WKS-14` |
| Access classification | **T1133** — External Remote Services |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Credential Access | T1110.003 – Password Spraying |
| Initial Access | T1133 – External Remote Services; T1078 – Valid Accounts |
| Command and Control | T1071.001 – Web Protocols |

## Takeaway

Spraying is designed to be invisible to per-account thresholds — one or two attempts per user never triggers a lockout, so the detection has to be horizontal: many accounts, few attempts each, one source, short window. The successful login then came from a different address than the spray, which is the usual separation between the credential-testing infrastructure and the operator. MFA on the VPN portal makes the entire campaign a non-event.
