# Jenkins Arbitrary File Read: CVE-2024-23897 to RCE

**Platform:** SOC Simulator | **Tools:** SIEM, XDR, Query | **Duration:** ~45m | **Verdict:** True Positive — 7/7 tasks

## Scenario

The Jenkins controller at Vetrina Systems was running a version vulnerable to CVE-2024-23897 — an args4j parser flaw that lets the CLI read any file from the server filesystem before authentication completes. An attacker used it to steal the Jenkins master key and credential store, decrypted stored service-account passwords offline, then authenticated and ran Groovy code through the Script Console to drop a persistent payload and pivot to connected build agents.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Initial access vector | `45.124.200[.]88` |
| What was taken from the filesystem | `/var/jenkins_home/secrets/master.key` |
| Credential use | `svc-ci-deploy` |
| Execution method | **T1059.007** — Groovy via the Script Console |
| Compromised build agent | `VETRINA-AGT-01` |
| Command-and-control destination | `45.124.200[.]133` |
| Initial credential exposure | **T1552.001** — Credentials in Files |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Credential Access | T1552 – Unsecured Credentials; T1552.001 – Credentials in Files |
| Privilege Escalation | T1078 – Valid Accounts |
| Discovery | T1083 – File and Directory Discovery |
| Execution | T1059.007 – JavaScript/Groovy |

## Takeaway

A pre-authentication file read is only as serious as what the filesystem holds, and a CI controller holds the keys to everything it deploys. The decryption happened offline, so there was no failed-login noise between theft and legitimate-looking authentication — the detection window is the CLI requests themselves, not the login. Build agents make the blast radius organizational rather than local, which is why CI infrastructure deserves the same segmentation as a domain controller.
