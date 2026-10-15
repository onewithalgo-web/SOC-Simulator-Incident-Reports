# Weaponized SVG: Embedded Code in an Image Attachment

**Platform:** SOC Simulator | **Format:** Guided walkthrough | **Tools:** Email, XDR | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks

## Scenario

A treasury analyst at a bank opened what looked like a SWIFT payment confirmation and clicked the button inside it. No document ever appeared. Minutes later the workstation was running code out of the user's own profile directory and holding an encrypted session to a host it had never contacted before.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Delivery sender | `swiftzjy1@financeplus[.]me` |
| File that wrote a ZIP to disk | `SWIFT_Payment_Confirmation.svg` — SVG smuggling |
| Execution chain | `wscript.exe` |
| Command-and-control host | `wwce.zapto[.]org` — dynamic DNS |
| Scripting technique | **T1059.007** — JavaScript |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.001 – Spearphishing Attachment |
| Defense Evasion | T1027.006 – HTML Smuggling (SVG variant) |
| Execution | T1204.001 – Malicious Link; T1059.007 – JavaScript |

## Takeaway

An SVG is an XML document that can carry script, so it passes attachment filtering as an image and executes as code. Pairing that with a dynamic-DNS C2 means neither the delivery nor the destination carries much reputation signal. The reliable detection is behavioural: a browser or image handler spawning `wscript.exe`, and a ZIP appearing in a user profile with no download event behind it.
