# MFA Fatigue: The Notification Flood

**Platform:** SOC Simulator | **Tools:** SIEM, XDR | **Duration:** ~30 min | **Verdict:** True Positive — 6/6 tasks, 36.25% accuracy

## Scenario

A modern identity-based attack against `nora.beckett` (host `corp-wks-042`, IP `10.0.2.219`): a phishing email disguised as a Dropbox share redirected her to an AiTM proxy, followed by an MFA-fatigue (push-bombing) attack, a foothold agent, lateral movement, and data exfiltration — all reconstructed from SIEM + XDR telemetry.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Kill-chain start | 14:22 phishing email (fake Dropbox notification) → 14:25 click → redirect via `drbx-share.com` to **`flowerstorm-auth.net`** (AiTM proxy) |
| MFA-fatigue technique | **T1621** — five denied push prompts followed by one approval from attacker IP `198.88.230.252`, in under 5 minutes |
| Foothold tool | `adversary-ai-agent.exe` deployed to `nora.beckett`'s Temp folder at 14:30, callback to `c2.flowerstorm-auth.net:8443` |
| Lateral-movement tool | `python.exe` executing `wmiexec.py` (Impacket) against `10.0.1.112:445` — staged via `certutil.exe` (LOLBin) pulling a portable Python runtime from `c2.flowerstorm-auth.net` |
| Pass-the-hash artifact | NTLM hash `aad3b435b51404eeaad3b435b51404ee:ccef208a86548b2a371ab851255ff093` replayed over SMB |
| Second compromised host | **`WS-MKT-02`** — confirmed successful lateral movement target (two other hosts, WS-HR-01 and WS-SALES-01, were attempted but denied) |
| Exfiltration | 4.2 GB of marketing data staged on WS-MKT-02 and exfiltrated to `103.112.22.236` |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Phishing: Spearphishing Link |
| Credential Access | T1557 – Adversary-in-the-Middle; **T1621 – Multi-Factor Authentication Request Generation** |
| Persistence / Defense Evasion | T1078.004 – Valid Accounts: Cloud Accounts |
| Lateral Movement | T1021.002 – SMB/Windows Admin Shares (wmiexec); T1550.002 – Pass the Hash |
| Command and Control | T1105 – Ingress Tool Transfer (certutil download) |
| Exfiltration | T1041 – Exfiltration Over C2 Channel |

## Outcome

Closed as **True Positive**, 6/6 tasks. Immediate containment executed in the walkthrough: isolate `corp-wks-042` and `WS-MKT-02`, block `198.88.230.252`, `103.112.22.236` and `flowerstorm-auth.net` at the perimeter/DNS, revoke Nora Beckett's sessions and force password + MFA re-enrollment, invalidate the compromised NTLM hash, and hunt for the `adversary-ai-agent.exe` hash and `wmiexec.py` execution pattern fleet-wide.

## Takeaway

This was my lowest-scoring operation (36.25%). I correctly identified the MFA-fatigue pattern and the overall kill chain, but was less precise pinning down the exact source-session correlation the platform expected as supporting evidence before moving to lateral-movement scoping. Recommended detection rule I'd now push for from this case: more than 3 MFA denials followed by 1 approval within 5 minutes → automatic high-severity alert, plus number-matching MFA to remove the simple-approve pattern the FlowerStorm kit relies on.
