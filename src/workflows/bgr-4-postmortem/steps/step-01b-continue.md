# Step 1b: Resume a Postmortem

## MANDATORY EXECUTION RULES (READ FIRST):

- 🛑 NEVER discard existing analysis or action items
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show your analysis before taking any action
- 📖 Read the existing postmortem completely
- 🚫 FORBIDDEN to proceed to next step without user confirmation

## CONTEXT BOUNDARIES:

- The existing postmortem and its frontmatter are available
- Action items may have been filed and progressed since the last session

## YOUR TASK:

Summarise where the postmortem stands and resume at the right step.

## CONTINUATION SEQUENCE:

### 1. Analyse State

From frontmatter: `status`, `mode`, `stepsCompleted`, `lastStep`, `owner`, `reviewDate`, `lastUpdated`.
From content: sections still empty, open questions for the meeting, action items with and without tickets.

If action items have ticket IDs and the ticket system tool is available, read their current status (read-only).

### 2. Present Summary

"**Postmortem {incident_id}** - {status}, {mode} mode, last updated {lastUpdated}.

- **Steps completed:** {stepsCompleted}
- **Open questions for review:** {count}
- **Action items:** {total}; {filed} filed, {done} done

[R] Resume at the next step
[O] Overview of remaining steps
[A] Go to action items (step 4)
[V] Go to validation & publish (step 5)"

### 3. Handle Choice

- **R**: load the step after the highest number in `stepsCompleted`
- **O**: describe remaining steps, let the user pick
- **A / V**: load `./step-04-action-items.md` or `./step-05-validation-publish.md`

Set `lastStep` before loading the next step.

## SUCCESS METRICS:

✅ Current state summarised including ticket progress
✅ No content lost

## FAILURE MODES:

❌ Re-running analysis that is already agreed
❌ Losing track of filed tickets

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions

## NEXT STEP:

Valid step files to load:
- `./step-02-timeline-impact.md`
- `./step-03-causal-analysis.md`
- `./step-04-action-items.md`
- `./step-05-validation-publish.md`
