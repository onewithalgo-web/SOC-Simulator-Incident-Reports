# QakBot bb02: Trace the Loader DLL to its C2

**Platform:** SOC Simulator | **Tools:** SIEM, XDR, Firewall | **Duration:** ~30m | **Verdict:** True Positive — 6/6 tasks, 60% accuracy

## Scenario

A purchasing coordinator opens a phishing email, downloads a password-protected archive, and runs a shortcut on the disk image hidden inside it. A signed Windows utility quietly registers a QakBot DLL, and the workstation starts beaconing to addresses nobody recognises.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Delivery | `invoices@sapplus.net` — failed sender authentication, pointed at a password-protected archive |
| Execution handoff | `regsvr32.exe` — a signed Microsoft utility, run with the silent switch |
| Payload | `tdfasdf.dat` — a loader DLL disguised with a `.dat` extension in the user's Temp folder |
| C2 beacon | `185.177.34[.]91:443` (`sxeuiqecowj[.]net`) — reached after five refused TLS attempts to bare addresses |
| Execution-proxy classification | **T1218.010** — System Binary Proxy Execution: Regsvr32 |

Delivery chain in the XDR process tree: Outlook → browser download → `7zFM.exe` opening `Orig1510220021.zip` → `cmd.exe /c E:\qbot.cmd` from the mounted disk image → `regsvr32.exe /s`.

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.001 – Phishing: Spearphishing Attachment |
| Execution | T1204.002 – User Execution: Malicious File |
| Defense Evasion | T1218.010 – System Binary Proxy Execution: Regsvr32; T1027 – Obfuscated Files or Information |
| Command and Control | T1071.001 – Application Layer Protocol: Web Protocols; T1573 – Encrypted Channel |

## Outcome

Closed as **True Positive**, 6/6 tasks, with five detection rules fired including "Session-Cycle-To-Bare-Addresses" — a single process opening sessions to a run of unresolved addresses, most refused.

## Takeaway

Reputation data failed completely here. The live C2 is unflagged by every VirusTotal vendor, its algorithmically generated domain is unflagged, and the executing process was Microsoft's own `regsvr32.exe` with a clean hash — an analyst working indicator-first would have found nothing to act on. The chain was exposed by structure instead: a signed utility running from a user directory with a silent switch, a shortcut on a freshly mounted image, and a process touching six addresses without a single DNS lookup. Those are properties of how the attack works rather than of who is running it, which is why the same detections will catch the next wave under different infrastructure.
