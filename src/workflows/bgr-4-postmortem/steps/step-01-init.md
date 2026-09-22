# Step 1: Postmortem Initialization

## MANDATORY EXECUTION RULES (READ FIRST):

- 🛑 NEVER invent incident facts; start from the triage record, tool data and the user
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- 💬 FOCUS on locating inputs and setting up the document - no analysis yet
- 🚪 DETECT an existing postmortem and hand off to continuation
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show your analysis before taking any action
- 💾 Create or upgrade the postmortem document
- 📖 Set frontmatter `stepsCompleted: [1]` before loading next step
- 🚫 FORBIDDEN to load next step until setup is confirmed

## CONTEXT BOUNDARIES:

- Variables from workflow.md are available in memory
- The incident may or may not have been triaged with `bgr-4-incident-triage`

## YOUR TASK:

Identify the incident, gather every existing record of it, choose the mode, and set up the postmortem document.

## INITIALIZATION SEQUENCE:

### 1. Identify the Incident

From the user's message, get the incident ID, or search `{bgr_incidents}/*/triage.md` for recent records and let the user pick. If the incident lives only in an external incident or paging tool, fetch it from there (read-only) and derive `incident_id` from its ID.

### 2. Check for an Existing Postmortem

Look for `{bgr_incidents}/{incident_id}/postmortem.md`:

- If it exists with `stepsCompleted` containing numbers (i.e. this workflow already ran), **STOP here** and load `./step-01b-continue.md`
- If it exists as a stub from triage (`stepsCompleted: []`), keep it; its content is upgraded in section 5 below

### 3. Load Inputs

| Input | Where | What to extract |
|-------|-------|-----------------|
| Triage record | `{bgr_incidents}/{incident_id}/triage.md` | Signal, evidence table with sources, hypotheses, actions, status updates, timeline, planning gaps |
| Postmortem stub | `{bgr_incidents}/{incident_id}/postmortem.md` | Pre-filled summary, timeline, impact |
| External incident | incident / paging tool | Timeline, responders, severity, customer impact flags |
| Incident response plan | `{bgr_artifacts}/*incident-response*.md` | Postmortem process, severity definitions, action-item tracking rules, review cadence |
| Observability plan | `{bgr_artifacts}/*observability*.md` | SLOs and error budgets for the affected service |
| Other plans | `{bgr_artifacts}/*.md` | Plans the incident may reveal as wrong or incomplete (pipeline, capacity, DR, security) |

Use sharded-first discovery (`*foo*.md`, then `*foo*/index.md`). Track loaded files in frontmatter `inputDocuments`.

If there is no triage record, tell the user the timeline will be reconstructed from the external tool and their input in step 2.

### 4. Choose Mode and Participants

Ask:

- **Mode**: [P] Prepare a draft for the review meeting, or [F] Facilitate the review live
- **Postmortem owner** and **participants** (roles are enough; names optional)
- **Review meeting date**, if scheduled

### 5. Create or Upgrade the Document

Copy `../templates/postmortem-template.md` to `{bgr_incidents}/{incident_id}/postmortem.md`. If a stub existed, move its content into the matching sections of the full template first, then replace the file. Fill frontmatter: `incidentId`, `severity`, `service`, `mode`, `owner`, `participants`, `reviewDate`, `triageRecord`, `createdDate`, `lastUpdated`, `inputDocuments`.

### 6. Report and Confirm

"Postmortem for **{incident_id}** set up at `{bgr_incidents}/{incident_id}/postmortem.md` ({mode} mode).

**Inputs:** triage record {found / not found}, stub {found / not found}, external incident {found / not found}
**Plans:** {incident response plan / observability plan / none}
**Owner:** {owner} | **Review:** {reviewDate or 'not scheduled'}

[C] Continue to timeline & impact"

### 7. Handle Menu Selection

- **C**: set `stepsCompleted: [1]`, load `./step-02-timeline-impact.md`

## SUCCESS METRICS:

✅ All existing records of the incident found and loaded
✅ Stub content preserved when upgrading to the full template
✅ Mode, owner and participants recorded
✅ Existing postmortem handed to step-01b

## FAILURE MODES:

❌ Starting a second postmortem for the same incident
❌ Discarding content from the triage stub
❌ Beginning causal analysis before the timeline is verified

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-02-timeline-impact.md`.
