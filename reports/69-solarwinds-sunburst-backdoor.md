# Trust, Forged: The Monitoring-Platform Backdoor (SUNBURST-style)

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall, Cloud, Threat Intel | **Duration:** ~1h 35m | **Verdict:** True Positive - 7/7 tasks

## Scenario

A trusted, signed plugin in a network-monitoring platform was backdoored upstream, in the style of the SolarWinds SUNBURST intrusion. From the monitoring server the implant switched off the host's own defences, beaconed over DNS to algorithmically generated subdomains, followed a CNAME redirect to a staged command-and-control host, and then reached the on-prem federation server to read the token-signing key. That key was used to forge SAML tokens and sign in to the cloud as privileged users without MFA.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Defense tamper | `WinDefend` start type set to disabled (System event 7040, with the matching registry change in event 4657) minutes after the plugin loaded a pipe |
| DNS beacon apex | `azcdnedge-sync[.]com` (long random subdomains, all one parent) |
| Redirect | CNAME answer to `cdn-stage6.datapush-relay[.]net` |
| Live C2 channel | HTTPS from the business-layer host process to `45.86.230[.]41` |
| Trust theft | AD FS token-signing (DKM key) access on `GREYFORD-ADFS-01` |
| First forged sign-in | `193.142.146[.]77`, privileged federated sign-in with no interactive logon behind it |
| Initial access | **T1195.002** - Compromise Software Supply Chain |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1195.002 - Compromise Software Supply Chain |
| Defense Evasion | T1562.001 - Disable or Modify Tools |
| Command and Control | T1568.002 - Domain Generation Algorithms; T1071.004 - DNS |
| Credential Access | T1552 - Unsecured Credentials (AD FS signing material) |
| Defense Evasion / Lateral Movement | T1606.002 - Forge Web Credentials: SAML Tokens |
| Persistence | T1098.001 - Additional Cloud Credentials (service principal credentials added after the forged sign-in) |

## Takeaway

The intrusion has no failed-logon trail to chase. The forged sign-ins are successful, federated, single-factor and from unfamiliar external addresses, so the usable signal is what is absent: no interactive authentication preceded the SAML token issue, and conditional access never applied. The timeline is also the proof. The Defender disable, the first beacon lookup and the DNS redirect all sit within the first hours on the monitoring server, and the token-signing access follows later the same day, so a detection on the very first tamper event would have cut the chain before any identity was touched.