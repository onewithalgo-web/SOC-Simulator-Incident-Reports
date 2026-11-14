# Exposed MongoDB: Open Database to Ransom Note

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall, Threat Intel | **Verdict:** True Positive - 6/6 tasks | **Accuracy:** 87.5%

## Scenario

A MongoDB instance at Calventis Group, a financial-services firm, was left reachable from the internet on tcp/27017 with no authentication. Scanners found it within hours. One actor read the customer-records data, another bulk-dumped every collection, and a third dropped every database and left a ransom note. The chain had to be rebuilt from the MongoDB audit logs and the perimeter firewall.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| How external clients reached it | Destination port **27017** open to the internet; a database port should never appear in inbound internet traffic, and the firewall's rule table showed it had been permitted |
| First hands-on operator | `185.220.101[.]63`, the first interactive read of the database contents (separate from the drive-by scanner) |
| Data that left in the bulk read | 48,291,044 bytes in the session that read every document in every collection |
| Source of the destruction | `185.151.0[.]197`, the wiper address |
| Ransom note collection | `PLEASE_READ_ME_TO_RECOVER`, the collection created to hold the note |
| ATT&CK impact technique | **T1485** - Data Destruction |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Impact | T1485 - Data Destruction |

## Takeaway

Three different actors used the same open door within hours, and only the last one was destructive. The audit log let me separate them by what each did: a scanner that only touched the port, an operator that read data, a bulk reader that took everything, and a wiper that dropped the databases and wrote the note. The firewall rule table is what explains the whole incident, because it shows the exposure was a deliberate permit and not a bypass.
