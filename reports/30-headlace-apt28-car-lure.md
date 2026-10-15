# Fighting Ursa: Car-for-Sale Lure to HeadLace

**Platform:** SOC Simulator | **Tools:** XDR, Email, SIEM | **Duration:** ~30m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A diplomatic attaché clicked a link offering a discounted vehicle for sale, and a trusted Windows program started behaving like malware. The chain runs from a spearphishing link through an abused web-service redirector and a staged archive to a side-loaded HeadLace loader executing under a legitimate signed binary. Activity consistent with Fighting Ursa (APT28).

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Way in | `https://webhook[.]site/b7d41e9a-3c52-4af0-8e16-9d2f7c64a1be` — legitimate service abused as redirector |
| Payload archive | `Audi-Q7-quattro.zip` |
| Trusted decoy | `Calc.exe` — signed Microsoft binary used as the launcher |
| Loader | `WindowsCodecs.dll` — side-loaded DLL |
| Technique | **T1574.001** — DLL Search Order Hijacking |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Resource Development | T1583.006 – Acquire Infrastructure: Web Services |
| Initial Access | T1566.002 – Spearphishing Link; T1566.001 – Spearphishing Attachment |
| Execution | T1204.001 – Malicious Link; T1059.003 – Windows Command Shell |
| Persistence / Defense Evasion | T1574.002 – DLL Side-Loading |

## Takeaway

Every component in the delivery chain is something a blocklist cannot touch: a webhook.site URL, a ZIP, and a Microsoft-signed `calc.exe`. The malicious element is the DLL sitting beside the signed binary, and the only reliable signal is the pairing — a signed Windows binary executing from a user-writable directory with a non-system DLL in the same folder. Application-directory integrity, not file reputation.
