---
description: Harvest learnings from the work just done and propose how to persist them (with an approval gate)
argument-hint: "[task/scope — defaults to this session]"
---

Use the `learning-harvest` skill to run the harvest loop.

Target: $ARGUMENTS (if empty, all substantive work done in this session)

Rules:

1. **Propose the candidates as a table first**, then stop. Do not edit any file without approval.
2. For every candidate fill in: the three-condition verdict (recurrence, cost, generality), the home, the encoded wording (`When you see <symptom> → do <action> → because <failure mode>`), and any overlap with existing artifacts.
3. For every candidate ask "can this go to a more executable layer (test > script > hook > procedure > note)?" and state why if it cannot.
4. Exclude unverified guesses, or mark them "needs verification".
