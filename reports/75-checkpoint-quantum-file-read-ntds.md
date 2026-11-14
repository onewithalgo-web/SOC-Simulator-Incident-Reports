# Path Traversal to NTDS: Check Point Quantum VPN Leak (CVE-2024-24919)

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Verdict:** True Positive - 6/6 tasks | **Accuracy:** 100%

## Scenario

An internet-facing Check Point Quantum security gateway was exploited through CVE-2024-24919, an unauthenticated path-traversal arbitrary file read. The attacker read the appliance's password-hash file and a VPN/LDAP configuration file, recovered the gateway's Active Directory bind credential, cracked it offline, then logged back in over the VPN as that account and extracted the Active Directory database from a volume-shadow copy on the domain controller. No malware touched the gateway.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Exploit source | `103.243.24[.]124` |
| Initial access technique | **T1190** - Exploit Public-Facing Application |
| Leaked credential | The AD bind account `svc-vpnldap`, recovered from the VPN/LDAP configuration |
| Credential reuse | VPN logon as that account from `125.20.210[.]182` |
| Directory theft technique | **T1003.003** - OS Credential Dumping: NTDS |
| Staged database indicator | `f1a7c93e0d4b86527a0c9f1e6d3b84a05c72e9d1b0a6f4837c25e0d9b6f1a473` |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 - Exploit Public-Facing Application |
| Credential Access | T1003.003 - OS Credential Dumping: NTDS |

## Takeaway

This is an intrusion with no malware to find. The path-traversal read, the offline crack and the later VPN logon all use legitimate features, so the detection surface is the pair of addresses and the account: an unauthenticated read of sensitive appliance files from one address, and the same service account then appearing from a different one. The reuse of an LDAP bind account for an interactive VPN logon is the point where it can be separated from normal activity.
