# Malicious npm Package: Postinstall Infostealer

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** XDR, SIEM | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A developer ran a routine `npm install` and the terminal returned clean. Less than a minute later the web proxy recorded a large outbound upload from that workstation. The investigation works the process tree, the file activity, and the outbound traffic to reconstruct what the install actually did.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Malicious package | `chalck` — typosquat of `chalk` |
| What executed the postinstall script | `node.exe` |
| File staged on disk | `.npm_telemetry` |
| Exfiltration destination | `cdn-metrics.build-telemetry[.]io` |
| Initial access classification | **T1195.001** — Supply Chain Compromise: Software Dependencies |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1195.001 – Compromise Software Dependencies and Development Tools |
| Execution | T1059.007 – JavaScript |
| Credential Access | T1552.001 – Credentials in Files |
| Exfiltration | T1567 – Exfiltration Over Web Service |

## Takeaway

The `postinstall` hook is arbitrary code execution as a documented feature of the package manager, and it runs with the developer's full environment — cloud credentials, SSH keys, `.npmrc` tokens. One letter separated the real package from the typosquat. `npm ci --ignore-scripts` plus an internal registry proxy removes the whole class; absent that, the detection is a `node` process writing a dotfile and immediately uploading it.
