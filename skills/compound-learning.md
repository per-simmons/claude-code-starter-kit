---
name: compound-learning
description: Use this skill after fixing a non-obvious bug, untangling a tricky workflow, or discovering a constraint Claude kept tripping over. It captures the lesson in a structured note so the same problem doesn't cost you another hour next month.
---

# Compound Learning

Every solved problem is an asset — but only if you write it down in a form a future agent (or a future you) can actually use. The default of "I'll remember this" doesn't survive context resets, model updates, or three weeks of unrelated work.

This skill turns one-off fixes into permanent leverage.

## When to capture

Capture when **any** of these are true:

- You spent more than ~20 minutes on something that, in hindsight, had a one-line answer.
- Claude went down a wrong path you've seen it go down before.
- A non-obvious constraint of a tool, file format, API, or library bit you.
- A specific phrasing or prompt structure unblocked something that was stuck.
- A workaround you'll forget by next quarter.

If the lesson is "obvious from reading the code now," skip it. The code is the record. Save learnings only for things that are *not* derivable from a fresh look at the project.

## Where to store

Create a `learnings/` folder at your project root (or `~/learnings/` for cross-project notes). One file per lesson, kebab-case names, and an `INDEX.md` at the top that lists all of them with a one-line summary.

Don't dump everything into a single mega-file — Claude will load it, get overwhelmed, and ignore the parts that matter. Many small files + a tight index works better.

## File template

```markdown
---
title: <Short, searchable title>
captured: YYYY-MM-DD
tags: [tool-name, pattern, mistake-type]
---

## Problem
One paragraph. What was happening, what you expected, what was surprising.

## Root cause
One paragraph. The actual underlying reason — not the symptom.

## Fix
The minimal change that resolved it. Paste the diff or command verbatim if useful.

## How to recognize this next time
A short list of tells: error messages, behaviors, contexts where this same pattern is likely to appear again.

## Related
Links to related learnings, docs, issue threads.
```

## How to write the lesson

Three rules:

1. **Lead with the symptom you'll search for.** Future-you will type the error message or the weird behavior into a search bar, not the elegant root cause. Make sure the symptom is the first thing in the file.
2. **Separate root cause from fix.** They're different things. The fix without the cause is cargo-culting.
3. **Be specific about the next-time tell.** "When you see X, suspect Y" is the highest-leverage sentence in any learning.

## Don't capture

- Things already documented in the code, READMEs, or commit messages.
- Things that change frequently (API versions, dependency lists, env var names) — they go stale fast.
- Things that are obvious from one read of the file in question.
- Anything you wrote just to feel productive. If you can't articulate when this lesson will fire again, you don't have one yet.

## Maintenance

Once a quarter, open `INDEX.md` and skim. For each entry:

- Still true? Keep.
- Outdated by a tool update? Delete or annotate.
- Pattern keeps recurring? Promote it into a proper skill.

Stale learnings are worse than no learnings — they confuse the model.

## Done criteria

- [ ] File lives under `learnings/` with a date in frontmatter
- [ ] Symptom appears in the first paragraph (so search finds it)
- [ ] Root cause and fix are separate sections
- [ ] "How to recognize this next time" has at least one concrete tell
- [ ] `INDEX.md` updated with a one-line summary
