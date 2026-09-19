# Overdue Invoice, Compromised Endpoint: Tracing a Finance Workstation Intrusion

**Platform:** SOC Simulator | **Tools:** SIEM, Email | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks, 70% accuracy

## Scenario

A finance clerk works through her inbox on a Tuesday morning. Within seventeen minutes of one message arriving, her workstation is running hidden PowerShell, has staged a payload in a world-writable directory, has installed itself in the registry to survive a reboot, and is talking to an external server. The whole intrusion is four records across two log sources.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| What the message delivered | `INVOICE#BUSAPOMKDS03.lnk` — a Windows shortcut inside `INVOICE_8841.zip`, from `billing@invoices-bwlogistics-pay.com` (spf=fail, dmarc=fail, delivered) |
| What the first stage put on disk | `UHCYbG.cab` — a script, not a cabinet archive, staged in `C:\Users\Public\` and read via `IEX(Get-Content)` |
| How it survives a reboot | Run value **`svchostno2`** under `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`, pointing at `C:\Users\Public\49120862.bat` |
| Where the host phoned home | `goosess[.]com/read/get.php`, resolved `198.235.24.58:443` |
| Trigger classification | **T1204.002** — User Execution: Malicious File |

The PowerShell command line was obfuscated with caret escapes (`i^e^x`, `g^c`), which Windows strips before execution.

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.001 – Phishing: Spearphishing Attachment |
| Execution | T1204.002 – User Execution: Malicious File; T1059.001 – PowerShell |
| Defense Evasion | T1027 – Obfuscated Files or Information; T1036.005 – Masquerading: Match Legitimate Name or Location |
| Persistence | T1547.001 – Registry Run Keys |
| Command and Control | T1071.001 – Application Layer Protocol: Web Protocols |

## Outcome

Closed as **True Positive**, 6/6 tasks. VirusTotal confirms the C2 domain (11/89) and its hosting address (12/89), the latter inside Google Cloud address space.

## Takeaway

Process lineage carried this case. `parent=explorer.exe` on the PowerShell process is the single most informative field in the whole incident: it says a human double-clicked something, which separates this immediately from a scheduled task or service compromise and points the investigation at the mailbox. Add the two locations — `C:\Users\Public` for staging and `HKCU` for persistence — and the attacker's constraint becomes clear: they had no administrator rights and did not need them. The persistence value name, imitating the Windows service host, is designed to be skimmed past in a list of startup entries.
