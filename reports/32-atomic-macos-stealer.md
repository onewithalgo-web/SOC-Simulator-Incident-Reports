# Fake Browser Update to Atomic macOS Stealer

**Platform:** SOC Simulator | **Tools:** XDR, SIEM | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A freelance designer's Mac was lured by a ClearFake "your browser is out of date" prompt into downloading a fake Safari update. The bundled app was Atomic macOS Stealer: it phished the login password through a native-looking dialog, raided the keychain and browser stores, and uploaded the loot to a single host over the same channel it used to communicate.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| How the machine was lured | `macos-updatecheck[.]live` |
| File downloaded | `SafariUpgrade.dmg` |
| What executed | `SafariUpgrade` |
| Credential theft | **T1555.001** — Credentials from Password Stores: Keychain |
| Where the stolen data went | `185.151.40[.]23` |
| Exfiltration classification | **T1041** — Exfiltration Over C2 Channel |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Execution | T1204.002 – User Execution: Malicious File |
| Credential Access | T1555.001 – Keychain; T1555.003 – Credentials from Web Browsers |
| Command and Control | T1071 – Application Layer Protocol |
| Exfiltration | T1041 – Exfiltration Over C2 Channel |

## Takeaway

macOS stealers do not need an exploit because the keychain will hand over everything to a user who types their password — so the malware simply asks, in a dialog that looks like the system's own. There is no privilege escalation step to detect. What remains detectable is the shape of the session: one host receiving a single large upload immediately after an unsigned app launched from `~/Downloads`.
