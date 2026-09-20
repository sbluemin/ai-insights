---
description: Audit the steering surfaces — instruction files, skill/command triggers, and request templates
argument-hint: "[path or surface — auto-discovered if omitted]"
---

Use the `rethinking-steering` skill to audit the steering surfaces.

Target: $ARGUMENTS — if empty, discover all three in this repository:

- always-on instruction files at any level (any filename, including inherited or scoped ones)
- the skill, command, and subagent catalog (names, descriptions, bodies)
- recurring request templates, if the repository keeps any

Deliverables:

1. A per-sentence classification table (surface / role / expiry / the assumption underneath and whether it still holds / action / rationale)
2. Rules that can be **migrated** to automation (lint, schema, generators, CI) — do not remove them before confirming the mechanism actually blocks
3. Trigger conflicts across the catalog — descriptions written as topics, and overlapping firing conditions
4. **Pre-authorization** clauses for boundaries that state only prohibitions (only where reversibility, isolation, and observability all hold)
5. Completion-contract gaps in request templates — undefined "done", unstated scope, phrasing that biases toward stopping early
6. A trimmed draft, plus the representative-task benchmark that would confirm it

Do not use "models are good at this now" as grounds for deletion; mark anything unobserved as "needs verification".
Do not edit any file before approval.
