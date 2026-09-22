# Step 2: Gather the Current State

## MANDATORY EXECUTION RULES (READ FIRST):

- 🔒 READ-ONLY: only query tools and records
- 🎯 GATHER FIRST: run the checks below proactively, then present
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- 🧪 Every item carries its source (record path, alert ID, ticket ID, query) so the incoming engineer can open it
- 🔐 Redact secrets, tokens and customer PII
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show the raw inventory before prioritising
- 💾 Write the inventory to the "State Inventory" section
- ⚠️ Present [C]ontinue / [R]evise menu after the inventory
- 📖 Update frontmatter `stepsCompleted` to include 2 before loading next step

## CONTEXT BOUNDARIES:

- Scope, shift windows, previous open items and tool access from step 1 are available
- This step collects facts; step 3 decides what matters

## YOUR TASK:

Build a complete inventory of everything the incoming engineer may need to act on or know about.

## GATHERING SEQUENCE:

### 1. Previous Open Items

For each open item from the previous handoff, find its current state (resolved, unchanged, worse) from its source. Nothing from the previous handoff may disappear without a stated outcome.

### 2. Incidents

- Incident records in `{bgr_incidents}/*/triage.md` with `status` other than `closed`, for services in scope: status, severity, current owner, last update, next status update due, pending mitigations
- Postmortems in `{bgr_incidents}/*/postmortem.md` with open P1 action items or a `followUpDate` in the next shift
- Active or stable incidents in the incident or paging tool, and in any other incident system available in this session

### 3. Alerts

For services in scope, from `{bgr_observability_tool}`:

| Check | What to record |
|-------|----------------|
| Currently firing (alert, warn, no data) | Since when, linked incident if any, acknowledged or not |
| Fired and recovered during the shift | How often; whether anyone looked |
| Flapping | Number of state changes in the shift; the monitor goes on the toil list |
| Muted monitors and downtimes | Which, why, and when they expire; expiring during the next shift is important |
| SLOs | Error budget remaining and burn rate for services in scope |

### 4. Changes

- **During the shift**: deployments, config and feature-flag changes, infrastructure changes for services in scope, with any error-rate change after them
- **During the next shift**: scheduled changes, releases and maintenance windows from the ticket system or change calendar; note who executes them and the rollback owner

### 5. Tickets and Requests

Tickets opened or updated during the shift for services in scope that need on-call attention: high-severity defects, requests waiting on on-call, vendor cases waiting for a response.

### 6. Record Gaps

List every system that could not be queried and every check that returned an error. The incoming engineer must know what was not verified.

### 7. Write and Present

Write the inventory to "State Inventory", grouped as above, each item with its source.

"**State for {scope}** ({shiftStart} - {shiftEnd} UTC)

- **Previous open items:** {n} ({resolved} resolved, {open} still open)
- **Incidents:** {n active} active, {n followups} postmortem follow-ups due
- **Alerts:** {firing} firing, {recovered} fired and recovered, {flapping} flapping, {muted} muted ({expiring} expire next shift)
- **Changes:** {n} during shift, {n} scheduled next shift
- **Tickets needing on-call:** {n}
- **Not checked:** {systems or 'none'}

Anything you know that the tools do not show (conversations, promises made, things you are keeping an eye on)?

[C] Continue to prioritise and write the note
[R] Revise - check something else"

### 8. Handle Menu Selection

- **R**: gather more, update the inventory, return to the menu
- **C**: add the engineer's own notes to the inventory with source "outgoing engineer", update `stepsCompleted`, load `./step-03-prioritise-write.md`

## SUCCESS METRICS:

✅ Every previous open item has a current state
✅ Incidents, alerts, changes and tickets all checked for the scope
✅ Muted monitors and downtimes with their expiry recorded
✅ Next-shift changes included
✅ Unchecked systems listed
✅ Outgoing engineer's tacit knowledge captured

## FAILURE MODES:

❌ Previous open items missing from the inventory
❌ Only current alerts; missing what fired and recovered during the shift
❌ Missing downtimes that expire during the next shift
❌ Items without a source

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-03-prioritise-write.md`.
