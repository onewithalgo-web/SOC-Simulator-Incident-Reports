# CI/CD Pipeline Hijack: GitHub Actions Compromise (CVE-2025-30066)

**Platform:** SOC Simulator | **Tools:** XDR, SIEM, Threat Intel | **Duration:** ~50m | **Verdict:** True Positive - 8/8 tasks

## Scenario

In March 2025 a version tag on the widely used `tj-actions/changed-files` GitHub Action was re-pointed at malicious code, and the `reviewdog/action-setup` action was pulled into the same run. Linux CI runners executed a payload fetched from an external gist and then scanned the runner for secrets, leaking them into workflow logs behind double-base64 encoding so the platform's log masking would not catch them. The task was to rebuild the chain from SIEM and XDR telemetry and separate the malicious artifacts from benign threat-intel lookups.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Actor identity driving the pipeline | `@tj-actions-bot` |
| Hijacked dependency | `tj-actions/changed-files` (tag `v40` re-pointed; CVE-2025-30066) |
| Setup action pulled into the run | `reviewdog/action-setup` |
| Payload on the runner | `malicious-payload.sh`, fetched from an external gist with `curl` and piped to a shell |
| Secret hunting | TruffleHog run against the runner |
| Staging pattern | Secrets written to workflow logs, double-base64 encoded to evade masking |
| Network path opened | CloudTrail `AuthorizeSecurityGroupIngress` on the GitHub API proxy, source `10.0.2[.]138` |
| Sensor that first flagged the proxy breach | The network IDS (`sec-ids`), not CloudTrail |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1195.002 - Compromise Software Supply Chain |
| Execution | T1059.004 - Unix Shell |
| Defense Evasion | T1027 - Obfuscated Files or Information (double-base64) |
| Credential Access | T1552.001 - Credentials In Files |
| Defense Evasion | T1562.007 - Disable or Modify Cloud Firewall |

## Takeaway

Two details carry the lesson. First, the compromise arrived through a tag, not a code change in the victim's repository, so nothing in the victim's own review process could have caught it; pinning actions to a full commit SHA is the control that closes this. Second, the first alert on the proxy came from the IDS and not from the CloudTrail event that actually loosened the security group. The audit-log record of the change was an Info-level line sitting among ordinary API calls, which is a reminder that a sensor's coverage and a sensor's severity logic are separate questions, and that a quiet source can still hold the key event.