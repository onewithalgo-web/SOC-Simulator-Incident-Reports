# Finance Mailbox Takeover at MegaCorp Logistics

**Platform:** SOC Simulator | **Tools:** SIEM, Endpoint XDR, Mail platform logs | **Duration:** ~45 min | **Verdict:** True Positive — 8/8 tasks, 56.08% accuracy

## Scenario

A finance analyst at MegaCorp Logistics reported that colleagues were receiving replies to messages she never sent; her account was disabled while the investigation ran. Reconstructed which host produced the endpoint evidence, what its browser reached, how the account was taken over, and what was left on the mail platform.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Compromised endpoint | `corp-wks-082` |
| Outbound connection traced | `ywnjb[.]com` — malicious domain reached by the workstation's browser |
| First XDR detection | **T1557** – Adversary-in-the-Middle |
| Second XDR detection (same host, minutes later) | **T1539** – Steal Web Session Cookie |
| Adversary's objective (rolled-up ATT&CK tactic) | Credential access |
| Delivered file | `invoice.svg` — the initial lure attachment |
| Launching process | `msedge.exe` — opened the SVG, which carried the AiTM redirect (SVG smuggling) |
| Mail-server artifact | An **Email Forwarding Rule** left behind, explaining why colleagues kept receiving unsolicited replies from the compromised mailbox |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.001 – Phishing: Spearphishing Attachment (SVG smuggling) |
| Credential Access | T1557 – Adversary-in-the-Middle; T1539 – Steal Web Session Cookie |
| Persistence / Defense Evasion | T1078.004 – Valid Accounts: Cloud Accounts |
| Persistence | T1114.003 – Email Forwarding Rule |

## Outcome

Closed as **True Positive**, 8/8 tasks. Containment: remediate `corp-wks-082`, revoke the compromised account's sessions and reset credentials, remove the attacker-created forwarding rule, block `ywnjb.com` at the perimeter/DNS, and notify the counterparties who received attacker-sent replies as part of the business-email-compromise attempt.

## Takeaway

This scenario required tying three separate evidence sources — endpoint (`msedge.exe` opening the SVG), network (the AiTM domain), and mail platform (the forwarding rule) — into one causal chain rather than treating them as independent alerts. My score here (56.08%) reflects spending more time confirming the endpoint-to-mailbox link than the platform expected; a reminder to timebox host-level triage once the pivot to the identity/mail layer is clear, similar to the scoping lesson from [Credential Harvesting: The Lookalike Login](04-credential-harvesting-lookalike-login.md).
