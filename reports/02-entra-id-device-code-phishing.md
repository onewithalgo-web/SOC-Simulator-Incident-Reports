# Entra ID Device Code Phishing: Token Theft

**Platform:** SOC Simulator | **Tools:** SIEM, Entra sign-in/Graph audit logs, Email | **Duration:** ~45 min | **Verdict:** True Positive — 7/7 tasks, 88.1% accuracy

## Scenario

A Microsoft 365 user at Arlowick Financial Services received a convincing phishing email asking her to enter a device code at the legitimate Microsoft `devicelogin` page. Entering the code completed the attacker's OAuth device-authorization request, handing over a valid access token without the victim ever "entering a password on a fake site."

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Compromised account | `kate.henderson@arlowick-fs.com` |
| Authentication anomaly | Sign-in via the `deviceCode` grant type — legitimate for device enrollment, abused here as a phishing vector |
| Attacker's first IP | `185.125.0[.]44` |
| Rogue application | Client ID `f2a9c817-3b4e-4d91-a7c0-8f1d2e3b5a6c` — the app the stolen token was issued to |
| Phishing sender domain | `ms-teams-notify[.]io` |
| Data accessed | Graph API path `/v1.0/users/kate.henderson@arlowick-fs.com/mailFolders/inbox/messages` |
| Technique classification | **T1528 – Steal Application Access Token** |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Phishing: Spearphishing Link |
| Credential Access | T1528 – Steal Application Access Token |
| Collection | T1114.002 – Email Collection: Remote Email Collection |
| Defense Evasion / Persistence | T1078.004 – Valid Accounts: Cloud Accounts |

## Outcome

Closed as **True Positive**, 88.1% accuracy. Containment: revoke the OAuth refresh/access tokens and Kate Henderson's active sessions, remove the rogue application registration, disable device-code flow at the tenant level where not required, and add conditional-access policies restricting device-code auth to managed devices.

## Takeaway

MFA doesn't defeat every phishing technique — device-code phishing abuses a legitimate OAuth flow rather than harvesting a password, so detection has to live in the sign-in/Graph audit logs (the `deviceCode` grant type and the unrecognized application ID), not just at the credential layer.
