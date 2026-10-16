# SOC Simulator — Incident Triage Reports

📄 **[Download the combined PDF](https://raw.githubusercontent.com/onewithalgo-web/SOC-Simulator-Incident-Reports/main/SOC_Simulator_Incident_Reports.pdf)** — the operation summaries in one document, recruiter-friendly.

📑 **Full per-operation incident reports** — each individual PDF (linked in the table below) follows a full SOC report template: metadata, executive summary, task-by-task triage, VirusTotal threat-intel checks, timeline, IOCs, a STIX 2.1 bundle, MITRE ATT&CK mapping, containment and recommendations, and real screenshots of the SOC Simulator evidence.

Analyst write-ups for hands-on SOC investigations completed on [SOC Simulator](https://www.socsimulator.com), a browser-based training platform that places you inside realistic SIEM, XDR, email and cloud-audit consoles to investigate scenarios mapped to the MITRE ATT&CK framework.

Each report below documents the scenario, the investigative approach taken across the available telemetry sources, the relevant ATT&CK technique mapping, and the outcome. These are training exercises, not live incidents — the scenario descriptions are platform-authored; the investigation narrative and ATT&CK mapping reflect my own analysis.

## Full incident reports

Each of these has a complete PDF report alongside the write-up: metadata, executive summary, task-by-task triage, VirusTotal checks, timeline, IOCs, a STIX 2.1 bundle, ATT&CK mapping, containment, recommendations and screenshots of the console evidence.

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

## Case write-ups

Shorter analyst write-ups for the rest of the completed operations — scenario, the confirmed findings in order, the ATT&CK mapping, and what the case is actually worth remembering. Full PDF reports are being produced for these in batches.

| # | Operation | Attack type | Level | Tools | Result |
|---|-----------|-------------|-------|-------|--------|
| 16 | [The Backdoored Browser Extension](reports/16-backdoored-browser-extension.md) | Trojanized extension, cookie theft | Easy | SIEM, Firewall | 6/6 tasks |
| 17 | [Trusted Domain, Untrusted Destination](reports/17-open-redirect-o365-phish.md) | Open-redirect credential phishing | Easy | Email, SIEM | 6/6 tasks |
| 18 | [Account Takeover: Impossible-Travel Sign-In](reports/18-impossible-travel-account-takeover.md) | Cloud account takeover | Easy | Cloud audit, SIEM | 6/6 tasks |
| 19 | [Spring4Shell: Class-Loader RCE to Webshell](reports/19-spring4shell-webshell.md) | Public-app exploitation (CVE-2022-22965) | Medium | SIEM, Firewall | 6/6 tasks |
| 20 | [Open Bucket, Open Books](reports/20-exposed-s3-bucket-data-leak.md) | Cloud storage exposure | Easy | Cloud audit | 4/4 tasks |
| 21 | [OneNote Attachment to RAT](reports/21-onenote-attachment-rat.md) | Attachment-delivered RAT | Easy | Email, XDR | 6/6 tasks |
| 22 | [Weaponized SVG: Embedded Code in an Image](reports/22-weaponized-svg-attachment.md) | SVG smuggling to scripted loader | Easy | Email, XDR | 6/6 tasks |
| 23 | [EvilProxy AiTM: M365 Cookie Theft](reports/23-evilproxy-aitm-m365.md) | AiTM session-token theft | Medium | Email, SIEM, Firewall | 7/7 tasks |
| 24 | [Illicit OAuth App-Consent Grant](reports/24-illicit-oauth-consent-entra.md) | Entra consent-grant campaign | Medium | Cloud audit, Email | 7/7 tasks |
| 25 | [GoldPickaxe: The First iOS Trojan Stealing Your Face](reports/25-goldpickaxe-ios-biometric-theft.md) | Mobile trojan, biometric theft | Medium | SIEM, Firewall, Query | 7/7 tasks |
| 26 | [Anatsa Banker: The Trojan Hiding in a Play Store Reader](reports/26-anatsa-google-play-dropper.md) | Android dropper to banking trojan | Easy | SIEM, Firewall | 6/6 tasks |
| 27 | [Exposed .git Folder: Scanning the Web for Secrets](reports/27-exposed-git-config-credential-scrape.md) | Credential cascade from exposed repo | Easy | SIEM, Firewall | 7/7 tasks |
| 28 | [FluBot: The Parcel-Delivery Text That Spreads Itself](reports/28-flubot-smishing-worm.md) | Smishing worm, DNS C2 | Easy | SIEM, Firewall | 6/6 tasks |
| 29 | [The Template That Read the Disk](reports/29-crushftp-ssti-session-theft.md) | SSTI to session theft (CVE-2024-4040) | Easy | SIEM, Firewall | 6/6 tasks |
| 30 | [Fighting Ursa: Car-for-Sale Lure to HeadLace](reports/30-headlace-apt28-car-lure.md) | APT28 DLL side-loading | Easy | XDR, Email, SIEM | 6/6 tasks |
| 31 | [Copyright Lure to Rhadamanthys Stealer](reports/31-rhadamanthys-copyright-lure.md) | Search-order hijack to infostealer | Medium | SIEM, XDR, Firewall | 6/6 tasks |
| 32 | [Fake Browser Update to Atomic macOS Stealer](reports/32-atomic-macos-stealer.md) | macOS keychain/browser theft | Easy | XDR, SIEM | 6/6 tasks |
| 33 | [Malicious npm Package: Postinstall Infostealer](reports/33-malicious-npm-postinstall.md) | Software supply chain | Easy | XDR, SIEM | 6/6 tasks |
| 34 | [Remcos RAT: Malicious Invoice Attachment](reports/34-remcos-invoice-attachment.md) | Commodity RAT via malspam | Easy | Email, XDR | 6/6 tasks |
| 35 | [Purchase Order, Poisoned](reports/35-formbook-purchase-order.md) | Exploit to hollowed-process stealer | Easy | Email, XDR, SIEM | 6/6 tasks |
| 36 | [Mounted and Loaded: ISO Container Bumblebee](reports/36-iso-container-bumblebee.md) | ISO delivery to Cobalt Strike | Medium | XDR, SIEM, Firewall, Query | 8/8 tasks |
| 37 | [SCARLETEEL: AWS Fargate Breach to Crypto-Mining](reports/37-scarleteel-aws-fargate.md) | Container breakout to cloud IAM abuse | Medium | Cloud audit, SIEM, Firewall | 7/7 tasks - First Blood |
| 38 | [Ivanti CSA Exploit Chain](reports/38-ivanti-csa-exploit-chain.md) | Chained zero-days to rootkit | Hard | SIEM, XDR, Firewall, Query | 8/8 tasks |
| 39 | [Hidden in the Pixels: LSB Steganography Exfil](reports/39-png-steganography-exfil.md) | Steganographic exfiltration | Medium | SIEM, Firewall, XDR, Query | 7/7 tasks |
| 40 | [FortiOS Bypass to Hunters International Ransomware](reports/40-fortios-bypass-hunters-ransomware.md) | Edge bypass to ransomware | Easy | SIEM, Firewall, XDR | 6/6 tasks |
| 41 | [UNC6384: Captive-Portal PlugX Implant](reports/41-plugx-captive-portal.md) | Captive-portal hijack to espionage implant | Easy | SIEM, XDR, Firewall | 7/7 tasks |
| 42 | [PowerShell Download Cradle](reports/42-powershell-download-cradle.md) | Encoded cradle to beacon | Easy | XDR, SIEM | 6/6 tasks |
| 43 | [Daemon in the Dark](reports/43-exposed-docker-api-kinsing.md) | Exposed container API to cryptomining | Easy | SIEM, Firewall, Cloud audit | 5/5 tasks |
| 44 | [Bumblebee Returns: The Voicemail VBA Macro](reports/44-bumblebee-voicemail-macro.md) | Macro loader via signed-binary proxy | Easy | SIEM, Email, Firewall, XDR | 7/7 tasks |
| 45 | [VPN Brute Force: Password Spray on the Portal](reports/45-vpn-password-spray.md) | Password spraying | Easy | SIEM, Firewall | 6/6 tasks |
| 46 | [Open SMB Share: Unauthorized Data Access](reports/46-open-smb-share-exposure.md) | Over-permissioned share, insider access | Easy | SIEM, Firewall | 6/6 tasks |
| 47 | [RDP Brute Force: Internet-Facing Server](reports/47-rdp-bruteforce-internet-facing.md) | Password guessing to interactive logon | Easy | SIEM | 6/6 tasks |
| 48 | [DarkGate via Microsoft Teams](reports/48-darkgate-teams-lure.md) | Teams external-chat lure to loader | Medium | XDR, SIEM, Email | 7/7 tasks |
| 49 | [Apache Path Traversal to RCE](reports/49-apache-path-traversal-rce.md) | CVE-2021-41773 to webshell | Easy | SIEM, Firewall | 6/6 tasks |
| 50 | [WordPress Plugin RCE: Webshell on a Site](reports/50-wordpress-plugin-rce.md) | Upload bypass to webshell | Easy | SIEM | 7/7 tasks |
| 51 | [Tomcat Manager Abuse: Weak Credentials](reports/51-tomcat-manager-weak-creds.md) | Default creds to WAR webshell, mining | Easy | SIEM | 6/6 tasks |
| 52 | [Transfer Denied: The MOVEit Web Shell](reports/52-moveit-webshell-data-theft.md) | MFT SQLi to header-gated webshell | Easy | SIEM, Firewall | 6/6 tasks |
| 53 | [Kernel Thread Impostor](reports/53-outlaw-ssh-cryptominer.md) | SSH brute force to disguised miner | Easy | SIEM, Firewall | 6/6 tasks |
| 54 | [Wide Open](reports/54-exposed-elasticsearch-wiper.md) | Exposed datastore, mining and wiping | Easy | SIEM, Firewall | 6/6 tasks |
| 55 | [Lazarus: ManageEngine RCE to QuiteRAT](reports/55-lazarus-manageengine-quiterat.md) | State-aligned espionage implant | Medium | SIEM, XDR, Firewall | 6/6 tasks |
| 56 | [GootLoader: SEO Poisoning to Domain Control](reports/56-gootloader-seo-domain-control.md) | Drive-by loader to DC compromise | Medium | SIEM, XDR, Firewall | 7/7 tasks |
| 57 | [Emotet Revival: Triage the November Loader](reports/57-emotet-revival-loader.md) | Thread hijack to encrypted C2 pool | Medium | SIEM, Email, Firewall | 7/7 tasks |
| 58 | [Cisco IOS XE Web UI: Implant and Rogue Admin](reports/58-cisco-ios-xe-implant.md) | Network-device implant (CVE-2023-20198) | Medium | SIEM, Firewall | 6/6 tasks |
| 59 | [Jenkins Arbitrary File Read to RCE](reports/59-jenkins-file-read-to-rce.md) | CI controller compromise (CVE-2024-23897) | Medium | SIEM, XDR, Query | 7/7 tasks |
| 60 | [Cleo Harmony MFT: Cl0p Data Theft](reports/60-cleo-mft-clop.md) | MFT autorun abuse to exfiltration | Medium | SIEM, Firewall | 7/7 tasks |
| 61 | [Ivanti Connect Secure: Auth Bypass to Webshell](reports/61-ivanti-connect-secure-webshell.md) | VPN appliance zero-day chain | Medium | SIEM, Firewall, XDR, Query | 7/7 tasks |
| 62 | [Overnight on the DMZ Wiki](reports/62-confluence-ognl-china-chopper.md) | OGNL injection to China Chopper | Medium | SIEM, Firewall | 6/6 tasks |
| 63 | [Edge Device Exploitation: VPN Zero-Day](reports/63-edge-device-vpn-zeroday.md) | Gateway RCE to internal pivot | Medium | SIEM, XDR, Firewall, Query | 8/8 tasks |
| 64 | [Search Engine Poisoning to the Domain Controller](reports/64-seo-poisoning-to-domain-controller.md) | Sponsored-result ISO to ransomware | Medium | SIEM, XDR | 8/8 tasks |
| 65 | [Operation Lunar Peek: PAN-OS Mgmt Takeover](reports/65-panos-lunar-peek-mgmt-takeover.md) | Firewall management-plane compromise | Hard | Firewall, SIEM | 8/8 tasks |
| 66 | [Removable Media: USB Autorun Malware](reports/66-usb-autorun-infection.md) | Removable-media infection | Easy | XDR, SIEM, Query | 6/6 tasks |
| 67 | [PAN-OS GlobalProtect Command Injection](reports/67-panos-globalprotect-command-injection.md) | CVE-2024-3400 to log-driven implant | Medium | SIEM, Firewall | 5/6 tasks |
| 68 | [CI/CD Pipeline Hijack: GitHub Actions](reports/68-github-actions-pipeline-hijack.md) | Supply-chain tag hijack to CI secret leak | Intermediate | XDR, SIEM, Threat Intel | 8/8 tasks |
| 69 | [Trust, Forged: Monitoring-Platform Backdoor](reports/69-solarwinds-sunburst-backdoor.md) | SUNBURST-style supply chain to forged SAML | Advanced | SIEM, Firewall, Cloud, Threat Intel | 7/7 tasks |

Operations marked *guided walkthrough* in their write-up are the platform's step-by-step format; the rest are open investigations where the console gives you the telemetry and nothing else.

## Platform stats (as of this writing)

- 69 operations completed
- 450 of 451 tasks solved
- Investigation skill: Expert (89/100) | Precision: Advanced (64/100) | Consistency: Competent (57/100) | Detection speed: Learning (39/100)
- Rank: Barbican (Level 6), with two First Bloods (first completion on the platform): Storm-0501: Azure Storage Key Abuse and Cloud Ransom, and SCARLETEEL: AWS Fargate Breach to Crypto-Mining

## What these cases have in common

Across the set, the decisive infrastructure is very often unflagged. The Snowflake VPN exit nodes, the Storm-0501 federated-domain backdoor and the QakBot C2 carried **no VirusTotal detections at all**, and the QakBot payload executed inside a legitimate signed Microsoft binary with a clean hash. Those cases were resolved on behaviour instead: authentication that skipped MFA, one identity appearing from distant addresses within the hour, a client application string absent from the baseline, a signed utility registering a file from a user directory, and a process opening sessions to addresses no DNS lookup preceded.

Three patterns repeat often enough to be worth naming:

- **The abused thing is usually a feature, not a flaw.** Docker's API on 2375, Tomcat's WAR deployment, Cleo's autorun directory, an S3 bucket policy, a `postinstall` hook, an OAuth consent grant — in each case the product did exactly what it documents, and the compromise happened at configuration time.
- **Signed binaries carry the payload.** DLL side-loading and search-order hijacking appear in five separate cases (HeadLace, PlugX, Rhadamanthys, Bumblebee, FormBook). File reputation is the wrong detection surface; what is anomalous is a signed binary running from a user-writable directory with an unexpected DLL beside it.
- **Identity persistence outlives credential resets.** A recovery address, a registered MFA method, a consent grant, a rogue appliance admin — these survive the password reset that most playbooks treat as containment.

Reputation data confirmed the phishing cases and contributed almost nothing to the rest. Several reports end on control gaps rather than indicators — a proxy that correctly categorised a newly-registered domain and allowed it anyway, a firewall rule named for web traffic that permitted a RAT beacon on port 6606, a missing account lockout policy — which is the more useful finding to hand to whoever owns the policy.

## Skills demonstrated

- Alert triage and log correlation across SIEM, XDR, email, firewall and cloud-audit interfaces
- Identity-centric incident response: OAuth consent abuse, device-code phishing, AiTM session-token theft, MFA fatigue, impossible-travel account takeover
- Endpoint investigation: PowerShell/LOLBin abuse, DLL side-loading, process injection and hollowing, persistence mechanisms, infostealer behaviour
- Edge and appliance compromise: VPN, firewall, MFT and network-device exploitation, including chained zero-days and post-exploitation implants
- Cloud and container investigation: IAM credential theft, metadata-service abuse, storage exposure, resource hijacking
- Ransomware kill-chain reconstruction: initial access through lateral movement to impact, including cloud-native extortion with no encryptor binary
- MITRE ATT&CK technique mapping for each investigated attack chain
- Containment decision-making (the platform grades not just detection but choosing the action that actually evicts the attacker)

## Related work

- [Malware-Analysis-MSIL-RAT](https://github.com/onewithalgo-web/Malware-Analysis-MSIL-RAT) — static and dynamic analysis of a live Windows RAT sample
- [SQL-Injection-Write-Up](https://github.com/onewithalgo-web/SQL-Injection-Write-Up)
- [Python-ATM-System](https://github.com/onewithalgo-web/Python-ATM-System)
