# Fake Window, Real Loss: Browser-in-the-Browser Steam Phishing

**Platform:** SOC Simulator | **Tools:** SIEM, Email | **Duration:** ~25m | **Verdict:** True Positive — 6/6 tasks, 61.18% accuracy

## Scenario

A Counter-Strike 2 player on a games studio's community team receives an email offering a free in-game case from a well-known esports organisation. The link leads to a scam site that paints a fake browser pop-up — complete with a counterfeit Steam address bar — entirely in HTML. Sixteen minutes pass between delivery and credential loss.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Lure sender | `rewards@navi-drops.com` — spf=fail, dkim=none, dmarc=fail, delivered anyway |
| Hosting behind the scam domain | `77.105.161[.]50` — `caserevs.com` (11 days old) and sibling `caseneus.cfd` both resolve to it |
| Campaign fingerprint | Page title **"Navi Roulette"**, with body hash `ece6e86b76285b0373fd5806c4` shared by the landing page and the fake pop-up |
| Where the credentials went | `caseners[.]com/auth/steam/login` — form fields `account`, `password`, `remember`; account `marko_dust2` |
| Deception classification | **T1036** — Masquerading |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1566.002 – Phishing: Spearphishing Link |
| Defense Evasion | T1036 – Masquerading (fake in-page browser window with painted URL bar) |
| Credential Access | T1056.003 – Input Capture: Web Portal Capture |
| Resource Development | T1583.001 – Acquire Infrastructure: Domains |

## Outcome

Closed as **True Positive**, 6/6 tasks. Unusually for this batch, every external indicator is independently confirmed: the sender domain (10/89), both campaign domains (14/89 and 15/89) and the hosting address (7/89).

## Takeaway

The user did exactly what security training tells people to do — he checked the address bar — and the address bar was a picture. What settled the case were properties the attacker could not paint: the envelope sender the gateway recorded, the 11-day-old domain, the IP the names resolved to, and above all the destination of the POST. The credentials went neither to Steam nor to the domain shown in the fake bar, but to a third domain on the same host. In a credential-theft case, what was lost is decided by where the form posted, not by what the page looked like.
