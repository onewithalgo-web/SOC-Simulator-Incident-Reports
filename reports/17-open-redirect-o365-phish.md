# Trusted Domain, Untrusted Destination: Open-Redirect Phishing

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** Email, SIEM | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A finance clerk at Larkfield Mutual Assurance clicked a "Release My Messages" link in an *Undelivered Mails* phishing email. The link opened on a trusted brand domain carrying an open-redirect flaw, so it sailed past URL filtering. From there a 302 redirect handed the browser to an attacker cushion server, a JavaScript hop landed on a spoofed Microsoft 365 login page, and the harvested credentials were replayed against the real tenant.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Trusted domain inside the phishing link | `citi[.]com` — abused open redirect, which is why URL reputation passed it |
| First hop off the trusted domain | `frudyj.codesandbox[.]io` — cushion server |
| Page that stole the credentials | `gw8aes.office-docs[.]net` — spoofed M365 login |
| Credential replay source | `37.120.234[.]53` |
| Technique classification | **T1684.001** — Phishing for Information via spoofed sign-in |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Phishing: Spearphishing Link |
| Reconnaissance | T1598.003 – Phishing for Information: Spearphishing Link |
| Credential Access | T1684.001 – Credential phishing via spoofed portal; T1539 – Steal Web Session Cookie |

## Takeaway

URL reputation is a check on the first hop only. The entire value of the open redirect to the attacker was that the link a human — and a filter — inspects is a bank's real domain. The detectable moment was not delivery but the chain: a redirect off a financial brand into a developer sandbox host, then onto a lookalike login. Proxy logs that record the full redirect chain, rather than only the final URL, are what make that visible.
