# Step 2: Verified Timeline & Impact

## MANDATORY EXECUTION RULES (READ FIRST):

- 🔒 READ-ONLY: only query tools
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- 🧪 Every timeline entry and impact number has a source (query, tool record, or named role's account)
- 🤝 BLAMELESS: timeline entries describe what happened and what was known, by role, never judgement
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Show gaps and corrections before writing
- ⚠️ Present [C]ontinue / [R]evise menu after the timeline and impact
- 💾 ONLY save when user chooses C (Continue)
- 📖 Update frontmatter `stepsCompleted` to include 2 before loading next step

## CONTEXT BOUNDARIES:

- Triage record, stub and external incident data from step 1 are loaded
- Triage happened under time pressure; its timestamps and numbers may be approximate and must be re-verified now that the incident is over

## YOUR TASK:

Produce the authoritative timeline and final impact numbers, and compute the response metrics.

## SEQUENCE:

### 1. Merge the Timeline

Merge entries from the triage record, the external incident tool, and change tracking into one UTC timeline. For each entry keep the source. Mark conflicts between sources.

### 2. Verify Key Timestamps With Data

Re-query the evidence source for the full incident period plus margin, and determine:

| Milestone | Definition |
|-----------|------------|
| Trigger | The change or event that started the incident |
| Onset | First measurable user impact |
| Detection | First alert or report that reached a human |
| Response | First responder acknowledged |
| Mitigation | User impact stopped (verified by data, not by the action time) |
| Resolution | Underlying cause removed; no recurrence risk from this trigger |

Triage often records onset from coarse buckets; refine with finer resolution now.

### 3. Final Impact

Quantify, at the user-facing layer:

- Users, requests or transactions affected; affected journeys; duration
- Error budget consumed per affected SLO (percentage of the period's budget)
- Degraded-but-successful responses (fallbacks, stale data), confirmed with the owning team
- Data integrity, security, financial or contractual consequences (confirmed yes / no)
- Downstream or upstream systems affected (e.g. queued messages rejected, partner systems)

### 4. Response Metrics

Compute: time to detect (onset -> detection), time to respond (detection -> response), time to mitigate (onset -> mitigation), time to resolve (onset -> resolution). Compare against the incident response plan's targets if they exist.

### 5. Collect Gaps

Anything you cannot verify goes to "Open Questions for Review" with the role who can answer it.

### 6. Present and Menu

Show the timeline (key milestones highlighted), impact, metrics and open questions.

"**Timeline & impact for {incident_id}**

- **Trigger -> onset -> detection -> mitigation -> resolution:** {times}
- **Time to detect / mitigate / resolve:** {durations}
- **Impact:** {user-facing numbers}; error budget {percent} of {SLO}
- **Corrections vs triage:** {list or 'none'}
- **Open questions:** {count}

[C] Continue to causal analysis
[R] Revise the timeline or impact"

### 7. Handle Menu Selection

- **R**: adjust and return to the menu
- **C**: write sections Timeline, Impact and Response Metrics, update `stepsCompleted`, load `./step-03-causal-analysis.md`

## SUCCESS METRICS:

✅ One merged timeline with a source per entry
✅ All six milestones determined or listed as open questions
✅ Impact quantified at the user-facing layer, including degraded responses
✅ Response metrics computed and compared to targets
✅ Triage approximations corrected with finer data

## FAILURE MODES:

❌ Copying triage numbers without re-verifying
❌ Using the mitigation action time as the mitigation time
❌ Judgemental wording in the timeline ("failed to notice", "should have")

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-03-causal-analysis.md`.
