# Purchase Order, Poisoned

**Platform:** SOC Simulator | **Tools:** Email, XDR, SIEM | **Duration:** ~30m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A sales coordinator opened a malspam "purchase order" and a finance workstation went quiet, then started talking to the internet. The chain runs from a sender-spoofed email through an Equation Editor exploit, a rundll32 loader, Run-key persistence, and a hollowed system process to the FormBook stealer's HTTP command-and-control.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Delivery | `billing@dispatch-orders-intl[.]com` |
| Exploited binary | `EQNEDT32.EXE` — Equation Editor |
| Loader | `vbrtxh64.dll` |
| Process wearing a disguise | `ImagingDevices.exe` — hollowed |
| Command-and-control channel | `www.tk7q-status[.]com` |
| Persistence classification | **T1547.001** — Registry Run Keys |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.001 – Spearphishing Attachment |
| Execution | T1204.002 – User Execution; T1203 – Exploitation for Client Execution; T1218.011 – Rundll32 |
| Persistence | T1547.001 – Registry Run Keys |
| Defense Evasion | T1055.012 – Process Hollowing |
| Credential Access | T1555.003 – Credentials from Web Browsers |
| Command and Control | T1071.001 – Web Protocols |

## Takeaway

`EQNEDT32.EXE` spawning a child process is as close to a zero-false-positive detection as endpoint telemetry offers — the component was retired years ago and has no legitimate reason to launch anything. Everything after it (rundll32, hollowing, the Run key) is noisier. When a chain contains one step that is categorically abnormal, that is where the detection belongs, not at the end.
