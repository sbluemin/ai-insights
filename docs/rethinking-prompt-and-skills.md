# Rethinking Prompts and Skills: Runtime Context with an Expiry Date

> **In one line** — Instructions and skills are not documentation. They are an **always-loaded context router** that decides where to look next and which boundaries must not be crossed — and every steering sentence in them is a bet on the assumption *"the model can't do this on its own."* Minimize the total cost of constant injection, exploration, violation, rework, and maintenance; and because that bet **expires** when the capability baseline moves, re-audit rather than accumulate.

> 한국어판: [rethinking-prompt-and-skills.ko.md](./rethinking-prompt-and-skills.ko.md)

---

# Part I — What belongs there at all

## 1. Two roles are enough

Ordinary documentation is looked up when needed; always-on instructions charge you every time you enter the relevant scope. So give them only these two roles.

1. **Shrink the search space** — which area of responsibility or directory should I go to?
2. **Preserve boundaries** — which ownership, dependency-direction, trust, and approval invariants are easy to miss when reading code piecemeal?

Pile in feature descriptions, implementation inventories, task commands, and change history, and the instructions become a miniature documentation portal. From then on they inject stale priors over and over instead of aiding exploration.

## 2. A risk-weighted retention gate

Keep a sentence when the **expected benefit of knowing it in advance** exceeds the cost of injecting it constantly.

```text
expected benefit = exploration saved + (probability of late discovery × cost of violation)
total cost       = repeated injection cost + drift/maintenance cost
```

| Axis | Question to ask |
|---|---|
| **Need it up front** | Must this be known before the work or the edit? |
| **Late discovery** | Is ordinary exploration unlikely to surface it in time? |
| **Cost of violation** | If missed, does it cause a large security, data, deployment, approval, or ownership error? |
| **Stability and reuse** | Does it stay valid as the implementation changes, and does it recur in this scope? |
| **Alternative mechanism** | Is automation or on-demand routing cheaper and more reliable? |

Do not treat every axis as an equally weighted AND. An invariant that is rare but carries a very large violation cost, or that would be discovered far too late, can stay.

Mechanical rules that remain necessary should be **migrated first** to lint, schema, generators, or CI, and removed from the instructions only once there is evidence the mechanism blocks violations cheaply. Keep two judgments distinct: retiring a rule that is no longer valid, versus relocating where a still-valid rule is enforced.

## 3. Keep constraints, not descriptions

"What currently lives where" ages fast. "What must not cross into what, and which facts are too late to discover after reading the code" lives long. The same split separates invariants from procedure: **a procedure goes stale faster the more capability rises; an invariant does not.**

| Keep in the instructions | Send to another SSoT, or leave to the model |
|---|---|
| owner / must-not-own | Current implementation responsibilities and source layout |
| Allowed and forbidden dependency directions | Import and dependency lists |
| Trust / security / approval boundaries | Route, DTO, and field listings |
| The policy owner that must be singular | Current symbol and registry contents |
| The boundary between authoring source and generated output | Hand-copied contents of generated output |
| Required gates that prevent data loss or bad deploys | A full catalog of run commands |
| Lifetime semantics that cut across implementations | Operational procedure and incident retrospectives |
| **What must be true** — invariants, acceptance criteria | **How to get there** — the order of steps, which file to open first |
| Non-obvious pitfalls and **their reasons** | An enumeration of obvious checks |
| Cases where the order itself is a safety requirement | Cases where the order is merely convenience |

Written as trigger + action + **reason** (see [the harvest methodology §2③](./compounding-engineering-learning-harvest.md)), a rule lets the model re-decide when the context changes. A list of steps with no reason produces cargo cult.

For choosing a home for the details and migrating between homes, follow [the settling/SSoT methodology](./compounding-engineering-learning-harvest.md). Never copy the same fact into both the always-on instructions and the detailed doc.

## 4. An index only narrows the search space

The default resolution of an index is **directory, package, domain**. Its purpose is not to name the right file in advance but to narrow quickly the area worth searching.

An exhaustive file index duplicates what search already does and goes stale the moment the structure changes. Rare landmarks are still allowed for files that are **stable and expensive to miss**: the single policy SSoT, a composition root, the owner of generated output, an approval manifest. These are not a listing — they are a boundary that prevents starting to write or search in the wrong place.

## 5. Localize hierarchical context

Ancestor instructions are composed into every descendant context. A long parent is a tax on every sub-task, and it consumes the room the most specific leaf instruction needs first.

- Put only identity and global invariants in global instructions.
- Put a shared rule once, at the **lowest common ancestor** it applies to.
- A leaf adds only its **local delta**; it does not restate the parent.
- When two siblings repeat the same meaning, lift it to the common parent.
- Do not promote one sub-area's exception into a global rule for convenience.
- When several harnesses demand different instruction filenames, point them at one SSoT instead of copying.

Watch for **semantic** duplication more than literal repetition. Describing the same ownership, DI, or visibility boundary at several levels in different words still multiplies cost and drift.

---

# Part II — Why what belongs there goes stale

## 6. Steering only accumulates in one direction (a ratchet)

A steering sentence is usually a response to one past failure. But **the occasion to add one is observed; the occasion to delete one is not.**

- The model oversteps → an incident happens, and someone nails down a line: "from now on, never do ~."
- The model stops making that mistake → **nothing happens at all.** No notification, no failure.

So instructions lengthen like a one-way ratchet. Staleness announces itself as silence, which means **deletion happens only through periodic audit, never as an event.**

### When the capability baseline rises, the cost structure inverts

| | While capability was low | After capability rose |
|---|---|---|
| Dominant failure | **Under-instruction** — omissions, no verification, wrong files | **Over-instruction** — needless reads and re-runs, narrowed scope, premature stops |
| Effect of the sentence | Prevents failure | Makes the model **redo** what it would do anyway, or **blocks** what it should do |
| Visibility of the cost | High (surfaces as an incident) | **Low** (surfaces only as slower and less) |

The crucial part is that the new cost is *quiet*. Over-steering throws no errors. It just makes every task slightly more expensive and occasionally halts work that should have run to completion.

### Re-audit triggers

> When you see the following → re-audit instructions and skills wholesale → because a capability assumption can go silently invalid without ever raising a signal.

- **The capability baseline changed** — a swap, an upgrade, a configuration change. The clearest occasion.
- **Recurring surplus behavior under the same instructions** — a sign you are asking for something the model already does.
- **Monotonic growth in instruction length** — if the last several months held only additions and no deletions, you are already late.

## 7. The skill surface: the constant cost is in the trigger, not the body

A skill (procedure doc) splits into two parts that **charge you at different times**.

| Part | When it costs | Design goal |
|---|---|---|
| **Recall trigger** (name, description) | **Constantly** — as long as it is in the catalog | Fire precisely, in minimal length |
| **Body** | Only when it fires | Unfold only as much as needed |

### ① Write the trigger as a *firing condition*, not a *topic*

| | Example |
|---|---|
| **Bad** | "Create and validate schema migrations. **Use when working with databases, queries, models, or persistence.**" |
| **Good** | "Create and validate schema migrations. **Use when adding or changing a migration, or reviewing its rollout.**" |

The bad version raises its hand for everything adjacent. A trigger must say **when it turns on**, not **what it is about**, or it will misfire.

### ② The trigger budget is a shared resource

All triggers in a catalog share one space. As skills multiply, each description gets truncated or diluted and **routing quality degrades globally**. In other words, one skill's verbose description *crowds out someone else's skill*. Judging "more detail is better" from your own skill alone produces a tragedy of the commons.

→ When adding a skill, ask: **"Does this description interfere with another skill firing? Do any triggers overlap?"**

### ③ The entry file is a router, not a document

For a skill spanning several workflows, keep the top-level file a **thin router** — it says where things are, but **does not demand that everything be read**. A monolithic body burns context the instant it fires and brings compaction forward. Push detail into sub-documents and scripts that open only when needed. This is §4's index rule applied to the skill surface.

## 8. The sentences that go stale first

| Form | The assumption underneath | What it costs now | Replacement |
|---|---|---|---|
| **Mandatory pre-reads** — "read A, B, and C before any edit" | It can't pick the documents it needs | Loads the whole map even for a typo fix. A fixed charge on every task | **Conditional pointers** — "A for service boundaries, B for schema changes, C when preparing a deploy" |
| **Verification nudges** — "always test and check the result" | It declares completion without checking | Asks for what it already does, causing **redundant runs** | Delete. Keep only *non-standard* verification requirements |
| **Broad ask-first** — "always ask before changing anything" | It reaches beyond what was asked | Approval requests at every step; premature ending of work | §9 — write prohibitions and **pre-authorizations** together |
| **Step-by-step recipes** | It can't plan on its own | Blocks shorter paths | §3 — keep only invariants and acceptance criteria |
| **Exhaustive mini source tree** | Search is unreliable | Goes stale on any structural change and pre-injects the wrong location | §4 — directory-level index plus rare landmarks |
| **Feature snapshot** | It can't read the current state | Mistakes today's UI/API for a long-lived invariant | Send to code or on-demand docs |
| **Restating the ancestor** | The child might not receive the parent | Multiplies cost and semantic drift across every sub-task | §5 — local delta only |
| **Accumulating prohibition lists** | Every past failure could recur | Carries a deleted past into constant context forever | Retire with the failure mode |

> ⚠️ **Not all steering goes stale.** Operational invariants with a high violation cost (data loss, deployment, approval, permission boundaries) stay no matter how good the model gets. A model cannot infer *your organization's policy*. What goes stale are **sentences that compensated for capability**; what stays are **sentences that declared policy and boundaries**. This distinction is the central axis of the audit.

---

# Part III — Boundaries, completion, portability

## 9. Boundaries are asymmetric — write as much permission as prohibition

Boundary sentences usually record only "do not cross." The cost of crossing shows up as an incident, while **the cost of stopping when it shouldn't have is invisible.** The more capable the model, the more it over-honors boundaries — standing there asking for approval at a point where continuing would have been welcome.

→ Write the boundary definition in **both directions**.

```
For <class of work that is reversible and isolated in effect>, carry it through without asking for approval at each step.
Stop and report when you reach <boundary condition>.
```

**Qualifying conditions for pre-authorization** — all three must hold:

1. **Reversibility** — can it be undone?
2. **Isolation** — does the effect stay within disposable, local scope (never reaching production, shared state, or external services)?
3. **Observability** — if it goes wrong, does that surface immediately?

If any one fails, keep the gate. Actions that go outward (publishing, sending, deploying) and deletions or overwrites that are hard to undo remain approval-bound regardless of capability.

## 10. The completion contract: define "done" or the model's stopping tendency defines it for you

Where a model stops varies by model. One baseline runs alone for a long stretch; another comes back for review with work still left. **A request that leans on that tendency breaks when the baseline changes.** So pin "done" down in the *request*, not in the instructions.

- **Define the completion condition first** — what must be true for this to be finished?
- **State the scope included in the work** — if running it, checking the result, fixing failures, and re-checking are part of the job, say so.
- **Re-examine "get review after a first pass" phrasing** — it biases toward early termination. Confirm that checkpoint is genuinely wanted; if not, remove it.
- **Give exploratory work both a scope and a stopping point** — "how far to dig, and what you will see when you stop."

## 11. Portability: instructions are read by many models and many people

A repository's instructions do not steer only the author's own setup. Colleagues on a different baseline, and your future self, receive the same sentences. **A sentence added to compensate for one baseline's weakness over-constrains another.**

- **Separate them** — model-invariant content (boundaries, invariants, ownership, policy) goes in the always-on instructions; model-dependent compensation stays minimal, **marked as provisional**, and isolated.
- Record **why** each compensating sentence exists. Without the reason, the next audit cannot judge whether to delete it.

---

# Part IV — Auditing

## 12. The audit

**The one test that drives every verdict:**

> Ask of each sentence — **is this compensating for capability, or declaring policy and boundaries?**
> Policy/boundary declarations are capability-independent: keep them. Capability compensation has an expiry: expose the assumption and observe whether it still holds.

### Procedure

1. **Inventory** — collect every instruction file, alias, and root-to-leaf inheritance order. Take the **longest assembled path**, not individual file size, as the baseline.
2. **Classify** — along two axes at once: role (`routing / high-risk invariant / automation candidate / on-demand procedure / history`) and expiry (compensation / declaration).
3. **Retention gate** — apply §2 to the keepers.
4. **Test the assumption** — for each compensating sentence, state what it assumes the model cannot do, then **observe**. Pulling the sentence and running a representative task is the cheapest check. Never delete on a guess.
5. **Audit triggers** — §7: firing conditions, not topics; no overlap across the catalog.
6. **Restore symmetry** — §9: fill in pre-authorization wherever only prohibitions exist.
7. **Migrate before removing** — for rules leaving the instructions but still required, create the home (automation, docs, live metadata) first and confirm actual recall or fail-fast.
8. **Rewrite top-down** — leave only deltas in the leaves; check the claims against the code without copying the implementation snapshot back in.
9. **Verify the assembly** — confirm the first heading and last sentence of a representative leaf are both visible in the model's actual context; leave headroom below the observed limit; confirm aliases point at one SSoT.
10. **Propose → approve → apply** — keep / migrate / move / delete with reasoning. Apply only what is approved.

### Verifying that it actually got better

Assembly checks alone do not show that engineering quality improved. Repeat the same representative tasks under verbose / light / risk-weighted hybrid conditions and compare:

- Assembled tokens and whether truncation occurred
- Time, search calls, and volume read before reaching the correct owner/SSoT
- Task success and the accuracy of the first edit
- Critical violations, review findings, rework count

Hold non-instruction variables fixed — model, upper system/developer prompt, tool catalog, memory and retrieval policy, reasoning settings, sandbox, search/time budgets. Otherwise you cannot separate the effect of shorter instructions from some other context increasing exploration. Keep the trial that reproduces real operating conditions separate from the controlled one that isolates the instructions.

**Success criterion:** task success and critical violations are non-inferior, each cost axis degrades within a pre-agreed tolerance, and the **weighted total cost** of injection, exploration, rework, and maintenance goes down. The key metric is not one corpus-wide sum but the worst assembled path combined with real task outcomes.

You can hand the audit itself to the model. **But delegate the candidate proposal, not the verdict** — a model's self-report about its own capability is not evidence.

## 13. Guardrails

1. **Grounds for deletion are observations, not reputation** — "the latest models handle this" is not evidence. Delete only what you pulled out and checked.
2. **Policy is independent of capability** — boundaries and approval gates with a high violation cost stay as models improve. Only capability compensation goes stale.
3. **Migrate before removing** — retiring a rule and relocating its enforcement are different judgments. Confirm the mechanism blocks before deleting the sentence.
4. **Do not strip everything at once** — delete in a batch and you will not know what caused the regression. Apply by category and measure.
5. **Write less only as a consequence** — the goal is not token savings but that the most specific and most important boundaries actually reach the model when needed, without raising the cost of omission.
6. **Encoding and deletion are a pair** — every time you add new steering, find one piece of stale steering (→ [the harvest methodology §2④](./compounding-engineering-learning-harvest.md)).
