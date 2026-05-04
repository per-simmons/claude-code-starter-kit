# The Phased Workflow

Most people use Claude Code as a chat box: type a request, hope, retry, repeat. That works for tiny tasks and falls apart on anything real. The fix isn't a smarter prompt — it's structure.

This is a six-phase mental model. Not a slash command, not a tool. A discipline. You can run it in your head, in a checklist, or as a literal sequence of sub-agents. It applies to code, to spreadsheets, to slide decks, to any work where Claude needs more than one round-trip to get it right.

```
research → plan → implement → validate → review → compound
```

## Phase 1 — Research

**Goal:** understand the problem space before changing anything.

- What's the actual ask? (Restate it in your own words.)
- What inputs exist? (Files, data sources, APIs, prior outputs.)
- What's the desired output? Be specific — format, length, audience.
- What constraints are non-obvious? (Tooling quirks, deadline, style.)

Output of this phase is a short brief — a paragraph or two. Write it to disk as `brief.md` and let Claude read it for the rest of the workflow. Don't skip writing it. The act of writing forces specificity that thinking-out-loud doesn't.

## Phase 2 — Plan

**Goal:** decide *how* before doing *what*.

Use plan mode (`Shift+Tab` in Claude Code). It produces a step-by-step plan without executing anything. Read the plan critically:

- Are the steps in the right order?
- Does any step assume something that hasn't been established?
- What could go wrong at each step?

If the plan is wrong, the implementation will be wrong, no matter how clever the model is. Iterate on the plan until it actually matches what you want. Cheaper here than later.

## Phase 3 — Implement

**Goal:** execute the plan, one step at a time.

Two rules:

1. **Don't let Claude run away.** If the plan had 8 steps, you should see 8 steps in the implementation, not one big "I did it all" lump. If Claude is doing too much per step, stop and break the step down.
2. **Verify each step's output before moving on.** Open the file, read the rows, look at the chart. Catching a wrong assumption at step 2 saves redoing steps 3–8.

If a step fails or veers off, go back to the plan and fix the plan. Don't paper over with a new prompt.

## Phase 4 — Validate

**Goal:** confirm each requirement from Phase 1 is actually met.

This is where most workflows die — Claude declares "done" and the human nods. Don't.

For each requirement in `brief.md`, write down: *how would I prove this is true?* Then prove it. Spawn a fresh agent (see `skills/qa-multi-agent.md`) if the work is too big to eyeball. Use evidence — quoted lines, row counts, screenshots — never just a pass/fail claim.

Anything that fails goes back to Phase 3 with the specific failure attached.

## Phase 5 — Review

**Goal:** human eyes on the validated output before it goes anywhere.

Validate proves the work meets the spec. Review asks the questions the spec didn't:

- Does this look right?
- Does it tell the story I actually wanted to tell?
- Is anything technically correct but practically misleading?

Claude can help with Review (a fresh agent reading for tone, gaps, weird framings) but the final read is yours. Reviewing is the part that requires judgment, and judgment is the one thing not to outsource.

## Phase 6 — Compound

**Goal:** make the next run cheaper.

Before closing the loop, write down what you learned. Use the `compound-learning` skill. Specifically:

- Did Claude get stuck on something that's likely to recur? Capture the tell + the fix.
- Did your plan mid-iterate in a particular way? Note the pattern.
- Was a skill ignored or misapplied? Sharpen the skill while it's fresh.

Without Phase 6, you pay full price every time you do this kind of work. With Phase 6, the third run is half the cost of the first.

## When to use the full loop, and when to skip phases

Full loop: anything you'll review more than once, anything with a deliverable, anything where wrong answers cost you.

Skip-to-implement: throwaway scripts, one-line edits, exploratory prompts where you'd rather see the model's first instinct.

The skill is knowing which is which. Default to running the full loop the first few times you tackle a new kind of task; you'll quickly learn where you can compress.

## A note on fresh context

Phases 4 and 5 are dramatically more useful when you spawn fresh agents (sub-tasks with clean context) instead of asking the same Claude session that did the implementation. The implementer has already convinced itself the work is good; a fresh agent reads with no allegiance. This is the single highest-leverage trick in the entire workflow.
