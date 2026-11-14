# TeamFiltration: Password Spray to Forgotten Service Account Takeover

**Platform:** SOC Simulator | **Tools:** Cloud audit, SIEM | **Verdict:** True Positive - 5/5 tasks | **Accuracy:** 68%

## Scenario

An identity alert fired at 18:52 UTC on a Chilean retailer's Microsoft 365 tenant: nearly two hundred failed sign-ins to Microsoft Teams in half an hour, from cloud-hosted addresses, against most of the directory. The same addresses kept coming back every hour or so until late that night. The help desk had logged two lockouts and the on-call note read "password spray, blocked, keep watching". The job was to work the burst, find out whether anyone got in, and follow what any working credential reached, using three days of Entra sign-in and directory audit telemetry.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| TeamFiltration client fingerprint | User-agent fingerprint `1.3.00.30866`, presented on every attempt in the burst |
| First account that opened | `svc-posreports`, a service account |
| Where the operator carried on from | `149.88.104[.]19`, the address that kept going after the account opened |
| Where the second factor was in the way | **Azure Portal**; the sign-in stopped at the second-factor step there, but the account kept going from the same address and reached the company's documents |
| Application that held a silent Graph token | Application ID `08e18876-6177-487e-b8b5-cf950c1e598c`, a token issued for Microsoft Graph, the API behind mail, files and the directory |

## Takeaway

The on-call note said the spray was blocked and to keep watching. That was true for the lockouts and wrong about the outcome, because one account had opened. The failures told the help desk about the noise; the evidence that mattered was the single success inside it and what the credential did next. A fixed client string made the burst easy to attribute, and the follow-through is what turned "blocked" into "compromised": the same address kept going after the portal stopped at the second factor, and a Graph token was issued silently to an application for the account.
