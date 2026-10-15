# Anatsa Banker: The Trojan Hiding in a Google Play PDF Reader

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** SIEM, Firewall | **Duration:** ~20m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A relationship manager at Larkfield Mutual installed a five-star PDF and QR reader from the Google Play store onto a managed Android handset. It was an Anatsa (TeaBot) dropper: it staged a DEX payload disguised as an app update, sidestepped sandbox checks, downloaded the final banking trojan, and abused the Accessibility service to overlay a fake bank login.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| App that started it | `com.appandutilitytools.fileqrutility` — published on Google Play |
| Host the dropper phoned home to | `menusand[.]com` |
| Final payload fingerprint | `cb02f9e5a5671e3f13bc26d3017b8632` |
| Command-and-control callback | `185.215.113[.]31` |
| Chain classification | **T1407** — Download New Code at Runtime |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1660 – Phishing (mobile); T1456 – Drive-By Compromise |
| Defense Evasion | T1407 – Download New Code at Runtime; T1626.001 – Abuse Elevation Control Mechanism |
| Collection | T1417.001 – Input Capture: Keylogging; T1437.001 – Web Protocols |
| Command and Control | T1521.002 – Asymmetric Cryptography |

## Takeaway

The app that cleared Play review was genuinely benign — the banking trojan arrived afterwards as a runtime download, which is the whole point of the dropper pattern. Store provenance is therefore not a control for managed devices; the detectable event is a freshly installed app fetching executable code from a host unrelated to its publisher, followed by an Accessibility-service grant.
