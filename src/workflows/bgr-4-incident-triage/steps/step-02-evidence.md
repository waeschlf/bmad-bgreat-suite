# Step 2: Evidence Gathering

## MANDATORY EXECUTION RULES (READ FIRST):

- 🔒 READ-ONLY: only query tools; never change monitors, dashboards, infrastructure or tickets in this step
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- 🎯 GATHER FIRST: run the queries below proactively, then present; do not ask permission for each read-only query
- 🧪 Every finding must cite its source query so another engineer can re-run it
- 🔐 Redact secrets, tokens and customer PII from anything copied into the record
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for fix or resolution effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show findings before interpretation
- 💾 Append evidence to section 2 and observations to the timeline
- ⚠️ Present [C]ontinue / [R]evise menu after the evidence summary
- 📖 Update frontmatter `stepsCompleted` to include 2 before loading next step
- 🚫 FORBIDDEN to load next step until C is selected

## CONTEXT BOUNDARIES:

- Incident record from step 1 with signal, service and tool access is available
- Loaded plans tell you which SLOs, dashboards and alerts matter for this service
- This step describes what is happening; ranking causes happens in step 3

## YOUR TASK:

Build a factual picture of the incident: when it started, what is affected, how badly, and what changed around that time.

## EVIDENCE SEQUENCE:

### 1. Fix the Time Window

- Incident window: from 60 minutes before `detectedAt` to now
- Baseline window: same duration, 24 hours earlier (or 7 days earlier for weekly-seasonal traffic)
- State both windows in UTC in the record

### 2. Query the Evidence Source

Use the tools for `{bgr_observability_tool}`. If that tool provides its own skills or guides for querying (e.g. a skill-discovery tool), load the relevant ones before querying. Cover, for the affected service and its direct dependencies:

| Area | What to establish |
|------|-------------------|
| Triggering alert / monitor | Definition, threshold, current state, first trigger time |
| Golden signals | Latency (p50/p95/p99), traffic, error rate, saturation; incident vs baseline |
| SLO | Current SLI, error budget remaining, burn rate |
| Errors | Top error messages or exception types by count; first-seen time of new ones |
| Logs | Error and warning patterns grouped by message; spikes aligned to onset |
| Traces | Slow or failing spans; which downstream call dominates latency or errors |
| Infrastructure | Host, container or pod health, restarts, resource exhaustion |
| Dependencies | Health of upstream and downstream services, databases, queues, third parties |
| Other active alerts | Anything else firing in the window that may be the same incident |

Start broad (service-level aggregates) and narrow only where the data points. Stop when additional queries no longer change the picture.

### 3. Find What Changed

Changes near onset are the most common cause of incidents. Look for, within the incident window and the few hours before it:

- Deployments and releases of the affected service and its dependencies
- Configuration and feature-flag changes
- Infrastructure changes (scaling events, node rotations, certificate renewals, network changes)
- Traffic shifts (campaigns, batch jobs, bot traffic, regional failover)
- Third-party status incidents

Use the evidence source's change or event tracking if it has one; otherwise ask the user where changes are recorded.

### 4. Fall Back When Tools Are Unavailable

If the evidence source is unavailable, give the user a short, specific list of what to paste (e.g. "error rate graph for service X, last 2 hours", "last 3 deploys") and continue with what they provide. Mark those items as "user-provided" instead of citing a query.

### 5. Write the Evidence

Append to section 2 of the record:

```markdown
### 2.1 Windows
- Incident window: {start} - {end} UTC
- Baseline window: {start} - {end} UTC

### 2.2 Findings

| # | Observation | Incident vs baseline | Source (query / link) |
|---|-------------|----------------------|------------------------|
| E1 | {observation} | {numbers} | {query or link} |

### 2.3 Changes Near Onset

| Time (UTC) | Change | Service | Source |
|------------|--------|---------|--------|

### 2.4 Onset
- Best estimate of true onset: {UTC timestamp} (from {evidence #})
- Detection lag: {detectedAt minus onset}
```

Add timeline entries for onset, each relevant change, and any state change of the triggering alert.

### 6. Present Summary and Menu

"**Evidence for {incident_id}** ({n} queries)

- **Onset:** {time} UTC, {detection lag} before detection
- **Impact signal:** {key metric incident vs baseline}
- **Scope:** {services / regions / endpoints affected; what is NOT affected}
- **Changes near onset:** {list or 'none found'}
- **Notable:** {1-3 findings that stand out}

[C] Continue to impact & severity assessment
[R] Revise - dig deeper into a specific area
[M] Mitigate now - skip to step 4"

### 7. Handle Menu Selection

- **R**: ask which area, query further, update section 2, return to the menu
- **C**: update `stepsCompleted`, `lastUpdated`, load `./step-03-assessment.md`
- **M**: update `stepsCompleted`, add a timeline entry, load `./step-04-mitigation-comms.md`

## SUCCESS METRICS:

✅ Incident and baseline windows stated explicitly
✅ Every finding has a re-runnable source
✅ Changes near onset checked, even if none found
✅ Scope includes what is NOT affected
✅ Onset estimated from data, not from alert time alone

## FAILURE MODES:

❌ Interpreting causes before the evidence is written down
❌ Comparing incident numbers without a baseline
❌ Skipping change detection
❌ Copying raw log lines with secrets or PII into the record
❌ Running write operations in an evidence step

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-03-assessment.md`.
