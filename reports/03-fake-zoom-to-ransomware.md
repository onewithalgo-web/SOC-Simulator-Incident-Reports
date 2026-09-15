# Fake Zoom to Ransomware: The Social Engineering Pipeline

**Platform:** SOC Simulator | **Tools:** SIEM, XDR, Firewall | **Duration:** ~1h 40m (10 tasks, advanced) | **Verdict:** True Positive — 10/10 tasks, 92.5% accuracy

## Scenario

An advanced, multi-stage intrusion based on real-world 2025 threat intelligence. It begins with a drive-by download of a trojanized Zoom installer on host `CORP-WKS-102` (user `adriana.garcia`), progresses through loader stages to establish C2, moves laterally, steals backup credentials, and finishes with mass deployment of ransomware through enterprise software-deployment tooling.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Initial access vector | `Zoom_v_2.00.4.exe`, downloaded to `Downloads` and executed at 08:24:53 |
| Installer hash | SHA-256 `ecb0b3057163cd25c989a66683cfb47c19f122407cbbb49b1043e908c4f07ad1` |
| Execution chain | `mshta.exe` fetched a remote `.hta` from `92.51.2.22`, which handed off to **`MSBuild.exe`** via remote thread injection |
| C2 channel | Outbound to `45.141.87[.]218:443` immediately after the MSBuild injection |
| Attacker remote-access origin | `92.51.2[.]27` — source of later remote authentication attempts against the host |
| Second-stage payload | `ZoomUpdater.exe`, SHA-256 `498BA0AFA5D3B390F852AF66BD6E763945BF9B6BFF2087015ED8612A18372155` |
| Backup credential theft | `Veeam-Get-Creds-New.ps1` run against backup infrastructure |
| Persistence account | Local account `backup` created for durable access |
| Ransomware deployment mechanism | `PDQDeployService.exe` — attacker abused the legitimate PDQ Deploy software-distribution tool for fleet-wide push |
| Impact / ransom note | `rhddiicoE.README.txt` recovered, confirming encryption impact |

Detected-behavior tags surfaced directly by the platform: **T1204.002** (User Execution: Malicious File), **T1218.005** (System Binary Proxy Execution: Mshta), **T1055** (Process Injection, critical severity, remote thread created in MSBuild.exe by the trojanized installer).

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1189 – Drive-by Compromise; T1204.002 – User Execution: Malicious File |
| Defense Evasion | T1218.005 – System Binary Proxy Execution: Mshta; T1055 – Process Injection |
| Command and Control | T1071 – Application Layer Protocol |
| Credential Access | T1555 – Credentials from Password Stores (Veeam backup credentials) |
| Persistence | T1136 – Create Account |
| Execution (fleet-wide) | T1072 – Software Deployment Tools |
| Impact | T1486 – Data Encrypted for Impact |

## Outcome

Closed as **True Positive**, 10/10 tasks, 92.5% accuracy. Containment: isolate `CORP-WKS-102` and any host reached via the `backup` account, revoke the compromised PDQ Deploy credentials, rotate Veeam backup credentials, block `45.141.87.218` and `92.51.2.27`/`92.51.2.22` at the perimeter, and rebuild from clean, offline backups rather than attempting decryption.

## Takeaway

The abuse of a trusted internal software-deployment tool (PDQ Deploy) as the final delivery mechanism is what turned a single-host compromise into an enterprise-wide ransomware event — lateral-movement detection and privileged-tool monitoring matter as much as the initial-access defenses. The backup-credential theft step also mattered operationally: an attacker who compromises backup infrastructure before triggering encryption can sabotage the recovery path itself.
