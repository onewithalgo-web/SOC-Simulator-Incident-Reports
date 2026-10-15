# Exposed .git Folder: Scanning the Web for Secrets

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** SIEM, Firewall | **Duration:** ~25m | **Verdict:** True Positive — 7/7 tasks

## Scenario

A public web server at Larkspur Logistics was deployed straight from a git checkout, leaving its `.git` directory exposed to the internet. Following the EMERALDWHALE playbook, an attacker pulled `/.git/config`, stole the GitHub token baked into the clone URL, cloned the private repository, and found a hard-coded AWS key inside that handed them the cloud account.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Dotfile that should have 404'd | `/.git/config` |
| Whose key it gave away | `marcus.feldman` — GitHub PAT embedded in the clone URL |
| Who used the stolen token | `49.64.242[.]249` |
| Cloud key hidden in the source | `AKIA5RZ7QF2J6XW3LM8D` |
| Rogue account left behind | `mailer-sns-smtp` |
| Credential-theft classification | **T1552.001** — Credentials in Files |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Reconnaissance | T1595.003 – Wordlist Scanning; T1592 – Gather Victim Host Information |
| Credential Access | T1552.001 – Credentials in Files; T1552.007 – Container API |
| Initial Access / Persistence | T1078 – Valid Accounts |

## Takeaway

One deployment shortcut produced a three-stage credential cascade: web server to source repository to cloud account. Each step used a credential that was valid, so nothing exploited anything. The structural lesson is that secrets in a repository are secrets in every copy of that repository — the IAM user created at the end (`mailer-sns-smtp`) is the only artefact that an attacker had to create rather than find.
