# TerminalFix: A Signed Binary, Three PNGs and a Python Reverse Tunnel

**Platform:** SOC Simulator | **Tools:** XDR, SIEM, Firewall | **Verdict:** True Positive - 4/4 tasks | **Accuracy:** 38.07%

## Scenario

An endpoint alert fired on a claims analyst's workstation, `CMI-WKS-07`, at Caldwyn Mutual. A signed Microsoft binary, `LockScreenContentServer.exe`, had run that morning and three PNG files had landed on the same machine around the same time. A signed Windows component and a few pictures is the kind of alert a busy shift closes as noise. The analyst had already told the help desk they pasted a "verification command" into a terminal earlier, so how the activity started was not the question. What the binary and the images did afterwards was.

## Investigation and confirmed findings

| Step | Finding |
|---|---|
| Affected host | `CMI-WKS-07` |
| Where the signed binary ran from | `f47f2a8c21c9df4e`, taken from the process-creation and image-load telemetry |
| What would make it return | Two autostart entries, a registry Run value and a scheduled task, both created under the name `LockScreenContentServer_MuODG5yBM` and pointing at the same binary |
| Outbound session | External destination `gitnow[.]dev`, found by tying the session to the perimeter and endpoint network records |

## Takeaway

A binary being signed by Microsoft tells you who built it, not where it should run from or what it should load. That is why the work here was about the file's behaviour rather than its signature: where it ran from, what it loaded, what it set up to survive a reboot and where it connected afterwards. This was the lowest-scoring operation of the batch (38.07%), so it is the one I would revisit.
