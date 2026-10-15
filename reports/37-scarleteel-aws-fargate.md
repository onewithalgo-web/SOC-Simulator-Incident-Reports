# SCARLETEEL: AWS Fargate Breach to Cloud Crypto-Mining

**Platform:** SOC Simulator | **Tools:** Cloud audit, SIEM, Firewall | **Duration:** ~55m | **Verdict:** True Positive — 7/7 tasks

## Scenario

An internet-reachable JupyterLab notebook on AWS Fargate was exploited for code execution. The operator stole the task role credentials from the container metadata endpoint, replayed them to harvest Secrets Manager and SSM secrets, minted a new access key for persistence, read the Terraform state and a customer-ledger export, and finished by launching compute-optimized instances to mine cryptocurrency.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Where the intrusion began | `45.174.0[.]41` |
| How the cloud credentials were stolen | **T1552.005** — Cloud Instance Metadata API |
| Stolen workload identity | `auria-ds-notebook-task-role` |
| Secret retrieved in plaintext | `prod/db/credentials` |
| Persistence established | `AKIA734198265041QF7K` — attacker-minted access key |
| Data exfiltration egress | `45.174.0[.]58` |
| Final impact classification | **T1496** — Resource Hijacking |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access / Execution | T1610 – Deploy Container; T1611 – Escape to Host |
| Credential Access | T1552.005 – Cloud Instance Metadata API; T1555.006 – Cloud Secrets Management Stores |
| Persistence | T1098.001 – Additional Cloud Credentials |
| Collection / Exfiltration | T1530 – Data from Cloud Storage; T1537 – Transfer Data to Cloud Account |
| Impact | T1496 – Resource Hijacking |

## Takeaway

The mining is the loud part and the least important: the operator already had the customer ledger and the Terraform state, which describes the whole environment. The pivot that made everything else possible was the metadata endpoint — container code execution becomes IAM role compromise in one HTTP request unless IMDSv2 is enforced and the task role is scoped to what the notebook actually needs. The new access key is the artefact to hunt, because it survives rebuilding the container.
