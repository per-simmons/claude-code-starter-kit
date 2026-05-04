---
name: qa-multi-agent
description: Use this skill after any non-trivial task to verify the work was actually done. Solves the failure mode where Claude claims completion but missed requirements, skipped steps, or hallucinated output. Spawns separate fresh-context agents for definition and execution so the verifier cannot inherit the implementer's blind spots.
---

# qa-multi-agent

The most common Claude failure is false completion: the model says "done" while quietly skipping requirements. Asking the same session to check its own work doesn't help — it's already convinced itself.

The fix is architectural: split verification across separate agents with separate context windows.

## Pipeline

```
IMPLEMENTATION AGENT finishes work
        │
        ▼
DEFINITION AGENT   (fresh context, never saw the work)
  Reads the original request + the artifact
  Writes a specific checklist of what "done" means
  Output: qa-checklist.md
        │
        ▼
EXECUTION AGENT    (fresh context, never saw definition being written)
  Reads qa-checklist.md and the artifact only
  Walks each item, marks PASS or FAIL with evidence
  Evidence = a quoted line, a row count, a screenshot, etc.
  Output: qa-results.md
        │
        ▼
VERDICT
  All PASS  → ready for human review
  Any FAIL  → loop the failures back to implementation
```

## Why fresh context matters

If the execution agent inherits the definition agent's context, it inherits assumptions — including the assumption the work is good. A fresh agent reading only the checklist plus the output has no allegiance to the implementer.

Practically: spawn each agent as a sub-task with its own clean prompt, not as a continued conversation in the same session.

## How to invoke

Trigger phrases: "QA this", "verify the work", "check if that's actually done".

1. Identify the artifact. What file, output, dataset, deck, or change set is under test?
2. Spawn the definition agent. Give it the original request + the artifact. Tell it to produce a verifiable checklist (each item PASS/FAIL, not subjective). Save to `qa-checklist.md`.
3. Read the checklist yourself before continuing. 30-second gate, catches checklists that ask the wrong questions.
4. Spawn the execution agent. Give it only the checklist + the artifact. Each item must include evidence — quoted snippet, count, screenshot path, diff hunk.
5. Read the results yourself. All-pass → hand to human. Any fail → send back to implementer with evidence attached.

## What "verifiable" looks like

| Vague (rejected) | Verifiable (accepted) |
|---|---|
| "Analysis covers all months" | "File contains rows for 2023-01 through 2024-12, 24 rows" |
| "Slides are formatted correctly" | "Every slide has a title in 28pt and a footer with the page number" |
| "Numbers match the source" | "YoY growth on slide 4 equals (output[2024] / output[2023]) - 1, rounded to 2 dp" |

Every checklist item must be answerable as a hard PASS or FAIL with evidence. If it can't be, rewrite it.

## Optional: visual verification with Playwright

The default flow above only needs file reads. If the artifact is *rendered* — a slide deck, webpage, dashboard, chart — file reads aren't enough. The execution agent needs to see what a human sees.

Add Playwright in one of two ways:

- **Playwright MCP** — installs as an MCP server in Claude Code. The execution agent gets tools like `browser_navigate`, `browser_screenshot`, `browser_snapshot`. It opens the URL or local HTML, screenshots each slide/page, and checks against the checklist.
- **Playwright CLI** — if you'd rather not run an MCP, the agent can shell out to `playwright` directly via Bash. Same outcome, less ergonomic.

The orchestration is identical. Only the evidence type changes — screenshot path instead of row count.

| Checklist item | Evidence |
|---|---|
| "Slide 4 chart x-axis covers Jan–Dec 2024" | screenshot of slide 4 showing axis labels |
| "Page header is 28pt navy blue" | screenshot + computed style snapshot |
| "Submit button enables after form fills" | screenshots before/after |

For pure data work (spreadsheets, CSVs, documents), skip Playwright. For visual deliverables, wire it in.

## When to skip

For one-line edits, throwaway exploration, or anything you'll inspect by hand in 10 seconds, don't bother. This is for work too big to eyeball, too important to ship blind, or that has burned you before.

## Done criteria

- [ ] `qa-checklist.md` exists, every item is verifiable
- [ ] `qa-results.md` exists, every item has evidence (not just PASS/FAIL)
- [ ] All FAIL items have been re-implemented and re-verified, or explicitly flagged to the human
- [ ] Final summary names what was verified and what was not
