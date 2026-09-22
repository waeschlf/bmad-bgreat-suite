# Step 4: Mitigation & Communication

## MANDATORY EXECUTION RULES (READ FIRST):

- 🚫 NEVER execute a mitigation on production yourself; propose it, the human executes or explicitly delegates that single action
- 🔒 NEVER post a message, page anyone, or update a status page without explicit confirmation of the exact text
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- 🩹 MITIGATE BEFORE ROOT CAUSE: restoring service beats understanding it; prefer reversible actions
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for fix or resolution effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show options with risk and rollback before recommending one
- 💾 Record every action and message in the timeline with UTC timestamps
- ⚠️ Present the menu after each mitigation attempt
- 📖 Update frontmatter `stepsCompleted` to include 4 before loading next step
- 🚫 FORBIDDEN to load step 5 until impact has ended or the user decides to hand off

## CONTEXT BOUNDARIES:

- Assessment from step 3 is available, unless the user jumped here from step 1 or 2
- If severity is still `unclassified`, ask for a provisional severity first; it drives communication obligations
- Deployment and rollback procedures from the pipeline plan are in memory if found

## YOUR TASK:

Get user impact to end as safely as possible, verify it has ended, and keep stakeholders informed at the cadence the plan requires.

## MITIGATION SEQUENCE:

### 1. Propose Mitigation Options

For the leading hypothesis (or for the symptom, if there is no hypothesis yet), propose options:

| Option | Action (exact command or UI path) | Addresses | Risk | Reversible? | Rollback |
|--------|-----------------------------------|-----------|------|-------------|----------|
| A | Roll back deploy {version} | H1 | low | yes | redeploy {version} |

Typical options, in rough order of preference:

1. Roll back the change near onset
2. Disable the feature flag or configuration involved
3. Shift traffic away (failover, drain, region switch)
4. Add capacity (scale out, raise limits)
5. Shed load (rate limit, disable non-critical features)
6. Restart the affected component (last resort; destroys evidence - capture evidence first)

Include matched runbook actions. Recommend one option and say why.

### 2. Execute and Verify

For each action the user decides to take:

- Record in the timeline: who, what, when (UTC)
- After it takes effect, re-run the impact queries from step 2 and compare against the incident window and the baseline
- State the result: **mitigated**, **partially mitigated**, or **no effect**
- If no effect or worse: roll back the action if appropriate, and return to the options

### 3. Draft Status Updates

Draft updates from `../templates/status-update-template.md`, following the plan's channel and cadence for the severity:

- **Internal update**: what is happening, impact, what we are doing, next update time
- **Stakeholder update** (if required): plain language, business impact, no speculation about cause
- **Public status page** (if required): customer-facing, no internal names, no blame

Show each draft. Send only after the user approves the exact text, and only through a tool available in this session; otherwise hand the text to the user to post. Record each sent update in the timeline and in frontmatter `lastStatusUpdate`, and state when the next one is due.

### 4. Escalate When Needed

Compare elapsed time since onset against the escalation matrix. When an escalation threshold is reached, tell the user who must be engaged now. If the leading hypothesis collapses, or mitigation fails twice, recommend escalating or bringing in the owning team.

### 5. Write Section 4

Append to the record:

```markdown
## 4. Mitigation & Communication

### 4.1 Actions

| Time (UTC) | Action | By | Result |
|------------|--------|----|--------|

### 4.2 Status Updates Sent

| Time (UTC) | Audience | Channel | Summary |
|------------|----------|---------|---------|

### 4.3 Current State
- Impact: {ended / reduced / ongoing}, verified by {evidence}
- Mitigated at: {UTC timestamp or 'not yet'}
```

### 6. Menu

"**{incident_id}** - impact {ended / reduced / ongoing}. Next status update due {time or 'n/a'}.

[A] Another mitigation option
[U] Draft the next status update
[C] Continue to hand-off (impact ended, or handing over)
[R] Revise the options"

### 7. Handle Menu Selection

- **A / U / R**: do it and return to the menu
- **C**: if impact has not ended, confirm the user is handing over rather than resolving and record who takes over; set `mitigatedAt` if mitigated; update `stepsCompleted`; load `./step-05-handoff.md`

## SUCCESS METRICS:

✅ Options include exact actions, risk and rollback
✅ Every action verified against data, not assumed to work
✅ Status updates sent on the plan's cadence, each approved by the user
✅ Escalation thresholds checked against elapsed time
✅ Timeline complete with who / what / when

## FAILURE MODES:

❌ Running a production change without explicit human go-ahead
❌ Declaring mitigation without re-checking the impact metrics
❌ Restarting components before capturing evidence
❌ Status updates that speculate about root cause or blame
❌ Missing a required escalation or status update

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-05-handoff.md`.
