# Wide Open

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

An Elasticsearch node that should have stayed on the internal network had its API exposed to the internet on port 9200 with no authentication. Scanners found it within hours; one actor ran code on the host and dropped a cryptominer, a second wiped every index and left a ransom note.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Way in | TCP `9200` — unauthenticated Elasticsearch API |
| Hands-on operator | `45.61.139[.]22` |
| Code execution source | `89.234.157[.]99` |
| What got planted | `kdevtmpfsi` — XMRig miner |
| Where the money went | `pool.minexmr-relay[.]net` |
| Final act | **T1485** — Data Destruction |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Reconnaissance | T1595 – Active Scanning |
| Initial Access | T1190 – Exploit Public-Facing Application; T1133 – External Remote Services |
| Discovery | T1083 – File and Directory Discovery |
| Impact | T1496 – Resource Hijacking; T1485 – Data Destruction |

## Takeaway

Two unrelated actors on one host inside the same window is the signature of an exposure rather than a targeting — automated scanners sweep 9200 continuously, and whoever arrives second finds the door already open. The "ransom" note is worth scepticism: in the wipe-and-demand pattern the data is usually destroyed, not held, so recovery planning should assume the backup is the only copy.
