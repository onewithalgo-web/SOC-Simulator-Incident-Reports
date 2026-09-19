# SOC Simulator — Incident Triage Reports

📄 **[Download the combined PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/SOC_Simulator_Incident_Reports.pdf)** — the operation summaries in one document, recruiter-friendly.

📑 **Full per-operation incident reports** — each individual PDF (linked in the table below) follows a full SOC report template: metadata, executive summary, task-by-task triage, VirusTotal threat-intel checks, timeline, IOCs, a STIX 2.1 bundle, MITRE ATT&CK mapping, containment and recommendations, and real screenshots of the SOC Simulator evidence.

Analyst write-ups for hands-on SOC investigations completed on [SOC Simulator](https://www.socsimulator.com), a browser-based training platform that places you inside realistic SIEM, XDR, email and cloud-audit consoles to investigate scenarios mapped to the MITRE ATT&CK framework.

Each report below documents the scenario, the investigative approach taken across the available telemetry sources, the relevant ATT&CK technique mapping, and the outcome. These are training exercises, not live incidents — the scenario descriptions are platform-authored; the investigation narrative and ATT&CK mapping reflect my own analysis.

## Reports

| # | Operation | Attack Type | Tools | Score | Full report (PDF) |
|---|-----------|-------------|-------|---------|-------------------|
| 1 | [StealC: Cheat Tool to Emptied Wallet](reports/01-stealc-cheat-tool-wallet-theft.md) | Infostealer via trojanized download | SIEM, XDR | 6/6 tasks | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op01_Easy.pdf) |
| 2 | [Entra ID Device Code Phishing: Token Theft](reports/02-entra-id-device-code-phishing.md) | OAuth device-code phishing | SIEM, Cloud audit, Email | 7/7 tasks, 88.1% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op02_Intermediate.pdf) |
| 3 | [Fake Zoom to Ransomware: The Social Engineering Pipeline](reports/03-fake-zoom-to-ransomware.md) | Multi-stage loader chain to ransomware | SIEM, XDR, Firewall | 10/10 tasks, 92.5% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op03_Advanced_2.pdf) |
| 4 | [Credential Harvesting: The Lookalike Login](reports/04-credential-harvesting-lookalike-login.md) | AiTM credential phishing | SIEM | 7/7 tasks, 43.17% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op04_Easy.pdf) |
| 5 | [MFA Fatigue: The Notification Flood](reports/05-mfa-fatigue-notification-flood.md) | MFA fatigue / push-bombing | SIEM, XDR | 6/6 tasks, 36.25% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op05_Easy.pdf) |
| 6 | [ClickFix: The Fake CAPTCHA Trap](reports/06-clickfix-fake-captcha-trap.md) | Paste-and-run social engineering | SIEM, XDR | 5/5 tasks, 70.83% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op06_Easy.pdf) |
| 7 | [QR Code Phishing: Scan to Compromise](reports/07-qr-code-phishing-scan-to-compromise.md) | Quishing + AiTM session theft | SIEM | 6/6 tasks, 100% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op07_Easy.pdf) |
| 8 | [Finance Mailbox Takeover at MegaCorp Logistics](reports/08-finance-mailbox-takeover.md) | Endpoint compromise to BEC | SIEM, XDR, Email | 8/8 tasks, 56.08% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op08_Intermediate.pdf) |
| 9 | [Cloud Identity Under Siege](reports/09-cloud-identity-under-siege.md) | Cross-service cloud intrusion | SIEM, XDR, Firewall, Cloud audit | 8/8 tasks, 48.71% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op09_Intermediate.pdf) |
| 10 | [Frozen Assets: The Snowflake Tenant Heist](reports/10-snowflake-tenant-heist.md) | Credential-driven warehouse exfiltration | SIEM, Cloud | 6/6 tasks, 70.83% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op10_Intermediate.pdf) |
| 11 | [Fake Window, Real Loss: Browser-in-the-Browser Steam Phishing](reports/11-bitb-steam-phishing.md) | Browser-in-the-browser credential theft | SIEM, Email | 6/6 tasks, 61.18% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op11_Easy.pdf) |
| 12 | [Storm-0501: Azure Storage Key Abuse and Cloud Ransom](reports/12-storm0501-cloud-ransom.md) | Cloud-native ransomware and extortion | SIEM, Cloud | 8/8 tasks, 69.05% - First Blood | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op12_Advanced.pdf) |
| 13 | [QakBot bb02: Trace the Loader DLL to its C2](reports/13-qakbot-bb02-c2.md) | Signed-binary proxy execution to C2 | SIEM, XDR, Firewall | 6/6 tasks, 60% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op13_Easy.pdf) |
| 14 | [Overdue Invoice, Compromised Endpoint](reports/14-finance-workstation-intrusion.md) | LNK loader to registry persistence | SIEM, Email | 6/6 tasks, 70% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op14_Easy.pdf) |
| 15 | [Hijacked Discord Invite to ClickFix Loader](reports/15-discord-clickfix-loader.md) | ClickFix to AsyncRAT and infostealer | SIEM, Firewall | 7/7 tasks, 80.56% | [PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/Mbanjwa_May26_SOCTriage_Op15_Easy.pdf) |

Every operation so far closed **True Positive**. Task-completion rate reflects the platform's own grading of first-attempt accuracy on specific IOCs/artifacts, not whether the root cause was found — every case above was correctly resolved and correctly contained.

## Platform stats (as of this writing)

- 23 operations completed, 37 of 85 MITRE ATT&CK techniques covered across 14 tactics
- Investigation skill: Expert (81/100) | Detection speed: Learning (29/100) | Precision: Competent (53/100)
- Rank: Watchman (Level 4), with one First Blood (Storm-0501: Azure Storage Key Abuse and Cloud Ransom)


## What these cases have in common

In three of the six most recent investigations the decisive infrastructure had **no VirusTotal detections at all**. The Snowflake VPN exit nodes, the Storm-0501 federated-domain backdoor and the QakBot C2 were unflagged by every vendor, and the QakBot payload executed inside a legitimate signed Microsoft binary with a clean hash. Those cases were resolved on behaviour instead: authentication that skipped MFA, one identity appearing from distant addresses within the hour, a client application string absent from the baseline, a signed utility registering a file from a user directory, and a process opening sessions to addresses no DNS lookup preceded.

Reputation data confirmed the phishing cases and contributed almost nothing to the rest. Two of the reports also end on control gaps rather than indicators - a proxy that correctly categorised a newly-registered domain and allowed it anyway, and a firewall rule named for web traffic that permitted a RAT beacon on port 6606 - which is the more useful finding to hand to whoever owns the policy.

## Skills demonstrated

- Alert triage and log correlation across SIEM, XDR, email and cloud-audit interfaces
- Identity-centric incident response: OAuth/device-code abuse, AiTM session-token theft, MFA fatigue, impossible-travel account takeover
- Endpoint investigation: PowerShell/LOLBin abuse, process injection, persistence mechanisms, infostealer behaviour
- Ransomware kill-chain reconstruction: initial access through lateral movement to impact
- MITRE ATT&CK technique mapping for each investigated attack chain
- Containment decision-making (the platform grades not just detection but choosing the action that actually evicts the attacker)

## Related work

- [Malware-Analysis-MSIL-RAT](https://github.com/onewithalgo-web/Malware-Analysis-MSIL-RAT) — static and dynamic analysis of a live Windows RAT sample
- [SQL-Injection-Write-Up](https://github.com/onewithalgo-web/SQL-Injection-Write-Up)
- [Python-ATM-System](https://github.com/onewithalgo-web/Python-ATM-System)
