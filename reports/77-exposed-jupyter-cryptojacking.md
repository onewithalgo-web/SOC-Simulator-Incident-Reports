# Exposed Jupyter Notebook: Cryptojacking a Data Server

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Verdict:** True Positive - 6/6 tasks | **Accuracy:** 100%

## Scenario

A Jupyter Notebook server on a data-science Linux host had its HTTP API left open to the internet on tcp/8888 with no token required. Scanners found it within hours. An attacker created a kernel, ran Python cells to execute shell commands, dropped a Monero miner, and beaconed to a stratum pool. The chain had to be rebuilt from the Linux host logs and the perimeter firewall.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| How external traffic reached the host | Port **8888** exposed to the internet |
| First hands-on probe | `185.155.0[.]91` |
| Code execution | `185.155.0[.]103` |
| Planted binary | `pyxmrig` |
| Egress destination | `xmr.pool.dataflux-relay[.]net` (the mining pool) |
| Execution technique | **T1059.006** - Command and Scripting Interpreter: Python |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Execution | T1059.006 - Command and Scripting Interpreter: Python |

## Takeaway

The server did what it documents: Jupyter's API runs code on request, so with no token the exposure itself was the vulnerability. The two addresses matter because they split the work between a probe and the operator who ran code, and the miner's pool domain is what shows up on the firewall once the host starts beaconing. A data server that should only talk to internal systems calling out to a mining pool is the cleanest early signal here.
