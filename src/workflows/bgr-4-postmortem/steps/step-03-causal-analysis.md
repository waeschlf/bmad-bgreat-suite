# Step 3: Blameless Causal Analysis

## MANDATORY EXECUTION RULES (READ FIRST):

- 🤝 BLAMELESS: analyse systems, processes and conditions; when a human action is involved, ask what made it reasonable at the time and what would have made the safe path easier
- 🧪 A cause is **verified** only when evidence confirms the mechanism; otherwise it stays **suspected** and becomes an open question
- 🌳 NO SINGLE ROOT CAUSE: incidents need a trigger AND conditions that let it cause harm AND gaps that delayed detection or recovery
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Propose, then let the team confirm or correct
- ⚠️ Present [C]ontinue / [R]evise menu after the analysis
- 💾 ONLY save when user chooses C (Continue)
- 📖 Update frontmatter `stepsCompleted` to include 3 before loading next step

## CONTEXT BOUNDARIES:

- Verified timeline and impact from step 2 are available
- Triage hypotheses (with evidence for and against) are the starting point, not the answer

## YOUR TASK:

Explain why the incident happened, why it was as bad as it was, and why it lasted as long as it did.

## SEQUENCE:

### 1. Resolve the Triage Hypotheses

For each triage hypothesis: confirmed, refuted, or still open, with the evidence that decided it (code diff, config history, vendor confirmation, reproduction). In facilitate mode, ask the owning team directly.

### 2. Analyse Four Dimensions

| Dimension | Guiding questions |
|-----------|-------------------|
| **Trigger** | What change or event started it? Why did that change carry risk? Why did pre-production checks not catch it (tests, review, staging data, canary)? |
| **Conditions** | What made the system vulnerable? (missing validation, retries amplifying load, shared dependencies, capacity limits, missing fallbacks) |
| **Detection** | Why did detection take as long as it did? Did the alert measure user impact? Was it noisy or flapping, and had people learned to ignore it? |
| **Response & recovery** | What slowed diagnosis or mitigation? (missing runbook, missing access, unclear ownership, rollback difficulty, missing dashboards) |

For each dimension, go at least three levels of "why", stopping at something the organisation can change. Write each factor as a system property, not a person's mistake.

### 3. Check Language

Rewrite blaming or counterfactual phrasing before it enters the document:

| Avoid | Prefer |
|-------|--------|
| "Engineer X deployed broken code" | "The change passed review and CI; the test data did not include the value the downstream API rejects" |
| "Should have noticed the alert" | "The alert had flapped dozens of times that week, so a new trigger carried little signal" |
| "Human error" | The specific condition that made the error likely and undetected |

### 4. Capture the Positives and the Luck

- **What went well**: things to keep and reinforce
- **Where we got lucky**: conditions that limited impact by chance; these are hidden risks

### 5. Present and Menu

"**Causal analysis for {incident_id}**

- **Trigger:** {trigger} ({verified / suspected})
- **Conditions:** {top factors}
- **Detection:** {top factors}
- **Response & recovery:** {top factors}
- **Went well:** {items} | **Got lucky:** {items}
- **Still open:** {count} questions

[C] Continue to action items
[R] Revise the analysis"

### 6. Handle Menu Selection

- **R**: adjust and return to the menu
- **C**: write sections Hypotheses Resolved, Contributing Factors, What Went Well, Where We Got Lucky; add open questions; update `stepsCompleted`; load `./step-04-action-items.md`

## SUCCESS METRICS:

✅ Every triage hypothesis resolved or explicitly left open
✅ All four dimensions analysed to changeable factors
✅ Verified vs suspected clearly marked
✅ No blaming or counterfactual language in the document
✅ Luck captured as risk

## FAILURE MODES:

❌ Stopping at a single root cause
❌ Stopping at "human error"
❌ Presenting a suspected cause as verified
❌ Ignoring detection and response factors because the trigger is clear

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-04-action-items.md`.
