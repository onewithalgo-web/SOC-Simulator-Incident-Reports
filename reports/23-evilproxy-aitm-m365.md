# EvilProxy AiTM: Indeed Redirect to M365 Cookie Theft

**Platform:** SOC Simulator | **Tools:** Email, SIEM, Firewall | **Duration:** ~45m | **Verdict:** True Positive — 7/7 tasks

## Scenario

An executive at a logistics firm clicked a job-themed phishing link that abused a recruiting platform's open redirect to reach an EvilProxy adversary-in-the-middle page. The page reverse-proxies the genuine Microsoft 365 sign-in, so the victim completed MFA against the attacker, who captured and replayed the post-MFA session cookie.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Compromised account | `mason.ackerley@calderwood-logistics[.]com` |
| Delivery redirect | `t.jobreachly[.]com` — recruiting-platform open redirect |
| Adversary-in-the-middle landing | `lmo.fenmaragroup.com[.]au` — compromised legitimate domain |
| Relay behind the session | `185.173.41[.]18` |
| Interception classification | **T1557** — Adversary-in-the-Middle |
| MFA bypass classification | **T1539** — Steal Web Session Cookie |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Spearphishing Link |
| Credential Access | T1684.001 – Credential phishing; T1557 – Adversary-in-the-Middle; T1111 – Multi-Factor Authentication Interception; T1539 – Steal Web Session Cookie |

## Takeaway

The victim's MFA worked exactly as designed — against the wrong relying party. Because the proxy forwards the real sign-in, every credential and challenge is genuine, and the only artefact that distinguishes the session is the token's issuance context. Phishing-resistant authentication (FIDO2, certificate-based) is the structural fix; token-binding and sign-in risk policies are what detect it in the meantime.
