# GoldPickaxe: The First iOS Trojan Stealing Your Face

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall, Query | **Duration:** ~45m | **Verdict:** True Positive — 7/7 tasks

## Scenario

A mobile-first intrusion against a retail bank's managed iPhone fleet. A relationship officer was socially engineered into installing a fake government app through Apple TestFlight and trusting a rogue MDM profile, handing GoldFactory full control of the device. The GoldPickaxe.iOS trojan harvested identity documents and a facial-recognition video, intercepted SMS, and exfiltrated over three split channels — an RSA-encrypted HTTP API, a WebSocket command channel, and an RTMP face-video stream — to enable AI face-swap fraud against the bank's own facial verification.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Delivery host | `ms2ve[.]cc` |
| Trojan file indicator | `4571f8c8560a8a66a90763d7236f55273750cf8dd8f4fdf443b5a07d7a93a3df` |
| Data-upload C2 host | `61.151.249[.]194` |
| Face-video upload channel (separate from command) | `18.142.163[.]73` — RTMP stream |
| Runtime-code fetch classification | **T1407** — Download New Code at Runtime |
| Encrypted-upload classification | **T1521.002** — Asymmetric Cryptography |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1660 – Phishing (mobile) |
| Defense Evasion | T1456 – Drive-By Compromise; T1407 – Download New Code at Runtime |
| Collection | T1636.004 – Protected User Data: SMS Messages; T1437.001 – Application Layer Protocol: Web Protocols |
| Command and Control | T1521.002 – Encrypted Channel: Asymmetric Cryptography |

## Takeaway

Three separate egress channels for one implant is a deliberate design: splitting command, structured data, and the video stream means no single block kills the operation, and the RTMP flow looks nothing like malware traffic. The bank's exposure was not the phone — it was that a facial-recognition video is a reusable authentication factor once stolen. Biometrics that cannot be revoked should not be the only thing standing between an attacker and a payment.
