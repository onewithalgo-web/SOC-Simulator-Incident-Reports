# Open Bucket, Open Books

**Platform:** SOC Simulator | **Tools:** Cloud audit | **Duration:** ~25m | **Verdict:** True Positive — 4/4 tasks

## Scenario

A payments vendor's S3 bucket holding bank-mandate PDFs was quietly made world-readable. Anonymous outsiders listed and bulk-downloaded the data with no credentials at all — no exploit, no malware, no account compromise. The entire incident lives in the cloud audit trail.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Exposed resource | `avanti-nach-mandates` — S3 bucket of NACH bank-mandate documents |
| First anonymous outsider | `89.248.165[.]34` |
| Account attribution | `880142339071` |
| Root cause classification | **T1530** — Data from Cloud Storage |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Defense Evasion | T1578 – Modify Cloud Compute Infrastructure |
| Discovery | T1619 – Cloud Storage Object Discovery; T1580 – Cloud Infrastructure Discovery |
| Collection | T1530 – Data from Cloud Storage |

## Takeaway

Nothing here was an intrusion in the traditional sense — every request was authorized by the bucket policy. That is exactly why it is dangerous to a detection programme built around attack signatures: the alert has to come from configuration drift, not traffic. The audit trail's value was telling anonymous `GetObject` calls apart from the vendor's own, which only works if unauthenticated access is logged and reviewed rather than assumed absent.
