# Copyright Lure to Rhadamanthys Stealer

**Platform:** SOC Simulator | **Tools:** SIEM, XDR, Firewall | **Duration:** ~45m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A procurement officer opened a copyright-infringement notice, followed a shortened link, and ran a signed PDF reader out of a Downloads folder. The chain runs from a CDN-fronted delivery domain through a DLL search-order hijack and a Run-key autostart to the Rhadamanthys stealer's single command-and-control channel, where the beacon and the stolen data ride together.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Delivery domain | `cdn-docverify-pro[.]com` |
| What was actually run | `PdfViewerPortable.exe` — legitimately signed portable reader |
| Smuggled module | `msimg32.dll` |
| Execution technique | **T1574.001** — DLL Search Order Hijacking |
| Persistence | `DocVerifySync` — Run key |
| Command and control | `45.152.118[.]23` |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Spearphishing Link |
| Execution | T1204.002 – User Execution: Malicious File |
| Persistence / Defense Evasion | T1574.001 – DLL Search Order Hijacking; T1547.001 – Registry Run Keys |
| Command and Control | T1071.001 – Web Protocols |

## Takeaway

A copyright notice is a well-chosen lure for a procurement role — it carries implied legal urgency and a plausible reason to open an unfamiliar document reader. The technical pattern repeats across this portfolio: ship a genuinely signed executable, put the payload in a DLL beside it, and let Windows' own search order do the loading. The Run key is the cheap find; the DLL in a user-writable directory beside a signed binary is the one worth building a rule for.
