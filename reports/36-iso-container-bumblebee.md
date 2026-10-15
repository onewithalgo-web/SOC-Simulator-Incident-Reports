# Mounted and Loaded: ISO Container Delivery of the Bumblebee Loader

**Platform:** SOC Simulator | **Tools:** XDR, SIEM, Firewall, Query | **Duration:** ~1h | **Verdict:** True Positive — 8/8 tasks

## Scenario

An ISO image attachment hid a visible shortcut beside a hidden DLL. The user mounted the container and ran the shortcut, which executed the Bumblebee loader via `rundll32` against a named DLL export. Bumblebee injected into a signed Windows Mail binary through WMI, beaconed HTTPS to a C2 cluster, then layered a Meterpreter stager and a Cobalt Strike beacon. The operator ran AdFind discovery, dumped LSASS with ProcDump, created a rogue local admin, installed AnyDesk for fallback access, and moved laterally with a harvested domain administrator before the intrusion was contained pre-encryption.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| How attacker code first executed | `rundll32.exe E:\tamirlan.dll,EdHVntqdWt` |
| Hidden loader hash | `123f96ff0a583d507439f79033ba4f5aa28cf43c5f2c093ac2445aaebdcfd31b` |
| Masquerading step | **T1036** — Masquerading |
| Beacon command-and-control domain | `fuvataren[.]com` |
| Harvested administrator | `dag.fjellheim` |
| Fallback remote-access channel | **T1219** — Remote Access Software (AnyDesk) |
| Inbound operator address | `108.177.235[.]25` |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Defense Evasion | T1553.005 – Mark-of-the-Web Bypass; T1036 – Masquerading; T1218.011 – Rundll32 |
| Execution | T1204.002 – User Execution: Malicious File |
| Credential Access | T1003.001 – LSASS Memory |
| Lateral Movement | T1021.001 – Remote Desktop Protocol; T1021.002 – SMB/Admin Shares |
| Command and Control | T1071.001 – Web Protocols; T1219 – Remote Access Software |

## Takeaway

The ISO is doing one specific job: files extracted from a mounted image do not inherit Mark-of-the-Web, so the SmartScreen and Protected View prompts that would have fired on a raw attachment never appear. Containment landed before encryption, and the reason it could was that the operator's tooling — AdFind, ProcDump, AnyDesk — is all legitimate software whose presence on a finance workstation is itself the anomaly. Hunting for admin tooling in the wrong place beats hunting for malware.
