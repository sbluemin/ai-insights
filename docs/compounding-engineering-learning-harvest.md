# Compounding Engineering: Feeding a Task's Learnings Back into Durable Artifacts

> **In one line** — When a task ends, feed what you learned into *durable artifacts* (always-on instructions, procedure docs, tests, automation hooks) so the next task starts from a higher baseline. A working session's context is volatile; left alone, the learning evaporates. **Compounding is only positive when signal accumulates faster than noise.**

This document covers the general method for making an engineering collaboration with an AI agent *improve itself from what the last task taught it*. It stays at the level of principle — nothing tool- or project-specific.

> 한국어판: [compounding-engineering-learning-harvest.ko.md](./compounding-engineering-learning-harvest.ko.md)

---

## 1. Framing: work happens at three orders

| Order | What it is | Example |
|---|---|---|
| **First** | Do the work | Ship a feature, fix a bug |
| **Second** | Fix the *system* that does the work | Improve the procedures, tools, and instructions used for that work |
| **Third** | Decide how the system gets fixed | ← this methodology |

Most collaboration never leaves the first order. Compounding hinges on the habit of **settling the by-products of first-order work — friction, discoveries, judgments — into second-order artifacts**, and the third order is what standardizes that habit. Encode the third order once and every task afterwards produces second-order output on its own.

---

## 2. The harvest loop: extract → settle → encode → prune

### ① Extract — what is worth keeping (the noise filter)
A candidate qualifies only if **all three** hold. Otherwise drop it.
- **Recurrence** — will this happen again? (One-off coincidences and environment-specific numeric edges are out.)
- **Cost** — was it non-obvious, or did it actually eat time?
- **Generality** — does it generalize beyond this instance?

> ⚠️ **Guard against overfitting.** Keeping everything contaminates the artifact. Never nail a single incident down as a general rule. Apply the same restraint that separates *accept* from *reject* in code review to selecting learnings.

### ② Settle — where it belongs (SSoT routing)
Map each kind of learning to **the single home whose recall trigger matches**. When the home and the moment of recall are misaligned, the knowledge is dead on arrival.

| Kind of learning | Home (described by function) | When it is recalled |
|---|---|---|
| Invariant that must always hold | **Always-on instructions** for that scope (agent instruction file) | Constantly, while working in that scope |
| "How to do X" procedures and pitfalls | **Procedure doc / skill** | When X fires |
| A specific bug or regression | **A test** | Every build (it executes) |
| The *reason* behind a decision | **Decision record** (ADR and the like) | Looked up to prevent re-litigation |
| Personal preference, way of working | **Persistent memory** | Relevance-based recall |
| Project context needed at all times | **Always-loaded instructions** | Every session |

> **Principle: one home, no duplicates. When a better home appears, *migrate*.**
> If a fact first landed in scratch memory and a more fitting procedure doc later records it properly, **delete the scratch memory**. The same fact in two places means both rot.

### ③ Encode — how to write it
Don't nail down the conclusion alone. Write **trigger + action + reason**.
> `"When you see <symptom> → do <action> → because <failure mode>."`
> (e.g. "When the same input yields non-deterministic results → pin the seed and ordering → because hidden external state is racing in.")

The reason is what lets **your future self re-decide when the context has changed** (this is what prevents cargo-culting). Keep it minimal and skimmable.

### ④ Prune — the precondition for compounding
Periodically sweep the artifacts and **delete rules that are stale or wrong**. Few strong rules beat many weak ones. Encoding and deletion are a pair — encoding without deletion is debt.

---

## 3. The hierarchy of compounding value — push as far toward 'executable' as you can

| Layer | Effect | Form |
|---|---|---|
| Low — **fact / note** | Prevents rediscovery | A pitfall memo |
| Medium — **rule / procedure** | Standardizes the approach | Checklists, procedure steps |
| High — **executable artifact** | Removes the work itself | Tests (auto-verify) · scripts (auto-perform) · **hooks** (auto-fire) |

Replacing *fixed-interval polling* with *a deterministic script that wakes only on an event* encodes a **mechanism** rather than a *fact*, so it compounds far more. When encoding, always ask: **"Can this learning be pushed down to a more executable layer?"**

---

## 4. Prompt patterns for asking an AI

### (a) A harvest request with a judgment gate — one level above "go apply it"
> "Propose this task's learning candidates first — with ① recurrence/cost/generality, ② the home, ③ the encoded wording. Persist only the ones I pick."

→ Inserting *propose → approve* instead of applying directly blocks overfitting.

### (b) Reverse-engineering probe — when you don't know what the learning is
> "Where did this task **eat time**, and **which artifact, had it already said so, would have made that trivial**?"

→ Friction points are the encoding candidates.

### (c) Pressure toward mechanism
> "Can this learning be encoded at the **most executable layer** (test/script/hook)? If not, why does it stay at the procedure/note level?"

### (d) Verification first
> "Before encoding, did you **prove the learning actually holds**? If it's a guess, don't encode it — verify first."

→ Verify a mechanism end to end *before* nailing it into a procedure. Keep only what you proved.

---

## 5. Ready-to-use prompts (copy-paste)

The §4 patterns, transcribed so you can drop them straight into a workflow. Fill in the `<…>` only.

### Example 1 — full harvest at task end (with judgment gate) · *the one you'll use most*
> When: right after finishing substantive work, to delegate the whole question of what to keep.
```
Let's harvest the reusable learnings from the work we just did. Don't apply anything yet — propose a candidate list first. For each candidate give:
1) a one-line summary
2) extraction verdict — does it satisfy recurrence, cost, and generality? (if any one fails, mark it "drop")
3) home — which durable artifact it belongs in (always-on instructions / procedure or skill / test / decision record / memory) and why
4) encoded wording — "When you see <symptom> → do <action> → because <failure mode>"
Also flag any overlap with existing artifacts. I will persist only the items I pick.
```

### Example 2 — reverse-engineer the learning from friction
> When: the work is done but you don't know what the learning was.
```
Name at most three points in this task that ate time or were non-obvious. For each, say whether it would have been trivial had some durable artifact already recorded it, and propose the one-line rule (trigger + action + reason) that would go into that artifact.
```

### Example 3 — pressure toward mechanism
> When: the learning is stuck at the level of a "be careful" note.
```
Propose how to encode this learning at the most executable layer. Work down the order test (auto-verify) > script/automation (auto-perform) > hook (auto-fire) > procedure doc > plain note. If a lower (more executable) layer is possible, take it; if not, state why it has to stay higher.
```

### Example 4 — pruning (maintenance)
> When: artifacts have piled up and the signal is getting muddy.
```
Sweep <target artifact/directory> and identify entries that are stale, wrong, or no longer apply. For each, propose keep / update / delete with the reasoning. Judge by "few strong rules" — put weak or duplicated entries up as deletion candidates.
```

### Example 5 — making it automatic (the meta-move, one-off)
> When: you don't want to ask for a harvest every time — nail it down once.
```
From now on, whenever substantive work finishes, I want you to propose the harvest loop above (candidates → my approval → persist) on your own. Propose how to settle this as always-loaded instructions or as a task-completion hook, and apply it once I approve.
```

---

## 6. The meta-move: encode the methodology itself

The biggest compounding win is **no longer having to explain this loop every time**.
- Put one line in the **always-loaded instructions** — "when substantive work ends, *propose* the §2·§4 harvest loop yourself" — and from the next task on, the agent raises candidates unprompted.
- Trigger "propose a learning harvest" from a **task-completion hook** (fires automatically on finish) and the harvest itself becomes a mechanism (top of the hierarchy).
- Or turn it into a **dedicated command/skill**.

Encode the third order once and second-order production automates itself.

---

## 7. Guardrails

1. **Block overfitting** — the three conditions (recurrence, cost, generality) plus a propose/approve gate.
2. **Verify first** — encode only what you proved; no guessing.
3. **SSoT and migration** — one home, no duplicates; when a better home appears, migrate.
4. **Prune** — encoding and deletion are a pair. Compounding is positive only while signal > noise.
5. **Preserve the reason** — trigger + action + reason, so it can be re-decided later.

---

## Appendix A — a generalized worked example

One cycle seen as an abstract scenario. (Look at the *shape*, not at any particular tool or project.)

| Friction/discovery (in first-order work) | Extraction verdict | Home (second order) | Encoded form |
|---|---|---|---|
| Fixed-interval polling is wasteful (most checks are empty) | recurrence · cost · generality ✓ | Procedure/automation | A deterministic script that wakes only on an event *(mechanism)* |
| An unstable identifier used to judge a non-deterministic signal (an external party reshuffles identifiers) | ✓ | Procedure rule | "Judge only by stable counts and timestamps" |
| A non-obvious pitfall in a tool (directly injected state gets overwritten by internal logic) | ✓ | Procedure gotcha | "Reach the state through the internal code path" + the reason |
| Environment restore works only when specific preconditions hold | ✓ | Setup recipe | Spell out the seed preconditions |
| A one-off edge that appears only at a particular value or environment | generality ✗ | — | **Not encoded** |
| The same fact sitting in two homes | Home improvement | Migrate to the better fit | Delete the duplicate |

→ The core pattern: **friction → verification → encode as mechanism/rule → settle in the right SSoT → remove duplicates.**
