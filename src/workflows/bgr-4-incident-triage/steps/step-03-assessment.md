# Step 3: Impact, Severity & Hypotheses

## MANDATORY EXECUTION RULES (READ FIRST):

- 🛑 The user decides the severity; you propose it with reasoning
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- 📏 Use the team's severity matrix when one was loaded; use the default scale below only when none exists
- 🧪 Every hypothesis lists evidence for AND against, by evidence number
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for fix or resolution effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show your reasoning before the proposal
- ⚠️ Present [C]ontinue / [R]evise menu after the assessment
- 💾 ONLY save when user chooses C (Continue)
- 📖 Update frontmatter `stepsCompleted` to include 3 and set `severity` before loading next step
- 🚫 FORBIDDEN to load next step until C is selected

## CONTEXT BOUNDARIES:

- Evidence from step 2 (section 2 of the record) is available
- Severity matrix, escalation matrix and runbooks from the incident response plan are in memory if they were found

## YOUR TASK:

Quantify user impact, classify severity, identify escalation obligations, and rank root-cause hypotheses so mitigation targets the most likely cause.

## ASSESSMENT SEQUENCE:

### 1. Quantify Impact

From the evidence, state as numbers where possible:

- Affected users, requests or transactions (count or percentage), measured at the **user-facing** layer (server / entry requests), not from dependency-call errors. If the two differ, state both and explain the gap (retries, fallbacks, cached or degraded responses)
- Whether "successful" user-facing responses might carry degraded or wrong data because a dependency failed behind them; if unknown, say so and ask the owning team
- Affected user journeys, and whether a workaround exists
- SLO impact: error budget consumed so far and current burn rate
- Data integrity or security implications (yes / no / unknown); if yes or unknown, bring in Sam (Security Lead) per the incident response plan
- Trend: getting worse, stable, or recovering

### 2. Propose Severity

Match the impact against the team's severity matrix. Quote the matching criterion.

**Default scale** (only when no incident response plan exists):

| Level | Criteria |
|-------|----------|
| SEV1 | Complete outage of a critical journey, data loss, or active security breach |
| SEV2 | Major degradation with significant user impact, or data integrity issue for a subset of users |
| SEV3 | Minor impact with a workaround, or elevated errors not yet affecting core journeys |
| SEV4 | Cosmetic or internal-only impact |

When impact is uncertain, propose the higher severity; downgrading later is cheap, under-reacting is not.

### 3. Determine Obligations

From the plan's escalation matrix and communication channels, list what this severity requires now:

- Who must be paged or notified, and by when
- Whether an incident commander is required and who it is
- Communication channel and update cadence
- Whether a public status page entry is required
- Whether a postmortem will be required

### 4. Rank Hypotheses

Build two to five hypotheses. For each:

| # | Hypothesis | Evidence for | Evidence against | Confidence | Fastest test |
|---|------------|--------------|------------------|------------|--------------|
| H1 | {cause} | E1, E3 | E4 | high / medium / low | {query or check} |

Rules:

- A change near onset that touches the affected path is always a hypothesis, even if it looks harmless
- An error-rate step change between deployed versions (from step 2) supports a deploy hypothesis strongly; a dependency-side cause should affect all versions equally
- Prefer hypotheses that explain ALL findings, including what is not affected
- If a fastest test is a read-only query, run it now and update the table

### 5. Match Runbooks

Match the triggering alert, service and leading hypothesis against loaded runbooks. List matches with their immediate actions. If none match, note "no runbook" for the postmortem.

### 6. Write and Present

Append section 3 (Assessment) to the record: impact, severity with quoted criterion, obligations, hypothesis table, matched runbooks.

"**Assessment for {incident_id}**

- **Impact:** {one line with numbers}; trend {worse / stable / recovering}
- **Proposed severity:** {SEVn} - matches '{criterion}'
- **Required now:** {paging / IC / channel / cadence / status page}
- **Leading hypothesis:** H1 {cause} ({confidence}) - {key evidence}
- **Runbook:** {name or 'none'}

Do you agree with the severity?

[C] Continue to mitigation & communication
[R] Revise severity or hypotheses"

### 7. Handle Menu Selection

- **R**: adjust, re-run tests if needed, return to the menu
- **C**: set frontmatter `severity`, add timeline entry "Severity set to {SEVn} by {user_name}", update `stepsCompleted`, load `./step-04-mitigation-comms.md`

## SUCCESS METRICS:

✅ Impact quantified, not just described
✅ Severity tied to a quoted criterion and confirmed by the user
✅ Escalation and communication obligations listed explicitly
✅ Hypotheses cite evidence for and against
✅ Cheap read-only hypothesis tests executed immediately

## FAILURE MODES:

❌ Setting severity without user confirmation
❌ A single hypothesis presented as the answer
❌ Ignoring a recent change because "it was small"
❌ Missing a security or data-integrity angle

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-04-mitigation-comms.md`.
