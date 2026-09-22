# Step 1b: Resume an Unfinished Handoff

## MANDATORY EXECUTION RULES (READ FIRST):

- 🛑 NEVER discard gathered state; re-check it, because it may be stale
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show your analysis before taking any action
- 🚫 FORBIDDEN to proceed to next step without user confirmation

## CONTEXT BOUNDARIES:

- A draft handoff exists; the live state has moved on since it was written

## YOUR TASK:

Resume the draft handoff and decide whether the gathered state must be refreshed.

## CONTINUATION SEQUENCE:

### 1. Read the Draft

From frontmatter: `outgoing`, `incoming`, `scope`, `shiftEnd`, `stepsCompleted`, `lastUpdated`. From content: number of items per priority.

### 2. Present

"Draft handoff **{outgoing} -> {incoming}**, last updated {lastUpdated} UTC ({age} ago).

- **Steps completed:** {stepsCompleted}
- **Items:** {act now} act now, {watch} watch, {fyi} FYI

[R] Refresh the state (step 2) - recommended if more than 30 minutes old
[P] Continue to prioritise and write (step 3)
[A] Go to acknowledgement (step 4)
[X] Discard this draft and start a new handoff"

### 3. Handle Choice

- **R / P / A**: load `./step-02-gather-state.md`, `./step-03-prioritise-write.md` or `./step-04-acknowledge.md`
- **X**: confirm, set `status: discarded`, then load `./step-01-init.md`

Set `lastStep` before loading the next step.

## SUCCESS METRICS:

✅ Stale state detected and refresh offered
✅ No gathered content lost

## FAILURE MODES:

❌ Handing over a state that is hours old without saying so

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions

## NEXT STEP:

Valid step files to load:
- `./step-01-init.md`
- `./step-02-gather-state.md`
- `./step-03-prioritise-write.md`
- `./step-04-acknowledge.md`
