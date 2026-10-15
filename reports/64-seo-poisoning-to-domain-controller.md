# Search Engine Poisoning: From a Sponsored Result to the Domain Controller

**Platform:** SOC Simulator | **Tools:** SIEM, XDR | **Duration:** ~1h | **Verdict:** True Positive — 8/8 tasks

## Scenario

An analyst at a mid-sized logistics firm searched for a routine software update, clicked the sponsored result at the top of the page, and went back to work. Five days later the SOC was holding a workstation, a file server, and a domain controller whose telemetry nobody had read. The evidence is five days of SIEM and endpoint data, most of which is an ordinary week — the work is separating the click's consequences from the noise.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| SEO ad callback domain | `update-service[.]com` |
| What the fake update page delivered | `Software_Update.iso` |
| Module run from the mounted image | `vcruntime140u.dll` |
| First dropped executable | `08207409e1d789aea68419b04354184490ce46339be071c6c185c75ab9d08cba` |
| File whose extension does not match its use | `powerranges.txt` |
| Second, distinct payload variant | `131da83b521f610819141d5c740313ce46578374abb22ef504a7593955a65f07` |
| Account behind the hands-on-keyboard phase | `itadm` |
| Odd binary in the final execution burst | `JustAskJacky.exe` |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Resource Development | T1608.006 – SEO Poisoning |
| Initial Access | T1189 – Drive-By Compromise |
| Execution | T1204.002 – User Execution; T1059.001 – PowerShell |
| Defense Evasion | T1218.011 – Rundll32; T1055 – Process Injection |
| Credential Access | T1003.001 – LSASS Memory |
| Execution / Impact | T1569.002 – Service Execution; T1490 – Inhibit System Recovery; T1486 – Data Encrypted for Impact |

## Takeaway

The five-day gap between click and impact is the real subject of this case. Everything that mattered was already in the telemetry on day one — an ISO mounted on a workstation, a DLL run from the mounted volume — and nothing looked at it until three systems were affected. Dwell time is not an attacker capability here; it is a detection-coverage gap. The `itadm` account marks the handover from automation to an operator, and that transition is the last cheap place to intervene before encryption.
