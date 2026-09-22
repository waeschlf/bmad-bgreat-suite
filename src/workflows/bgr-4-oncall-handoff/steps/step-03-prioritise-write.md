# Step 3: Prioritise & Write the Note

## MANDATORY EXECUTION RULES (READ FIRST):

- 🛑 The outgoing engineer decides priorities; you propose them with reasons
- 🧭 NOTHING SILENTLY DROPPED: every inventory item lands in a priority bucket or is closed with a reason
- 🔕 An item labelled "known noise" needs a reason and goes on the toil list
- ⏱️ The summary fits on one screen
- 📖 CRITICAL: ALWAYS read the complete step file before taking any action - partial understanding leads to incomplete decisions
- 🔄 CRITICAL: When loading next step with 'C', ensure the entire file is read and understood before proceeding
- ⚠️ ABSOLUTELY NO TIME ESTIMATES for effort
- ✅ YOU MUST ALWAYS SPEAK OUTPUT In your Agent communication style with the config `{communication_language}`

## EXECUTION PROTOCOLS:

- 🎯 Propose the buckets, then refine with the user
- ⚠️ Present [C]ontinue / [R]evise menu after the draft note
- 💾 ONLY save when user chooses C (Continue)
- 📖 Update frontmatter `stepsCompleted` to include 3 before loading next step

## CONTEXT BOUNDARIES:

- The state inventory from step 2 is available
- The incident response plan's severity and escalation rules apply when present

## YOUR TASK:

Turn the inventory into a short, prioritised handoff note the incoming engineer can act on immediately.

## SEQUENCE:

### 1. Propose Priority Buckets

| Bucket | Goes here when | Each item needs |
|--------|----------------|-----------------|
| **Act now** | Active incident, firing alert with user impact, deadline in the next hours (status update due, change to supervise, downtime expiring) | Next action, owner, check time |
| **Watch** | Degraded but stable, recovering incident, flapping alert on an important service, risky change in the next shift | What to watch, threshold that turns it into "act now", source |
| **FYI** | Resolved during the shift, closed previous items, context worth knowing | One line |
| **Carry-over** | Longer-running items that belong to on-call across shifts (vendor case, postmortem follow-up) | Owner, next check time |
| **Toil / noise** | Alerts that fired without needing action, manual repetitive work done during the shift | Reason, count, suggested fix |

Explain each proposal in one line. Ask about items where the bucket is unclear.

### 2. Check Completeness

- Every inventory item is in a bucket or closed with a reason
- Every "act now" and "carry-over" item has an owner and a check time
- Escalation contacts for the next shift are named, from the plan or the user
- Unchecked systems are listed

### 3. Draft the Note

Fill the note sections of the document from the template: summary (at most five lines), the buckets as tables, next-shift changes, escalation contacts, not-checked systems, and a link to the previous handoff.

### 4. Present and Menu

Show the complete note.

"[C] Continue to handover and acknowledgement
[R] Revise the note"

### 5. Handle Menu Selection

- **R**: adjust and return to the menu
- **C**: save the note, update `stepsCompleted`, load `./step-04-acknowledge.md`

## SUCCESS METRICS:

✅ Every inventory item accounted for
✅ Act-now and carry-over items have owner and check time
✅ Summary readable in under a minute
✅ Noise items have reasons and are on the toil list

## FAILURE MODES:

❌ A long unprioritised list
❌ "Known issue" without a reason or ticket
❌ Act-now items without an owner

❌ **CRITICAL**: Reading only partial step file - leads to incomplete understanding and poor decisions
❌ **CRITICAL**: Proceeding with 'C' without fully reading and understanding the next step file

## NEXT STEP:

After [C], load `./step-04-acknowledge.md`.
