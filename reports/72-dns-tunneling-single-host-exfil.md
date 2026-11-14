# DNS Tunneling: Covert Exfiltration from One Host

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Verdict:** True Positive - 6/6 tasks | **Accuracy:** 45.83%

## Scenario

A single workstation at Valkara Analytics had been leaking data all day through a DNS tunnel. Every query carried encoded payload chunks inside high-entropy subdomain labels, sent to an attacker-controlled name server. The task was to rebuild the tunnel from resolver and firewall logs, name the compromised host and the attacker's infrastructure, and map the technique.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Anomalous host | `10.116.30.31`, resolving at a rate far outside normal |
| Tunnel domain | `sync-updates[.]net` |
| Attacker's name server | `45.156.0[.]12` |
| Dominant query type | **TXT**, which lets data travel in both directions |
| Tunnel queries counted over the day | 312 |
| Primary technique | **T1048.003** - Exfiltration Over Alternative Protocol: Exfiltration Over Unencrypted Non-C2 Protocol |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Exfiltration | T1048.003 - Exfiltration Over Unencrypted Non-C2 Protocol |

## Takeaway

DNS tunnelling hides in a protocol every network allows, so the useful detections are about shape rather than content: one host producing far more queries than its peers, long random-looking subdomain labels under a single parent domain, and an unusual record type such as TXT dominating the session. Any one of those is a weak signal. All three on one host, against one domain, is a tunnel.
