# StealC: Cheat Tool to Emptied Wallet

**Platform:** SOC Simulator | **Tools:** SIEM, XDR | **Duration:** ~20 min | **Verdict:** True Positive — 6/6 tasks, 100%

## Scenario

A gamer chasing a free aimbot ran a fake cheat-tool installer, and within the hour the endpoint was beaconing outbound and pinning its CPU. Single unmanaged Windows desktop on a home network (no corporate domain) — host `DESKTOP-K4P7N2X`, user `tyler.brennan`.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Lure | Browser visited `entrarium[.]live`, advertising a free cheat, before any executable touched disk |
| Delivery | `setup.exe` executed from `C:\Users\tyler.brennan\Downloads\Entrarium\setup.exe` |
| Sample hash | SHA-1 `764a05c79d5baa853bc29780f700a0a426faedfa` |
| Persistence | Run-key value `Entrarium` written for autostart persistence |
| Collection | Installer read Chromium credential/cookie databases and cryptocurrency wallet files |
| Exfiltration | Staged archive uploaded in one large outbound transfer to `185.171.244[.]93` |
| Second stage | `cmd.exe` pulled `addon.exe` (SHA-1 `7f9af7ca36d2e539414a3d9eb5a2a96d72abf8df`) and `addon2.exe` (SHA-1 `667fc2c7e7aa428dd6a35e32ac979d910b740684`) from a separate payload host (`193.106.191.142`) |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1204.002 – User Execution: Malicious File |
| Persistence | T1547.001 – Boot or Logon Autostart Execution: Registry Run Keys |
| Credential Access | T1555 – Credentials from Password Stores |
| Collection | T1115 – Clipboard Data; T1005 – Data from Local System |
| Exfiltration | T1041 – Exfiltration Over C2 Channel |
| Impact | **T1496 – Resource Hijacking** (confirmed classification for the second-stage payload — a cryptojacker) |

## Outcome

Closed as **True Positive**, 6/6 tasks correct. Containment: isolate the host, rotate every credential and wallet key that was resident on the machine, remove the `Entrarium` Run-key entry, and block `entrarium[.]live`, `185.171.244.93` and `193.106.191.142` at the perimeter.

## Takeaway

Cracked/cheat-tool downloads remain one of the highest-yield social-engineering lures because the user is already primed to run an "untrusted" executable — user-awareness controls matter as much as endpoint detection here. The second-stage cryptojacker is a reminder that a stealer payload is often not the only monetization attempt on a compromised host.
