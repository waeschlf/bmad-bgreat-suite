# Step 1: Handoff Initialization

## MANDATORY EXECUTION RULES (READ FIRST):

- 🛑 NEVER assume the on-call scope; confirm it or reuse it from the previous handoff
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- 💬 FOCUS on setup only - gathering happens in step 2
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show your analysis before taking any action
- 💾 Create the handoff document
- 📖 Set frontmatter `stepsCompleted: [1]` before loading next step
- 🚫 FORBIDDEN to load next step until setup is confirmed

## CONTEXT BOUNDARIES:

- Variables from workflow.md are available in memory
- The user may be the outgoing engineer (typical) or the incoming one preparing for a shift

## YOUR TASK:

Establish who hands over to whom, which services and time window are covered, and what the previous handoff left open.

## INITIALIZATION SEQUENCE:

### 1. Check for an Unfinished Handoff

Look in `{bgr_incidents}/handoffs/` for a document with `status: draft` created in the last 24 hours. If found, **STOP here** and load `./step-01b-continue.md`.

### 2. Load the Previous Handoff

Find the most recent handoff with `status: acknowledged` (or `sent`). From it, take:

- `scope` (services, teams, environments, tags)
- `shiftEnd` as the default start of this shift window
- Open items: everything under "Act now" and "Watch", and all "Carry-over" items

If there is no previous handoff, say so; this handoff starts the chain.

### 3. Confirm Scope and People

Ask only for what is missing or has changed:

- **Outgoing** and **incoming** engineer (roles or names)
- **Scope**: services, team tags or environments this rotation covers. Offer the previous scope as default
- **Shift window**: start (default: previous `shiftEnd`, otherwise 12 hours ago) and handoff time (default: now), both in UTC
- **Next shift window**: the period the incoming engineer covers, for looking ahead at scheduled changes

### 4. Load Operational Context

From `{bgr_artifacts}/`: the incident response plan's on-call section (handoff protocol, escalation contacts, severity definitions) and the observability plan's SLOs for services in scope. Track loaded files in `inputDocuments`.

### 5. Check Tool Access

Verify with one cheap read-only query each: evidence source (`{bgr_observability_tool}`), ticket system (`{bgr_ticket_system}`), incident or paging tool, and any other ticket or incident system available in this session. Record results in `toolAccess`. Unreachable systems are named in the note; the incoming engineer must know which sources were not checked.

### 6. Create the Document

Copy `../templates/handoff-template.md` to `{bgr_incidents}/handoffs/handoff-{handoff_date}-{handoff_time}.md` (UTC, `YYYY-MM-DD` and `HHMM`). Fill frontmatter: `outgoing`, `incoming`, `scope`, `shiftStart`, `shiftEnd`, `nextShiftEnd`, `previousHandoff`, `toolAccess`, `inputDocuments`, `createdDate`.

### 7. Report and Confirm

"Handoff **{outgoing} -> {incoming}** set up.

**Scope:** {scope}
**Shift:** {shiftStart} - {shiftEnd} UTC | **Next shift until:** {nextShiftEnd} UTC
**Previous handoff:** {path or 'none - new chain'}; {n} open items to re-check
**Tool access:** {per system: ok / unavailable}

[C] Continue to gather the current state"

### 8. Handle Menu Selection

- **C**: set `stepsCompleted: [1]`, load `./step-02-gather-state.md`

## SUCCESS METRICS:

✅ Scope confirmed, not assumed
✅ Previous handoff found and its open items loaded
✅ Shift windows set in UTC
✅ Tool access verified; unavailable systems recorded

## FAILURE MODES:

❌ Starting a new chain when a previous handoff exists
❌ Guessing the scope
❌ Silently skipping a system that could not be reached

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-02-gather-state.md`.
