---
status: draft
stepsCompleted: []
---

# Status Update Templates

## Internal (incident channel)

**[{severity}] {incident_id} - {short_title}** - update #{n} at {time_utc} UTC

- **Status:** investigating | identified | mitigating | monitoring | resolved
- **Impact:** {who / what is affected, with numbers}
- **Since last update:** {what changed}
- **Current action:** {what we are doing now, and who}
- **Next update:** {time_utc} UTC

## Stakeholders (business audience)

**{service} incident - {status}**

{One or two sentences on business impact in plain language.}
We are {current action in plain language}. Next update by {time_utc} UTC.

## Public status page

**{customer-facing component} - {status}**

We are investigating {reports of / an issue with} {customer-visible symptom}.
{Workaround, if one exists.} We will provide an update by {time_utc} UTC.

<!-- Rules: no internal service names, no speculation about cause, no blame. -->
