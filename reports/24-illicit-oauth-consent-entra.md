# Illicit OAuth App-Consent Grant: Entra ID Impersonation Campaign

**Platform:** SOC Simulator | **Tools:** Cloud audit, Email | **Duration:** ~40m | **Verdict:** True Positive — 7/7 tasks

## Scenario

A mass illicit OAuth app-consent campaign against a maritime logistics tenant. Operators registered multitenant Entra applications impersonating trusted SaaS brands (iLSMART, Adobe, DocuSign, OneDrive-2025) and sent brand-themed consent lures from compromised supplier accounts. Victims who clicked were screened by a Tycoon antibot redirector into an adversary-in-the-middle relay that proxied the real Entra sign-in, harvested the password, and intercepted the MFA-approved token. The replayed token completed user-level consent grants to the impersonation apps, which then read mailboxes over Microsoft Graph, and the attacker registered a new MFA method for durable persistence.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| First compromised identity | `ingvild.brattheim@nordkliff-maritime[.]com` |
| App the analyst consented to | `14b2864e-3cff-4d33-b5cd-7f14ca272ea4` |
| Impersonation app's true reply URL | `https://azureapplicationregistration.pages[.]dev/redirectapp` |
| Relay redirector in the lure | `gmlygt[.]ru` — Tycoon antibot screening |
| Persistence on the second victim | `Security Method Add` — new MFA method registered |
| MFA-bypass classification | **T1550.001** — Use Alternate Authentication Material: Application Access Token |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Spearphishing Link |
| Credential Access | T1528 – Steal Application Access Token |
| Defense Evasion / Lateral Movement | T1550.001 – Application Access Token |
| Persistence | T1098.003 – Additional Cloud Roles |

## Takeaway

The consent grant is the part that outlives everything else. Revoking sessions and resetting passwords does nothing to an OAuth application that already holds `Mail.Read` on the user's behalf — it keeps reading the mailbox over Graph with its own token. The two controls that would have prevented the whole campaign are blocking user consent to unverified publishers and alerting on `Add service principal` / `Consent to application` events for multitenant apps whose reply URL is not the brand they claim.
