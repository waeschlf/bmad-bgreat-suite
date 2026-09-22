# Step 4: Action Items

## MANDATORY EXECUTION RULES (READ FIRST):

- 🔒 NEVER create a ticket without explicit confirmation of its content; confirmation may cover a reviewed batch
- 🎯 FEWER, BETTER ACTIONS: a short list that gets done beats a long list that does not
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort; owners set due dates
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Propose actions from factors, then refine with the owner
- ⚠️ Present [C]ontinue / [R]evise menu after the action list
- 💾 ONLY save when user chooses C (Continue)
- 📖 Update frontmatter `stepsCompleted` to include 4 before loading next step

## CONTEXT BOUNDARIES:

- Contributing factors, positives and luck from step 3 are available
- Planning gaps from the triage record are available
- The ticket system and its rules come from `{bgr_ticket_system}` and any skill available for it

## YOUR TASK:

Turn each significant contributing factor into an owned, measurable action or an explicitly accepted risk, file the actions, and route planning gaps to the right planning workflow.

## SEQUENCE:

### 1. Derive Candidate Actions

For each contributing factor and each "got lucky" item, propose at most one or two actions. Classify each:

| Type | Purpose | Example shape |
|------|---------|---------------|
| Prevent | Stop the trigger class from recurring | Contract test against the dependency's allowed values |
| Detect | Find it faster | Replace count alert with SLO burn-rate alert on the user-facing endpoint |
| Mitigate | Limit blast radius or speed recovery | Circuit breaker, fallback, one-step rollback |
| Process | Improve how people respond | Runbook, ownership, escalation path |

Every action needs:

- **Done criterion** that someone else can verify ("alert fires within 10 minutes in a replay of this incident", not "improve alerting")
- **Owner** (a role or team that accepts it; confirmed, not assigned by you)
- **Priority**: P1 (prevents recurrence of this incident or equivalent), P2 (reduces impact or detection time), P3 (improvement)
- **Due date** set by the owner

### 2. Prune and Balance

- Merge duplicates; drop actions without a clear owner or done criterion, or record them as accepted risks
- Check coverage: every significant factor has an action or an accepted risk with a reason
- Check balance: not only "prevent" actions; detection and recovery improvements often pay off across many incidents
- Keep P1 actions few enough that they actually get done

### 3. Route Planning Gaps

Map gaps to the planning workflow that owns them, and note them as actions of type Process:

| Gap | Workflow |
|-----|----------|
| Missing or noisy alerts, missing SLO, missing dashboard | `bgr-3-create-observability` |
| Missing runbook, unclear severity or escalation | `bgr-3-create-incident-response` |
| Unsafe deploy or rollback | `bgr-3-create-pipeline` |
| Capacity or scaling limits | `bgr-3-create-capacity-plan` |
| Failover or backup weaknesses | `bgr-3-create-disaster-recovery` |
| Security or compliance exposure | `bgr-3-create-security-plan` |

### 4. File Tickets

If `{bgr_ticket_system}` is not `none`:

- If a skill exists for that ticket system, load and follow it for fields, severity mapping and routing
- Draft every ticket: title, description (factor addressed, done criterion, link to the postmortem), priority, owner, due date, and a link to the incident's main defect if one exists
- Before drafting, check the Existing Tickets section: extend or link an existing ticket instead of creating a duplicate
- Draft separate tickets for Unrelated Issues Found; they are not action items of this incident
- Show the drafts as a batch; create only the confirmed ones; record ticket IDs in the action table

If the tool is unavailable, give the user the drafts to file manually.

### 5. Present and Menu

"**Action items for {incident_id}**

| # | Action | Type | Priority | Owner | Due | Ticket |
|---|--------|------|----------|-------|-----|--------|

- **Coverage:** {factors with action}/{total factors}; accepted risks: {count}
- **Planning workflows to run:** {list}

[C] Continue to validation & publish
[R] Revise actions"

### 6. Handle Menu Selection

- **R**: adjust and return to the menu
- **C**: write sections Action Items and Accepted Risks, update `stepsCompleted`, load `./step-05-validation-publish.md`

## SUCCESS METRICS:

✅ Every significant factor has an action or an accepted risk
✅ Every action has type, done criterion, owner, priority, due date
✅ Detection and recovery actions present, not only prevention
✅ Tickets created only after confirmation, IDs recorded
✅ Planning gaps routed to planning workflows

## FAILURE MODES:

❌ Vague actions ("be more careful", "improve monitoring")
❌ Owners assigned without their agreement
❌ A long P1 list that will not be done
❌ Filing tickets without showing drafts

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-05-validation-publish.md`.
