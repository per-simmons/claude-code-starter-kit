# ACE: Six-Phase Workflow

Six phases I run on anything bigger than a one-line task. Not a tool, not a slash command — a discipline. Run it in your head, in a checklist, or as literal sub-agents.

```
research → plan → implement → validate → review → compound
```

## Phase 1 — Research

Understand the problem before changing anything.

- What's the actual ask? Restate in your own words.
- What inputs exist? Files, data, APIs, prior outputs.
- What's the desired output? Format, length, audience.
- What constraints are non-obvious? Tooling quirks, deadline, style.

Output: a `brief.md`. Write it to disk. The act of writing forces specificity that thinking-out-loud doesn't.

## Phase 2 — Plan

Use plan mode (`Shift+Tab` in Claude Code). Claude produces a step-by-step plan and runs nothing.

Read it critically:
- Steps in the right order?
- Does any step assume something not yet established?
- What could go wrong at each step?

Iterate on the plan, not on the prompt. Cheaper here than later.

## Phase 3 — Implement

Execute the plan, one step at a time.

- Don't let Claude bundle steps. Eight-step plan means eight visible steps.
- Verify each step's output before moving on.

If a step veers off, fix the plan. Don't paper over with a new prompt.

## Phase 4 — Validate

Confirm each requirement from the brief is met. Use evidence — quoted lines, row counts, screenshots — never just a "looks good" claim. For non-trivial work, spawn a fresh agent (see `qa-multi-agent.md`) so the verifier hasn't inherited the implementer's assumptions.

Anything that fails goes back to Phase 3 with the specific failure attached.

## Phase 5 — Review

Human eyes on the validated output. Validate proves it meets the spec. Review asks the questions the spec didn't:

- Does this look right?
- Does it tell the story I wanted?
- Anything technically correct but practically misleading?

Claude can help (a fresh agent reading for tone, gaps, framings) but the final read is yours.

## Phase 6 — Compound

Capture what you learned so the next run is cheaper.

Write a short note when:
- You spent more than ~20 minutes on something with a one-line answer in hindsight.
- Claude went down a wrong path you've seen before.
- A non-obvious constraint of a tool, format, or API bit you.
- A specific phrasing or structure unblocked something stuck.

Skip the capture if the lesson is obvious from a fresh read of the project. Only save things not derivable from looking at the code.

### Where

A `learnings/` folder, one file per lesson, with an `INDEX.md` listing them. Many small files plus a tight index works better than one mega-file (which Claude partially ignores).

### Format

```yaml
---
title: Searchable title
captured: YYYY-MM-DD
tags: [tool, pattern, mistake-type]
---
```

```markdown
## Symptom
What was happening, what you expected, what was surprising. Lead with this — future-you will search for the symptom, not the elegant root cause.

## Root cause
The actual underlying reason, not the surface fix.

## Fix
The minimal change. Paste diff or command verbatim if useful.

## How to recognize this next time
Concrete tells: error messages, behaviors, contexts where this pattern recurs.
```

### Maintenance

Once a quarter, prune. Stale learnings confuse the model worse than missing ones.

## When to skip phases

Throwaway scripts, one-line edits, exploratory prompts where you want the model's first instinct: skip everything except Implement.

Anything with a deliverable, anything you'll look at twice, anything where wrong answers cost you: run all six.
