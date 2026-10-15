# Kernel Thread Impostor

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

An internet-exposed Linux node fell to an SSH password brute force, and within seconds the Outlaw (Dota3) toolkit dropped a hidden cryptominer disguised as a kernel thread. The case is reconstructed from auth logs, process records, and perimeter traffic.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Breached account | `suporte` |
| Brute-force source | `193.27.14[.]66` |
| Where the toolkit was staged | `~/.configrc6` |
| Cryptominer process | `kswapd0` — impersonating a kernel thread |
| Mining destination | `pool.minexmr-relay[.]net` |
| Objective classification | **T1496** — Resource Hijacking |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Credential Access | T1110.001 – Password Guessing |
| Initial Access | T1078 – Valid Accounts |
| Execution | T1059.004 – Unix Shell |
| Persistence | T1053.003 – Cron |
| Lateral Movement | T1021.004 – SSH |
| Impact | T1496 – Resource Hijacking |

## Takeaway

`kswapd0` is a real kernel thread, so the name passes a glance at `ps` — but a genuine kernel thread has no parent userland process and no network sockets. That distinction is the detection, and it costs nothing to check. The account name `suporte` also says something about the attack: this was a dictionary tuned to common service-account names, not a targeted guess.
