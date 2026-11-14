# MSP360 and ScreenConnect: Phishing Installer to Dual-RMM Persistence

**Platform:** SOC Simulator | **Tools:** Email, XDR, SIEM, Firewall | **Verdict:** True Positive - 5/5 tasks | **Accuracy:** 100%

## Scenario

Overnight hunting flagged two signed remote-management agents calling home from one advisory associate's workstation, both installed within the same hour the previous morning, with PowerShell running under one of them. The user said they had opened a document-review link. IT runs a sanctioned remote-support tool elsewhere, so at first read it looked like a rollout. The email, endpoint, SIEM and firewall evidence had to show how the agents arrived and what they were used for.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Delivery | **T1566.002** - Phishing: Spearphishing Link, traced to one message in the email panel |
| What asked for elevation | File with SHA-256 `108ef7e628d7a20bd6241a5b57149e27a6061f467123eb64061975559f8f73dc` |
| Persistence | **T1543.003** - Create or Modify System Process: Windows Service, so the MSP360 agent restarts after a reboot |
| Second channel's relay | `adswre[.]cfd`, the external host the ScreenConnect client connects to |
| Inbound rule opened | Port `48678`, allowed in the same minute the MSP360 agent was set up |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 - Phishing: Spearphishing Link |
| Persistence | T1543.003 - Create or Modify System Process: Windows Service |

## Takeaway

Both agents are real, signed remote-support products, which is exactly why this looks like a rollout at first glance. What separated it from IT's sanctioned tool was the context: an installer reached through a phishing link, an elevation prompt from a file the user had just run, a service set up to survive reboots, and a firewall rule opened the same minute. With two independent remote channels on one desk, removing one agent would not have evicted the intruder.
