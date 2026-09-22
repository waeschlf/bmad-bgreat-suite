# Step 5: Validation & Hand-off

## MANDATORY EXECUTION RULES (READ FIRST):

- 🔒 NEVER create a ticket or postmortem entry in an external system without explicit confirmation of its content
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- ✅ VALIDATE the record before closing it; an incomplete record makes the postmortem harder
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for fix or resolution effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show validation results before closing
- 💾 Set final frontmatter values only when the user chooses C
- 📖 Update frontmatter `stepsCompleted` to include 5 when finalizing

## CONTEXT BOUNDARIES:

- The complete incident record is available
- Postmortem triggers come from the incident response plan, or the default: SEV1 and SEV2 require a postmortem
- Ticket system rules come from `{bgr_ticket_system}` and any skill available for it

## YOUR TASK:

Validate the incident record, create the follow-up ticket, prepare the postmortem stub, and close or hand over the triage.

## HAND-OFF SEQUENCE:

### 1. Quality Gates

Check each gate and report pass / fail:

**Record:**
- [ ] Signal, service and environment recorded
- [ ] Onset, detection, mitigation times recorded in UTC (mitigation may be pending for a hand-over)
- [ ] Every finding in section 2 has a source
- [ ] Severity set and confirmed by a human

**Response:**
- [ ] Escalation and communication obligations for the severity were met, or the gap is noted
- [ ] Every mitigation action has who / what / when / result
- [ ] Impact end verified with data (or hand-over recipient named)

**Hygiene:**
- [ ] No secrets, tokens or customer PII in the record
- [ ] Hypotheses clearly separated from confirmed findings

Offer to fix any failed gate before continuing.

### 2. Create the Follow-up Ticket

If `{bgr_ticket_system}` is not `none`:

- If a skill exists for that ticket system (for example one that encodes the organisation's required fields, severity rules and routing), load and follow it
- Otherwise draft: title, description (impact, onset, mitigation, leading hypothesis, link to the record), severity mapped to the ticket system's scale, affected component
- Show the full draft; create it only after the user confirms
- Record the ticket ID in frontmatter `ticket` and in the timeline

If the ticket system tool is unavailable, give the user the draft to file manually.

### 3. Prepare the Postmortem Stub

If the severity meets the postmortem trigger (or the user wants one anyway):

- Copy `../templates/postmortem-stub-template.md` to `{bgr_incidents}/{incident_id}/postmortem.md`
- Pre-fill summary, timeline, impact, severity, detection lag, mitigation, the hypothesis table, and "runbook existed: yes / no"
- Leave root cause, contributing factors and action items empty for the blameless postmortem meeting
- Record the path in frontmatter `postmortem`

### 4. Update Operational Context

- If `{bgr_artifacts}/production-readiness-checklist.md` exists, add a row to its "Live Operations Log" section: incident ID, severity, service, record path
- List gaps found during triage that belong in planning artifacts (missing runbook, missing alert, flapping or late alert, alert measuring a dependency instead of user impact, SLO not defined, unclear escalation) and recommend the workflow that fixes each (e.g. `bgr-3-create-observability`, `bgr-3-create-incident-response`)

### 5. Final Summary and Menu

"**{incident_id} ready to close**

- **Severity:** {SEVn} | **Duration:** onset {t} -> mitigated {t}
- **Quality gates:** {passed}/{total}
- **Ticket:** {ID or 'none'}
- **Postmortem:** {path or 'not required'}
- **Planning gaps:** {list or 'none'}

[C] Close triage (status: mitigated, or handed-over)
[R] Revise the record"

### 6. Handle Menu Selection

- **R**: fix, re-run gates, return to the menu
- **C**: set frontmatter `status` to `mitigated` (or `handed-over` with `handedOverTo`), `stepsCompleted: [1, 2, 3, 4, 5]`, `lastUpdated`; add the final timeline entry; stop

## SUCCESS METRICS:

✅ Quality gates run and failures addressed or accepted by the user
✅ Ticket created with confirmed content, or draft handed to the user
✅ Postmortem stub pre-filled when required
✅ Planning gaps fed back to the right planning workflow
✅ Record closed with an accurate status

## FAILURE MODES:

❌ Creating tickets without confirmed content
❌ Writing root cause into the postmortem stub before the postmortem meeting
❌ Closing with `status: mitigated` when impact is ongoing
❌ Losing planning gaps that surfaced during the incident

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions

## NEXT STEP:

Workflow complete. The postmortem is run with the team using `{bgr_incidents}/{incident_id}/postmortem.md`.
