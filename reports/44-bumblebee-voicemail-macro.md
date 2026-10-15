# Bumblebee Returns: The Voicemail VBA Macro

**Platform:** SOC Simulator | **Tools:** SIEM, Email, Firewall, XDR | **Duration:** ~30m | **Verdict:** True Positive — 7/7 tasks

## Scenario

An accounts-payable clerk opened a voicemail-notification email, followed a OneDrive link, and a Word macro quietly pulled a loader onto the host. The chain runs from a sender-spoofed phishing message through a VBA macro, PowerShell, and signed-binary proxy execution to the Bumblebee loader's TLS command-and-control.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Delivery | `voicemail.svc@halloranfreight-billing[.]com` |
| Lure document | `ReleaseEvans#96.docm` |
| Macro handoff | `powershell.exe` |
| Loader | `w_ver.dll` |
| Channel to cut | `vx41tq0p[.]life` |
| Proxy-execution classification | **T1218.007** — Msiexec |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Spearphishing Link |
| Execution | T1204.002 – User Execution; T1059.001 – PowerShell |
| Defense Evasion | T1218.007 – Msiexec; T1218.011 – Rundll32 |
| Command and Control | T1105 – Ingress Tool Transfer; T1071.001 – Web Protocols |

## Takeaway

The macro arrives by link rather than attachment precisely so the document is fetched from a cloud-storage domain the gateway trusts, and so Mark-of-the-Web handling depends on how the file was saved. `msiexec` reaching out to a `.life` domain is the point in the chain with the best signal-to-noise: proxy execution via signed Windows binaries is common tradecraft, but the destination is not something an installer should be contacting.
