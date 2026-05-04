---
name: qa-multi-agent
description: Use this skill after Claude finishes any non-trivial task to verify the work was actually done. Solves the core failure mode where Claude claims completion but missed requirements, skipped steps, or hallucinated output. Spawns separate fresh-context agents for definition and execution so the verifier cannot inherit the implementer's blind spots.
---

# QA Multi-Agent Skill

The single most common Claude failure is **false completion**: the model says "done!" while quietly skipping half the requirements. Asking the same Claude session to check its own work doesn't help — it has already convinced itself the work is done.

The fix is architectural, not prompt-tuning: **split verification across separate agents with separate context windows**.

## The pipeline

```
┌──────────────────────────────────────────────────────────────┐
│ IMPLEMENTATION AGENT finishes work                           │
│ (this is the one that just claimed "done")                   │
└───────────────────────────┬──────────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────────┐
│ AGENT 1 — DEFINITION (fresh context, never saw the work)     │
│ Reads the original request + the changes/output              │
│ Writes a SPECIFIC checklist of what "100% done" means        │
│ Output: qa-checklist.md                                      │
└───────────────────────────┬──────────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────────┐
│ AGENT 2 — EXECUTION (fresh context, never saw definition     │
│            being written)                                    │
│ Reads qa-checklist.md and the actual output                  │
│ Walks each item, marks PASS or FAIL with evidence            │
│ Evidence = a quoted line, a row count, a screenshot, etc.    │
│ Output: qa-results.md                                        │
└───────────────────────────┬──────────────────────────────────┘
                            ▼
┌──────────────────────────────────────────────────────────────┐
│ VERDICT                                                      │
│ All PASS  → ready for human review                           │
│ Any FAIL  → loop the failures back to implementation         │
└──────────────────────────────────────────────────────────────┘
```

## Why fresh context matters

If Agent 2 inherits Agent 1's context, it inherits Agent 1's assumptions — including the assumption that the work is good. A fresh agent reading only the checklist + the output has no allegiance to the implementer and will read the output literally.

Practically: spawn each agent as a sub-task with its own clean prompt, not as a continued conversation in the same session.

## How to invoke this skill

Trigger: "QA this", "verify the work", "check if that's actually done".

Procedure:

1. **Identify the artifact under test.** What file, output, deck, dataset, or change set are we verifying?
2. **Spawn DEFINITION agent.** Prompt it with: the original request, the artifact, and an instruction to produce a verifiable checklist (each item must be PASS/FAIL, not subjective). Save to `qa-checklist.md`.
3. **Read the checklist yourself before continuing.** This 30-second gate catches checklists that ask the wrong questions.
4. **Spawn EXECUTION agent.** Prompt it with: only the checklist and only the artifact (not the original request, not the implementer's notes). Each item must include evidence — a quoted snippet, a count, a screenshot path, a diff hunk.
5. **Read the results yourself.** If everything passes, hand to the human. If anything fails, send the failures back to the implementer with the evidence attached.

## What "verifiable" looks like

| Vague (rejected) | Verifiable (accepted) |
|---|---|
| "Analysis covers all months" | "qa-checklist row 1: file contains rows for 2023-01 through 2024-12, 24 rows" |
| "Slides are formatted correctly" | "every slide has a title in 28pt and a footer with the page number" |
| "Numbers match the source" | "the YoY growth figure on slide 4 equals (output[2024] / output[2023]) - 1, rounded to 2 dp" |

Every checklist item must be answerable as a hard PASS or FAIL with evidence. If it can't be, rewrite it.

## When to skip this skill

For one-line edits, throwaway exploration, or anything you'll inspect by hand in 10 seconds, don't bother. This pattern is for work that is too big to eyeball, too important to ship blind, or has burned you before.

## Done criteria

- [ ] `qa-checklist.md` exists, every item is verifiable
- [ ] `qa-results.md` exists, every item has evidence (not just PASS/FAIL)
- [ ] All FAIL items have been re-implemented and re-verified, or explicitly flagged to the human
- [ ] Final summary names what was verified and what was not
