# Step 1: Incident Intake & Initialization

## MANDATORY EXECUTION RULES (READ FIRST):

- 🛑 NEVER invent incident facts; everything in the record comes from the user or from a cited tool result
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- ✅ ALWAYS treat this as a partnership with the on-call engineer; they own decisions
- 💬 FOCUS on intake and setup only - don't start root-cause analysis yet
- 🚪 DETECT an existing record for this incident and hand off to continuation
- ⏱️ Keep intake short: ask only what you cannot look up yourself
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for fix or resolution effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show your analysis before taking any action
- 💾 Create the incident record and start the timeline
- 📖 Set frontmatter `stepsCompleted: [1]` before loading next step
- 🚫 FORBIDDEN to load next step until intake is confirmed

## CONTEXT BOUNDARIES:

- Variables from workflow.md are available in memory
- The user may give anything from a pasted alert to a single sentence; work with what you get
- Plan discovery happens in this step; evidence gathering happens in step 2

## YOUR TASK:

Capture the incident signal, create or resume the incident record, and load the team's incident response plan and related operational context.

## INITIALIZATION SEQUENCE:

### 1. Capture the Signal

Extract from the user's message, asking only for what is missing:

- **Signal source**: alert/monitor ID or name, incident ID in the paging or incident tool, customer report, or engineer observation
- **Symptom**: what is broken, in the user's words
- **Affected service(s) / environment**: if known
- **First observed at**: absolute timestamp, converted to UTC
- **Reporter and current responders**

If the signal is an alert or incident ID and a matching MCP tool for `{bgr_observability_tool}` or the paging tool is available, fetch the alert or incident details yourself instead of asking.

For an alert, also read its state-change history (alert / warn / recovered events), not just its current status:

- **First observed at** is the earliest trigger of the current episode, not the latest notification. Widen the history window until you find a quiet period before the first trigger
- **Flapping** (repeated trigger / recover cycles) means the threshold sits at the edge of normal behaviour. Record the number of transitions; the incident may have started long before the alert looked serious, and the alert itself becomes a planning gap for step 5
- If the current status and the latest event disagree (e.g. status "Alert" but last event "Recovered" days ago), say so and trust the underlying data in step 2 over the alert state

Derive `incident_id`:

- Reuse the external incident ID if one exists (e.g. the paging tool's ID)
- Otherwise use `INC-{YYYYMMDD}-{HHMM}-{short-service-slug}` in UTC

### 2. Check for an Existing Record

Look for `{bgr_incidents}/{incident_id}/triage.md`, and also scan `{bgr_incidents}/*/triage.md` for records with `status: active` that match the same service and symptom:

- If a matching record exists with `stepsCompleted`, **STOP here** and load `./step-01b-continue.md`
- If an unrelated active incident exists for the same service, tell the user; they may be the same incident

### 3. Load Operational Context

Search `{bgr_artifacts}/`, `{project_knowledge}/` and `{project-root}/docs/` (sharded-first: for `*foo*.md` also try `*foo*/index.md`):

| Document | What to extract |
|----------|-----------------|
| `*incident-response*.md` | Severity matrix, escalation matrix, communication channels and cadence, postmortem triggers, incident commander role |
| `*observability*.md` | SLOs and error budgets for the affected service, alert definitions, dashboards |
| `*runbook*` (files or folders) | Runbooks for the affected service or alert |
| `*infrastructure*.md` | Topology and dependencies of the affected service |
| `*pipeline*.md` | Deployment and rollback procedures |
| `*disaster-recovery*.md` | Failover procedures, if the incident could escalate to DR |

If no incident response plan exists, say so plainly and use the default severity scale in `./step-03-assessment.md` for this incident only, with a postmortem required for SEV1 and SEV2. Recommend running `bgr-3-create-incident-response` after the incident.

Track loaded documents in frontmatter `inputDocuments`.

### 4. Check Tool Access

Determine which tools you can actually use in this session:

- **Evidence source** (`{bgr_observability_tool}`): look for MCP tools for it (e.g. metrics, logs, traces, monitors, events). Run one cheap read-only query to confirm access
- **Ticket system** (`{bgr_ticket_system}`): look for MCP tools or a skill for it (e.g. a skill that knows the house rules for defects in that system)
- **Paging / chat tools**: note whether they are available for later status updates

If a tool is configured but unavailable, tell the user and fall back to asking them to paste query results. Record tool availability in frontmatter `toolAccess`.

### 4b. Check Existing Tickets and Incidents

Search every ticket or incident system available in this session (e.g. incident management, ITSM, defect tracker) for items raised since shortly before `detectedAt` on the same service, dependency or error text. Record matches in the record's Signal section with what each covers. Name any system that is not reachable; never conclude "nothing exists" from a system you could not query.

### 5. Create the Incident Record

Copy `../templates/triage-record-template.md` to `{bgr_incidents}/{incident_id}/triage.md` and fill:

- Frontmatter: `incidentId`, `status: active`, `severity: unclassified`, `service`, `environment`, `detectedAt`, `createdDate`, `lastUpdated`, `inputDocuments`, `toolAccess`
- Section 1 (Signal) from the intake
- First timeline entries: detection time, and the time this triage started

### 6. Report and Confirm

"**{incident_id}** record opened at `{bgr_incidents}/{incident_id}/triage.md`.

**Signal:** {one line}
**Service / env:** {service} / {environment}
**Detected:** {UTC timestamp}

**Plans loaded:** {incident response plan / observability plan / runbooks found, or 'none - using default severity scale'}
**Tool access:** {evidence source: ok / unavailable}, {ticket system: ok / unavailable}

Anything to add before I pull evidence (other symptoms, suspected change, customers reporting)?

[C] Continue to evidence gathering
[M] Mitigate now - impact is severe, skip to step 4 and backfill later"

### 7. Handle Menu Selection

- **C**: set `stepsCompleted: [1]`, load `./step-02-evidence.md`
- **M**: set `stepsCompleted: [1]`, add a timeline entry "Jumped to mitigation before diagnosis", load `./step-04-mitigation-comms.md`

## SUCCESS METRICS:

✅ Incident ID derived and record created from the template
✅ Detection timestamp recorded in UTC
✅ Existing record detected and handed to step-01b
✅ Incident response plan loaded, or defaults declared explicitly
✅ Tool access verified with a real read-only query, not assumed
✅ Existing tickets and incidents searched and linked
✅ User confirmed intake

## FAILURE MODES:

❌ Asking the user for data a tool could have fetched
❌ Opening a second record for an incident that already has one
❌ Silently inventing severity rules when no plan exists
❌ Claiming tool access without testing it
❌ Starting root-cause analysis before evidence is gathered

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-02-evidence.md`. After [M], load `./step-04-mitigation-comms.md`.
