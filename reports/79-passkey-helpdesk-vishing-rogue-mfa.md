# Passkey Help-Desk Vishing: Rogue MFA Registration to SharePoint Data Theft

**Platform:** SOC Simulator | **Tools:** SIEM, Cloud audit, Query | **Verdict:** True Positive - 7/7 tasks | **Accuracy:** 100%

## Scenario

An identity alert fired at 13:53 UTC on 29 September on an employee's account at Northcape Mutual Insurance: a new Microsoft Authenticator method had been registered. The service desk was halfway through a company passkey rollout, three identical alerts had been closed as benign earlier in the wave, and the employee's own note on the ticket said IT had called him that afternoon, walked him through the enrolment page and that he had approved the prompt on his phone. A second alert for a bulk download on the same account landed that evening, next to one for a Claims colleague that had been closed against a change record three hours earlier. The job was to establish what was added to the account, from where, and what happened next, using three weeks of Entra sign-in and directory audit events and the Microsoft 365 activity for SharePoint, OneDrive and Exchange.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Where the newcomer came from | `203.0.113[.]154`, an origin that did not fit the employee's three weeks of logons |
| What was actually added | The directory audit record tags the new method `SoftwareTokenActivated`, a software token, not the passkey the ticket described |
| The page he was read | `northcape.passkeyhelpdesk[.]com`, an enrolment page on a domain that is not the company's own |
| The other heavy reader | The colleague's settled alert was closed with a comment saying the items were being moved to `Claims-Archive-2026`, which explained that case as legitimate and left the second one unexplained |
| Tool doing the pulling | User agent `python-httpx/0.27.2`, a scripted client rather than a browser |
| The morning after | The same identity signed in again from a second outside origin and cleared the second check with an **OATH verification code** instead of approving a push |
| Mailbox exposure | 229 mailbox items read through the Exchange API from the two outside origins, across both days |

## Takeaway

The ticket story and the audit record disagreed, and the record was the one the platform wrote rather than the employee. The employee described a passkey set up with IT on the line; the directory log shows a software token added, and an origin that did not fit his normal logons. Together with an enrolment page on an outside domain, that points to a vishing-led rogue MFA registration, and the next-morning sign-in cleared with a one-time code is consistent with the new token being used. The change record that cleared the colleague's alert is what made the second alert stand out: one bulk download had a business explanation and the other did not.
