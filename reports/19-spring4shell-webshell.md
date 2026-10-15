# Spring4Shell: Class-Loader RCE to Webshell (CVE-2022-22965)

**Platform:** SOC Simulator | **Tools:** SIEM, Firewall | **Duration:** ~40m | **Verdict:** True Positive — 6/6 tasks

## Scenario

An internet-facing Java Spring MVC application was compromised through CVE-2022-22965, a class-loader manipulation flaw that turns Tomcat's own access-logging subsystem into a webshell writer. The exploit request rewrites log configuration so that the "log file" is a JSP in the web root and the "log line" is the shell itself.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Exploit source | `45.120.0[.]88` |
| Dropped webshell | `app_config.jsp` |
| Service account behind the commands | `svc-tomcat` |
| Operator source address (post-exploitation) | `45.120.0[.]134` — different host from the exploit source |
| Lateral probe | `10.80.20[.]15` — internal backend service |
| Persistence classification | **T1505.003** — Server Software Component: Web Shell |

## MITRE ATT&CK mapping

| Tactic | Technique |
|---|---|
| Initial Access | T1190 – Exploit Public-Facing Application |
| Persistence | T1505.003 – Web Shell |
| Execution | T1059 – Command and Scripting Interpreter |
| Discovery | T1083 – File and Directory Discovery |

## Takeaway

The exploit and the hands-on-keyboard traffic came from two different addresses, which is the detail that matters operationally: blocking the IP that threw the exploit would have left the operator's session untouched. The webshell is the pivot point to hunt for — a JSP that appears in the web root with a modification time that does not match any deployment.
