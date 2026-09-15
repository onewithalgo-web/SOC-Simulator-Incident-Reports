# QR Code Phishing: Scan to Compromise

**Platform:** SOC Simulator | **Tools:** SIEM | **Duration:** ~15 min | **Verdict:** True Positive — 6/6 tasks, 100% accuracy

## Scenario

A modern "quishing" (QR-code phishing) attack that bypasses traditional email filters by hiding its payload inside an image rather than a clickable link. ~47 employees received a fake "Action Required: MFA Enrollment" email carrying a QR code; at least 3 scanned it on personal phones (bypassing corporate web filtering entirely). One confirmed compromise: `sarah.jenkins`.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Redirect server (1st hop from the decoded QR URL) | `176.111.219[.]85` |
| Credential-harvesting / AiTM server | `185.234.72[.]19` — an Evilginx-style proxy that stole an authenticated session cookie despite MFA |
| Post-compromise infrastructure | `185.234.72[.]40` — separate infra the attacker used to operate *as* Sarah Jenkins rather than to phish her, confirming mature operational separation between phishing and operating infrastructure |
| Phishing domain (email + QR generation) | `m365-mfa-enroll.com` |
| Post-compromise activity | Graph API mailbox enumeration, SharePoint document exfiltration, and a hidden inbox-forwarding rule |
| Containment answer | Revoke `sarah.jenkins`'s active sessions/tokens and remove the malicious inbox forwarding rule |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Phishing: Spearphishing Link (QR-embedded) |
| Credential Access | T1557 – Adversary-in-the-Middle; T1539 – Steal Web Session Cookie |
| Collection | T1114.002 – Email Collection: Remote Email Collection; T1213.002 – Data from Information Repositories: SharePoint |
| Persistence | T1114.003 – Email Forwarding Rule |
| Defense Evasion | T1078.004 – Valid Accounts: Cloud Accounts |

## Outcome

Closed as **True Positive** with a perfect task score. Correct containment: revoke the stolen session/token, force re-authentication, remove the hidden forwarding rule, and audit SharePoint access logs for the scope of data exposed.

## Takeaway

Full-chain reconstruction — not just spotting the initial phish — is what closed this cleanly: the forwarding rule alone would have let the attacker keep visibility into the mailbox even after a password reset, so containment had to explicitly target every persistence mechanism the AiTM foothold created. This operation directly applies the lesson from [Credential Harvesting: The Lookalike Login](04-credential-harvesting-lookalike-login.md) — scope the full campaign infrastructure, not just the reporting user — and the 100% score reflects that.
