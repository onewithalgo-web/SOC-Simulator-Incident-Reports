# Hijacked Discord Invite to ClickFix Loader: Tracing the Lure

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~15m | **Verdict:** True Positive — 7/7 tasks, 80.56% accuracy

## Scenario

A finance analyst follows a recycled Discord invite that quietly redirects the browser to a fake verification page. She does what the page asks — pastes a command into Windows and runs it — and inside the hour the workstation is running a remote-access trojan alongside an info-stealer shipping browser data out of the building.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Fake-CAPTCHA lure page | `captchaguard[.]me` — reached via a 302 from `discord.gg/r-modlab` through redirector `36.92.41[.]115`; proxy category **newly-registered-domain**, allowed |
| What the ClickFix prompt made the user run | `powershell.exe` — parent `explorer.exe`, `-w hidden -ec` with a base64 payload |
| First payload source | `hxxps://github[.]com/frfs1/update/raw/refs/heads/main/installer.exe`, which then pulled `AClient.exe` from `bitbucket.org` into `AppData\Local\Temp` |
| Command-and-control address | `101.99.76[.]120:6606` — with rotation to `185.234.247[.]8:6606` twelve minutes later |
| What stole the browser data | `skul.exe` — POSTed browser cookies and Discord tokens to a Discord webhook |
| User-execution classification | **T1204.001** — User Execution: Malicious Link |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Execution | T1204.001 – User Execution: Malicious Link; T1059.001 – PowerShell |
| Defense Evasion | T1027 – Obfuscated Files or Information |
| Command and Control | T1105 – Ingress Tool Transfer; T1102 – Web Service; T1571 – Non-Standard Port; T1219 – Remote Access Software |
| Credential Access | T1539 – Steal Web Session Cookie |
| Exfiltration | T1567.004 – Exfiltration Over Web Service: Exfiltration to Webhook |

## Outcome

Closed as **True Positive**, 7/7 tasks — the highest accuracy of this batch at 80.56%. No exploit was involved at any point; the chain moved forward only because a person acted.

## Takeaway

This is a clear demonstration of the limits of destination-based blocking. The invite was real Discord, the payloads were hosted on GitHub and Bitbucket, and the exfiltration went to Discord's own API — none of which an organisation can sensibly block. Two local signals would still have caught it: the proxy already knew `captchaguard[.]me` was registered days earlier and allowed it anyway, and the firewall permitted an outbound session on port 6606 under a rule named `Allow-Web-Egress`. Both are policy decisions rather than missing technology, which is the most actionable kind of finding a report can produce. Worth noting too that stolen session cookies defeat MFA, so containment here is session revocation, not just a password reset.
