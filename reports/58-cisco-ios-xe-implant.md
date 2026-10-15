# Cisco IOS XE Web UI: Implant and Rogue Admin (CVE-2023-20198)

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~40m | **Verdict:** True Positive — 6/6 tasks

## Scenario

The Cisco IOS XE Web UI management interface on `AXFORD-RTR-01` was inadvertently exposed to the internet. An attacker exploited CVE-2023-20198 to create an unauthenticated privilege-15 local account, then chained CVE-2023-20273 to drop a Lua implant on the device flash filesystem. The implant hooked the HTTP server and beaconed to an external C2 address.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Exposure vector | TCP `443` — Web UI published to the internet |
| Initial access source | `185.123.0[.]91` |
| Rogue account | `cisco_admin` — privilege 15 |
| Implant | `cisco_service.lua` |
| Persistence classification | **T1505** — Server Software Component |
| Implant callback | `185.123.0[.]118` |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application; T1133 – External Remote Services |
| Persistence | T1505 – Server Software Component; T1098 – Account Manipulation |

## Takeaway

Network devices rarely carry endpoint telemetry, so the evidence available is syslog and what the perimeter saw — which makes the privilege-15 account creation event the anchor for the whole timeline. The implant lives in flash and hooks the HTTP server, so it does not survive a reboot but the rogue account does; cleaning one without the other leaves the way back in. Management planes belong on a dedicated interface, never on the public one.
