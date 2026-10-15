# WordPress Plugin RCE: Webshell on a Vulnerable Site

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** SIEM | **Duration:** ~25m | **Verdict:** True Positive — 7/7 tasks

## Scenario

A media company's WordPress server was compromised after an attacker exploited an unpatched file-upload plugin to drop a PHP webshell into the public uploads directory. The investigation walks the Apache access logs and IDS alerts, tracing the probe, the upload bypass, and the command execution that followed.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Scanner | `185.101.0[.]47` |
| Exploited upload handler | `/wp-content/plugins/media-share-pro/upload-handler.php` |
| Webshell file | `thumb_cache.php` |
| Source executing commands | `185.101.0[.]118` |
| First OS command | `id` |
| Initial access classification | **T1190** — Exploit Public-Facing Application |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Persistence | T1505.003 – Web Shell |
| Execution | T1059 – Command and Scripting Interpreter |
| Initial Access | T1133 – External Remote Services |
| Discovery | T1087 – Account Discovery |

## Takeaway

The filename is the tell: `thumb_cache.php` is designed to look like plugin-generated clutter in a directory that legitimately fills with machine-named files. The durable control is not patching speed but configuration — `wp-content/uploads` should not execute PHP at all, which makes the upload harmless regardless of which plugin allows it. `id` as the first command is the standard operator orientation step and a reliable marker of the shift from automation to hands-on-keyboard.
