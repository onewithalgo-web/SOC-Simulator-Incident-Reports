# FluBot: The Parcel-Delivery Text That Spreads Itself

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** SIEM, Firewall | **Duration:** ~15m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A managed Android handset at Larkfield Mutual was infected with FluBot after the employee tapped a smishing SMS impersonating a DHL parcel notice. The fake tracking page talked the user into sideloading an app and granting Accessibility and SMS permissions; from there the trojan turned the phone into a sender — harvesting contacts, texting the same lure onward, and intercepting bank 2FA codes — while keeping a command channel the perimeter firewall never blocked.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Link the victim tapped | `dhl-parcel-tracking.bodrumenduro[.]com` — compromised legitimate site |
| Sideloaded app | `DHL47.apk` |
| Where the stolen contacts went | `wkqmxlbtpnvdcaup[.]ru` — DGA-style domain |
| How far it tried to spread | 214 outbound lure messages |
| How the C2 stayed hidden | **T1572** — Protocol Tunneling (DNS) |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1660 – Phishing (mobile); T1456 – Drive-By Compromise |
| Persistence / Defense Evasion | T1582 – SMS Control |
| Collection | T1636.003 – Protected User Data: Contact List; T1437.001 – Web Protocols |
| Command and Control | T1572 – Protocol Tunneling; T1521.002 – Asymmetric Cryptography |

## Takeaway

The firewall never blocked the command channel because the command channel was DNS. That is the detail worth carrying forward: an egress policy that allows unrestricted outbound DNS from a user VLAN has an unmonitored bidirectional channel in it. The 214 outbound messages also make the business case — this infection's damage was mostly inflicted on the organization's contacts, not on the organization.
