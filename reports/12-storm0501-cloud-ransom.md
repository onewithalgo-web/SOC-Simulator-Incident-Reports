# Storm-0501: Azure Storage Account Key Abuse and Cloud Ransom

**Platform:** SOC Simulator | **Tools:** SIEM, Cloud | **Duration:** ~1h 20m | **Verdict:** True Positive — 8/8 tasks, 69.05% accuracy | **First Blood**

## Scenario

A cloud-native ransomware operation against a hybrid Microsoft Entra ID tenant, delivered entirely through the Azure control plane with no encryptor binary. In five hours the operator takes over the tenant, plants persistence that survives password resets, steals 768 GB, destroys every recovery path, re-encrypts the survivors with a key the company does not hold, and extorts the business over Teams. This was the first completion of the operation on the platform.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Identity that opened the cloud | `svc-aadc-sync` — the Entra Connect directory-sync account, holding Global Administrator, MFA-exempt, signing in interactively from `109.100.14[.]222` at 01:12:39 with no MFA challenge |
| Cloud persistence backdoor | `kaldberg-sync-trust[.]com` — added as a federated domain with an attacker token-signing certificate, able to mint SAML tokens for any user |
| Data-plane key abuse | **T1528** — Steal Application Access Token (storage account `listKeys` after self-assigning Owner at subscription scope) |
| Exfiltration channel | `103.191.14[.]243` — SharedKey bulk read of the `freight-archive` container via AzCopy/10.25.1, ~768 GB egressed between 03:22 and 04:51 |
| Recovery-destruction step | Recovery Services vault `rsv-kaldberg-dr-ne`, deleted alongside snapshots and restore points |
| Encryption-for-impact | **T1486** — Data Encrypted for Impact (customer-managed key in an attacker key vault applied to `kaldbergprodblob01`) |
| Extortion attribution | `harriet.voss` — impersonated via the federation trust, used to post the ransom message in Teams |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access / Persistence / Privilege Escalation / Defense Evasion | T1078.004 – Valid Accounts: Cloud Accounts |
| Persistence / Privilege Escalation | T1098.003 – Additional Cloud Roles; T1484.002 – Trust Modification (federated domain) |
| Credential Access | T1528 – Steal Application Access Token; T1606.002 – Forge Web Credentials: SAML Tokens |
| Exfiltration | T1537 – Transfer Data to Cloud Account |
| Impact | T1490 – Inhibit System Recovery; T1486 – Data Encrypted for Impact; T1657 – Financial Theft |

## Outcome

Closed as **True Positive**, 8/8 tasks, with five Critical detection rules fired: directory-sync identity signing in interactively, federated domain added, subscription Owner assignment, storage key listing followed by SharedKey external read, and backup destruction preceding an encryption change.

## Takeaway

Two lessons carry beyond the scenario. First, ransomware no longer needs ransomware: every impact action here was a legitimate Azure API call, so a detection strategy built on malicious binaries would have seen nothing. Second, the four attacker addresses are all flagged by multiple vendors — yet blocking them would have been closing the door after each stage, while the federated domain that gave the operator permanent, identity-independent access is flagged by nobody. What catches this is configuration-state monitoring: an identity that should never sign in interactively doing so, a new federated domain, an Owner assignment, a `listKeys` call followed by a SharedKey read from the internet.
