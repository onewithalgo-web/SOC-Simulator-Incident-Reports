# Apache Path Traversal: CVE-2021-41773 to RCE

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** SIEM, Firewall | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

An Apache 2.4.49 web server at Kestrel Analytics was targeted via CVE-2021-41773. The attacker used percent-encoded path traversal to read `/etc/passwd`, confirmed `mod_cgi` was enabled, and escalated to remote code execution, leaving a shell behind.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Traversal request | `/cgi-bin/.%2e/.%2e/.%2e/.%2e/etc/passwd` |
| Who escalated to command execution | `45.104.0[.]91` |
| Webshell planted | `stats.sh` |
| Service account running the shell | `www-data` |
| Initial access classification | **T1190** — Exploit Public-Facing Application |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Discovery | T1083 – File and Directory Discovery |
| Execution | T1059 – Command and Scripting Interpreter |
| Persistence | T1505.003 – Web Shell |

## Takeaway

The `%2e` encoding is the whole bug — Apache 2.4.49 normalized the path after the traversal check rather than before it, so an encoded dot slipped through a filter that would have caught a literal one. File read alone is serious; file read plus `mod_cgi` is code execution. The practical hunt is for 200 responses to URLs containing encoded dot sequences, which no legitimate client generates.
