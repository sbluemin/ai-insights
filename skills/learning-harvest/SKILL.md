---
name: learning-harvest
description: Run the harvest loop that feeds a task's learnings back into durable artifacts (always-on instructions, procedure docs, tests, scripts, hooks). Use after finishing substantive work when asked to "capture what we learned", write a retrospective, reflect learnings into instructions, or make the next run cheaper — and when artifacts have piled up and need pruning.
---

# Learning Harvest

The SSoT for the methodology is `${CLAUDE_PLUGIN_ROOT}/docs/compounding-engineering-learning-harvest.md`.
**Read that document first**, then follow the procedure below. Do not copy its principles, tables, or prompt templates into this file.

## Procedure

1. **Extract** — collect candidates and judge each against the three conditions (recurrence, cost, generality). If any one fails, drop it.
2. **Settle** — map each candidate to the single home whose *recall trigger* matches (see the table in §2② of the doc). Always flag overlap with existing artifacts.
3. **Encode** — phrase it as `When you see <symptom> → do <action> → because <failure mode>`.
4. **Push toward mechanism** — for each candidate ask "can this go to a more executable layer?" (test > script > hook > procedure > note).
5. **Approval gate** — present the result as a table and **stop.** Persist only what the user picks.
6. **Prune** — alongside anything persisted, propose deletions for entries in the same home that are stale or duplicated.

## Output format

| Candidate | 3-condition verdict | Home | Encoded wording | Overlap | Lower layer possible? |
|---|---|---|---|---|---|

## Guardrails

- **Verify first** — never encode a guess you have not confirmed actually holds. Confirm, then encode.
- **Block overfitting** — do not nail a one-off incident down as a general rule.
- **No duplicates** — one home per fact. When a better home appears, migrate and delete the original.
- **No silent application** — do not skip the gate in step 5.
