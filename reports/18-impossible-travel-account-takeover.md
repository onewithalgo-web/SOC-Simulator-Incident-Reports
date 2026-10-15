# Account Takeover: Impossible-Travel Sign-In

**Platform:** SOC Simulator | **Tools:** Cloud audit, SIEM | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A finance analyst's Microsoft Entra account was accessed without MFA from Moldova 18 minutes after a normal London sign-in. The unauthorized session read the inbox over Microsoft Graph and added an external recovery address — persistence that survives a password reset.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Unauthorized entry point | `185.153.0[.]47` |
| Travel gap between the two sign-ins | 18 minutes — geographically impossible |
| Mailbox accessed | `m.alderton@thornvale-ins[.]com` |
| Persistence move | `otherMails` — external recovery address written to the user object |
| Initial access classification | **T1078.004** — Valid Accounts: Cloud Accounts |
| Collection classification | **T1114** — Email Collection |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access / Defense Evasion | T1078.004 – Valid Accounts: Cloud Accounts |
| Credential Access | T1110.003 – Password Spraying |
| Collection | T1114 – Email Collection |
| Persistence | T1098 – Account Manipulation |

## Takeaway

The impossible-travel alert is the cheap part of this case; the expensive part is the `otherMails` write. Resetting the password closes the session and leaves the attacker a self-service route back in. Any account-takeover playbook that stops at "reset credentials, revoke tokens" without auditing authentication-method and recovery-address changes hands the account back.
