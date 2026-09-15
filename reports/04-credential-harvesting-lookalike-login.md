# Credential Harvesting: The Lookalike Login

**Platform:** SOC Simulator | **Tools:** SIEM | **Duration:** ~20 min | **Verdict:** True Positive — 7/7 tasks, 43.17% accuracy

## Scenario

An adversary-in-the-middle (AiTM) credential-phishing campaign lured an employee, `alicia.rodriguez` (host `corp-wks-7712`), to a lookalike Microsoft 365 login page. Worked entirely from SIEM logs: email gateway/DNS, network connections, and Azure AD sign-in records.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Primary phishing domain | `login.microsoftonline.in[.]net` |
| Infrastructure hosting the AiTM page | `163.5.221[.]110` |
| Secondary phishing domain (second victim) | `integralsm[.]cl` — found by widening the search past Alicia's own workstation |
| First hop of the redirect chain | `absoluteprintgroup[.]com` (a compromised legitimate site) |
| Account-takeover confirmation | Azure AD sign-in as `alicia.rodriguez` from an attacker machine reporting OS **`linux`** — no MFA failure logged, because a stolen session cookie authenticates without triggering the checks a stolen password would |
| Containment answer | Revoke Alicia's active sessions and tokens, then reset her password |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Phishing: Spearphishing Link |
| Credential Access | T1557 – Adversary-in-the-Middle; T1539 – Steal Web Session Cookie |
| Defense Evasion / Persistence | T1078.004 – Valid Accounts: Cloud Accounts |

## Outcome

Closed as **True Positive**. Correct containment: revoke the stolen session/refresh token and force re-authentication — a password reset alone does not invalidate an already-stolen session cookie.

## Takeaway

This operation's lower score reflects where I initially under-scoped the incident: I confirmed the primary victim and the hosting infrastructure quickly but was slower to pivot onto the secondary phishing domain (`integralsm.cl`) targeting a second employee off the same campaign. Lesson applied since: always pivot on infrastructure indicators (redirect domain, hosting IP) across the full user population before closing scope, not just the reporting user — this exact gap is what I closed cleanly two operations later in [QR Code Phishing](07-qr-code-phishing-scan-to-compromise.md), where I traced the full multi-hop infrastructure and campaign scope on the first pass (100% accuracy).
