# UNC6384: Captive-Portal PlugX Implant

**Platform:** SOC Simulator | **Tools:** SIEM, XDR, Firewall | **Duration:** ~30m | **Verdict:** True Positive — 7/7 tasks

## Scenario

A diplomat's managed laptop on an untrusted conference network had its captive-portal check hijacked and was steered to a page posing as a security update. The download was a signed Canon utility paired with a malicious DLL: running the trusted binary side-loaded the DLL, which ran PlugX in memory, beaconed to a single HTTPS host, and set a Run key.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Where the captive-portal check was redirected | `mediareleaseupdates[.]online` |
| Malicious DLL side-loaded | `cnmpaui.dll` |
| Execution classification | **T1574.001** — DLL Search Order Hijacking |
| Command-and-control host | `45.156.40[.]21` |
| Persistence | `CanonPrinter` — Run key |
| Data exfiltration classification | **T1041** — Exfiltration Over C2 Channel |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1189 – Drive-By Compromise; T1566.002 – Spearphishing Link |
| Defense Evasion / Persistence | T1574.002 – DLL Side-Loading; T1547.001 – Registry Run Keys |
| Command and Control | T1071.001 – Web Protocols |
| Exfiltration | T1041 – Exfiltration Over C2 Channel |

## Takeaway

Hijacking the captive-portal connectivity check is an elegant delivery choice: every laptop makes that request automatically on joining a network, so no user action is needed to put the attacker's page in front of them. For a travelling-executive threat model the control is not detection but posture — always-on VPN before any network trust decision, so the connectivity probe never resolves against the hostile network in the first place.
