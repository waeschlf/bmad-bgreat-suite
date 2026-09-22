# Step 4: Handover & Acknowledgement

## MANDATORY EXECUTION RULES (READ FIRST):

- 🔒 NEVER post or send the note without explicit confirmation of the exact text and channel
- 🤝 Responsibility transfers only when the incoming engineer acknowledges
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Run the checks before handing over
- 💾 Set final frontmatter values only when the user chooses C
- 📖 Update frontmatter `stepsCompleted: [1, 2, 3, 4]` when finalizing

## CONTEXT BOUNDARIES:

- The complete handoff note is available
- The incoming engineer may be present in this session, or reached through a channel

## YOUR TASK:

Validate the note, hand it over, record the acknowledgement, and feed toil back into planning.

## SEQUENCE:

### 1. Quality Gates

- [ ] Every previous open item has an outcome
- [ ] Every active incident has an owner, next action and next status-update time
- [ ] Every firing alert is linked to an incident, explained, or on the toil list with a reason
- [ ] Downtimes and mutes expiring in the next shift are listed
- [ ] Next-shift changes listed with executor and rollback owner
- [ ] Escalation contacts named
- [ ] Unchecked systems listed
- [ ] No secrets, tokens or customer PII

Offer to fix failed gates.

### 2. Hand Over

Ask how the note reaches the incoming engineer (chat channel, ticket comment, e-mail, in person). Draft the message; send or post only after confirmation and only through a tool available in this session; otherwise hand the text to the user. Set `status: sent` and `sentAt`.

### 3. Acknowledgement

Record when and how the incoming engineer confirmed (in this session, reply in the channel, or verbally as reported by the user), plus any questions they asked and the answers. Set `status: acknowledged`, `acknowledgedAt`, `acknowledgedBy`. If the acknowledgement is pending, say so plainly: responsibility has not been transferred yet.

### 4. Feed Back

- For toil and noise items, recommend the planning workflow that fixes them (usually `bgr-3-create-observability` for noisy or flapping alerts, `bgr-3-create-incident-response` for missing runbooks or unclear escalation)
- Update the handed-over incident records: set `handedOverTo` and add a timeline entry
- If `{bgr_artifacts}/production-readiness-checklist.md` exists, update the status of incidents listed in its "Live Operations Log"

### 5. Menu

"**Handoff {outgoing} -> {incoming}** - gates {passed}/{total}, status {sent / acknowledged}

[C] Finalise
[R] Revise"

### 6. Handle Menu Selection

- **R**: fix and return to the menu
- **C**: set `stepsCompleted: [1, 2, 3, 4]`, `lastUpdated`; stop

## SUCCESS METRICS:

✅ Quality gates pass or failures accepted by the outgoing engineer
✅ Note delivered only with confirmed text and channel
✅ Acknowledgement recorded, or pending status stated plainly
✅ Incident records and toil feedback updated

## FAILURE MODES:

❌ Marking the handoff complete without an acknowledgement
❌ Posting the note without confirmation
❌ Losing toil items instead of routing them to planning

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions

## NEXT STEP:

Workflow complete. The next handoff starts from this document.
