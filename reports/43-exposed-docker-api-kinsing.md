# Daemon in the Dark

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall, Cloud audit | **Duration:** ~30m | **Verdict:** True Positive — 5/5 tasks

## Scenario

A Linux Docker host quietly started mining cryptocurrency overnight. Its container-engine API was open to the internet, and an automated intruder used it to launch a host-mounting container, drop the Kinsing malware and an XMRig miner, kill the defences, and attempt to spread.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Way in | TCP `2375` — unauthenticated Docker API exposed to the internet |
| Supply drop | `45.61.139[.]24` |
| Implant | `kinsing` |
| Where the money went | `193.201.224[.]96` — mining pool |
| Objective classification | **T1496** — Resource Hijacking |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Execution | T1610 – Deploy Container; T1059.004 – Unix Shell |
| Lateral Movement | T1685.001 – SSH Hijacking |
| Command and Control | T1071.001 – Web Protocols |
| Impact | T1496 – Resource Hijacking |

## Takeaway

Port 2375 is the Docker daemon with TLS disabled, and anyone who can reach it can start a container that mounts the host filesystem — which is root on the host by another name. No vulnerability was exploited; the API did what it is documented to do. Internet-wide scanners find these within hours of exposure, so the window between misconfiguration and compromise is measured in hours, not weeks.
