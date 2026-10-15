# The Template That Read the Disk

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** SIEM, Firewall | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

The file-transfer portal that Tideglow Logistics' partners upload to spent a morning returning host files it was never meant to publish, and finished it running an administrator session nobody had logged into. The root cause is CVE-2024-4040 in CrushFTP 10.6.0 — a server-side template injection reachable without credentials.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Template-injection payload | `<INCLUDE>/etc/passwd</INCLUDE>` |
| File that gave up the session tokens | `sessions.obj` |
| Where the admin session was replayed from | `218.145.181[.]48` |
| Account the attacker became | `crushadmin` |
| Initial access classification | **T1190** — Exploit Public-Facing Application |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Discovery | T1083 – File and Directory Discovery; T1087 – Account Discovery |
| Privilege Escalation | T1078.001 – Valid Accounts: Default Accounts |
| Exfiltration | T1041 – Exfiltration Over C2 Channel |

## Takeaway

An arbitrary file read becomes an authentication bypass the moment the application stores its session state on disk. The attacker never guessed or cracked anything: `sessions.obj` handed over a live administrator token. Managed file transfer appliances concentrate exactly this risk — they are internet-facing by design, they hold partner data, and their session stores are flat files.
