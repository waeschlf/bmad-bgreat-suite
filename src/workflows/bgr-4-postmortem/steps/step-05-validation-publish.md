# Step 5: Validation & Publish

## MANDATORY EXECUTION RULES (READ FIRST):

- 🔒 NEVER send or post the postmortem or its summary without explicit confirmation of the exact content and audience
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- ✅ VALIDATE before publishing
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show validation results before finalizing
- 💾 Set final frontmatter values only when the user chooses C
- 📖 Update frontmatter `stepsCompleted: [1, 2, 3, 4, 5]` when finalizing

## CONTEXT BOUNDARIES:

- The complete postmortem is available
- In prepare mode, the review meeting may still be ahead; the document can be finalised as `review-ready` and completed after the meeting

## YOUR TASK:

Validate the postmortem, write the executive summary, publish it to the agreed audience, and close the loop with the incident record and planning artifacts.

## SEQUENCE:

### 1. Quality Gates

**Facts:**
- [ ] Timeline has all six milestones or open questions naming who will answer
- [ ] Every timeline entry and impact number has a source
- [ ] Impact measured at the user-facing layer; error budget impact stated

**Analysis:**
- [ ] Trigger, conditions, detection and response & recovery all analysed
- [ ] Verified and suspected causes clearly separated
- [ ] Every triage hypothesis resolved or listed as open

**Actions:**
- [ ] Every significant factor has an action or an accepted risk
- [ ] Every action has type, done criterion, owner, priority, due date
- [ ] Tickets filed (or drafts handed over) and IDs recorded

**Blameless & hygiene:**
- [ ] No individual blamed; no "human error" as a cause; no counterfactual "should have"
- [ ] No secrets, tokens or customer PII

Offer to fix failed gates before continuing.

### 2. Executive Summary

Write five sentences at most, for readers who will read nothing else: what happened, user impact in numbers, why (verified cause or current best understanding), what is being done (P1 actions), and the follow-up date.

### 3. Publish

Ask who receives it (engineering, stakeholders, customer-facing teams) and through which channel. Draft the announcement; send or post only after confirmation, and only through a tool available in this session; otherwise hand the text to the user.

### 4. Close the Loop

- Set the triage record's frontmatter `postmortem` to this document's path and add a timeline entry there
- If `{bgr_artifacts}/production-readiness-checklist.md` exists, update this incident's row in "Live Operations Log" with the postmortem status
- Propose a follow-up review date to check action item progress; record it as `followUpDate`

### 5. Menu

"**Postmortem {incident_id}** - quality gates {passed}/{total}

[C] Finalise ({status: complete} or {status: review-ready} if the meeting is still ahead)
[R] Revise"

### 6. Handle Menu Selection

- **R**: fix and return to the menu
- **C**: set `status` (`review-ready`, `complete`, or `approved` when the owner confirms sign-off), `stepsCompleted: [1, 2, 3, 4, 5]`, `lastUpdated`, `followUpDate`; stop

## SUCCESS METRICS:

✅ All quality gates pass or failures accepted by the owner
✅ Executive summary readable on its own
✅ Published only with confirmed content and audience
✅ Triage record, readiness checklist and follow-up date updated

## FAILURE MODES:

❌ Publishing with blaming language
❌ Marking complete while the review meeting is still ahead
❌ No follow-up date, so action items drift

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions

## NEXT STEP:

Workflow complete. On `followUpDate`, resume this workflow (step-01b) to review action item progress.
