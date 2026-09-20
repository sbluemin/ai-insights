---
name: rethinking-steering
description: Design, audit, and trim any surface that steers an agent — always-on instruction files at any level (CLAUDE.md, AGENTS.md, nested or scoped ones), skill and command definitions, and the recurring request templates a team reuses. Use when writing or cleaning up such a surface, when instructions feel too long, after changing model or baseline to strip stale steering, when skill triggers overlap or misfire, and when the agent re-verifies redundantly or asks approval at every step and stops early.
---

# Rethinking Steering

The SSoT for the methodology is `${CLAUDE_PLUGIN_ROOT}/docs/rethinking-prompt-and-skills.md`.
**Read it first**, then follow the procedure below. Do not copy its criteria tables into this file.

Its four parts map onto the steps: what belongs in a steering surface at all (§1–5), why it goes stale (§6–8), boundaries and the completion contract (§9–11), and the audit itself (§12–13).

## Surfaces in scope

All three carry steering, charge context, and go stale the same way. Audit them together — a rule deleted from one often just moved to another.

| Surface | What it is | What goes wrong |
|---|---|---|
| **Always-on instructions** | Instruction files at any level and under any filename, including inherited/nested ones | Length taxes every task; ancestors get restated; stale priors keep injecting |
| **Skill / command catalog** | Names, descriptions, and bodies of skills, commands, and subagent definitions | Triggers written as topics misfire and crowd each other out; monolithic bodies burn context |
| **Request templates** | The prompt shapes a team reuses for recurring work | Missing completion contract; "pause for review" phrasing that biases toward stopping early |

## Procedure

1. **Inventory** — collect every sentence from the surfaces in scope: instruction files, aliases, root-to-leaf inheritance order, and every trigger in the catalog. Take the **longest assembled path**, not individual file size, as the baseline.
2. **Classify** — along two axes at once.
   - Role: routing / high-risk invariant / automation candidate / on-demand procedure / history
   - Expiry: **capability compensation** (has a shelf life) / **policy or boundary declaration** (capability-independent, keep)
3. **Retention gate** — apply the risk axes to the keepers. Do not cut mechanically by AND; a rare invariant with a high violation cost stays.
4. **Test the assumption** — for each capability-compensation sentence, state what it assumes the model cannot do, then **observe whether that still holds.** Pulling the sentence and running a representative task is the cheapest check. Never delete on a guess.
5. **Audit triggers** — confirm each skill or command description is written as a *firing condition* rather than a *topic*, and that it does not overlap others in the catalog. Overlapping triggers crowd each other out.
6. **Restore symmetry** — fill in a pre-authorization clause wherever a boundary states only prohibitions, but only when all three hold: reversibility, isolation, observability.
7. **Migrate before removing** — for rules leaving the surface but still required, create the home (lint, schema, generator, CI, on-demand doc) first and confirm actual fail-fast or recall. Distinguish retirement from relocation.
8. **Rewrite top-down** — leave only local deltas in the leaves; check claims against the code without copying the implementation snapshot back in.
9. **Check the completion contract** — for recurring request templates, confirm "done" is defined, the included scope is stated, and no phrasing biases toward stopping early.
10. **Propose → approve → apply** — keep / migrate / move / delete with reasoning, then stop. Apply only what is approved.
11. **Benchmark** — compare representative tasks before and after, holding non-instruction variables fixed (model, upper prompt, tools, memory, reasoning, sandbox, budgets) or the effect cannot be attributed.

## Output format

| Surface | Sentence/trigger | Role | Expiry (compensation/declaration) | Assumption · still true? | Action (keep/migrate/move/delete/symmetrize) | Rationale |
|---|---|---|---|---|---|---|

## Guardrails

- **Self-report is not evidence** — never use your own judgment that "models handle this now" as grounds for deletion. Delete only what was pulled and observed.
- **Policy is independent of capability** — boundaries for data loss, deployment, approval, permissions, and ownership stay.
- Never copy the same fact into two surfaces (SSoT). Link to the detail instead.
- Do not strip everything at once. Apply by category and benchmark.
- Do not skip the approval gate in step 10.
