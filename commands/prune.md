---
description: Sweep instructions, docs, and artifacts and propose deletions for what is stale, wrong, or duplicated
argument-hint: "<target directory/file — defaults to all repository instructions>"
---

Apply the pruning step (④) of the `learning-harvest` skill.

Target: $ARGUMENTS (if empty, all always-on instructions and documents in this repository)

Classify every entry as **keep / update / delete** with reasoning, present it as a table, and stop.
Judge by "few strong rules" — put weak or duplicated entries, and entries that contradict the current code or behavior, up as deletion candidates.
When the same fact sits in two homes, pick the better one and list the rest for deletion.
Delete nothing before approval.
