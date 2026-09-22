# Step 1b: Resume an Active Incident

## MANDATORY EXECUTION RULES (READ FIRST):

- 🛑 NEVER discard or rewrite existing timeline entries
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- 💬 FOCUS on re-establishing situational awareness fast
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for fix or resolution effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show your analysis before taking any action
- 📖 Read the existing record completely
- 💾 Add a timeline entry for the resumption
- 🚫 FORBIDDEN to proceed to next step without user confirmation

## CONTEXT BOUNDARIES:

- The existing record and its frontmatter are available
- Time has passed since the record was last updated; the situation may have changed

## YOUR TASK:

Rebuild situational awareness from the record, check what changed since `lastUpdated`, and resume at the right step.

## CONTINUATION SEQUENCE:

### 1. Read the Record

From frontmatter: `status`, `severity`, `stepsCompleted`, `lastStep`, `lastUpdated`, `toolAccess`, `ticket`, `postmortem`.
From content: last five timeline entries, current leading hypothesis, mitigations in progress, last status update sent.

### 2. Check What Changed

If the evidence source is available, re-run the key queries recorded in section 2 (Evidence) for the window since `lastUpdated`. Summarise: better, worse, or unchanged.

### 3. Present the Briefing

"**{incident_id}** - {status}, {severity}. Last updated {lastUpdated} UTC.

**Since then:** {better / worse / unchanged, with the metric}
**Leading hypothesis:** {hypothesis or 'none yet'}
**Mitigation in progress:** {action or 'none'}
**Last status update sent:** {time or 'none'}; next due per plan: {time or 'n/a'}
**Steps completed:** {stepsCompleted}

[R] Resume at the next step
[E] Re-gather evidence (step 2)
[M] Go to mitigation & comms (step 4)
[H] Go to hand-off (step 5)
[X] Close this record as a duplicate or false alarm"

### 4. Handle Choice

- **R**: load the step after the highest number in `stepsCompleted`
- **E / M / H**: load `./step-02-evidence.md`, `./step-04-mitigation-comms.md`, or `./step-05-handoff.md`
- **X**: ask for the reason, set `status: closed`, `resolution: duplicate | false-alarm` and reference the other incident ID if duplicate; add a timeline entry; stop

Add a timeline entry "Triage resumed by {user_name}" and set `lastStep` before loading the next step.

## SUCCESS METRICS:

✅ Briefing delivered with a fresh read of the key signals
✅ No existing content lost
✅ Resumption recorded in the timeline

## FAILURE MODES:

❌ Resuming from stale data without re-checking current signals
❌ Rewriting earlier timeline entries
❌ Closing a record without a stated reason

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions

## NEXT STEP:

Valid step files to load:
- `./step-02-evidence.md`
- `./step-03-assessment.md`
- `./step-04-mitigation-comms.md`
- `./step-05-handoff.md`
