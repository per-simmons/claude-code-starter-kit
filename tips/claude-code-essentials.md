# Claude Code Essentials

A one-pager of the things that make the biggest difference in day-to-day Claude Code usage. None of this is in the welcome screen, all of it is high-leverage.

## File formats

**Excel is hostile to LLMs.** Cells, formulas, merged ranges, and binary `.xlsx` structure waste tokens and confuse the model. Anything beyond a trivial spreadsheet is a CSV problem dressed up as an Excel problem.

Default move: ask Claude to convert the input to CSV first, do the analysis on the CSV, then export back to Excel only if the deliverable demands it. You'll see a step-change in accuracy and speed.

If you genuinely need native Excel handling — formula preservation, charts in place — use a tool with a real spreadsheet plugin (Codex's Excel integration is currently the best bet). Don't try to muscle Claude through it.

## Plan mode

`Shift+Tab` in any Claude Code session enters plan mode. Claude produces a step-by-step plan and runs nothing.

Use it before any non-trivial task. Reading and editing the plan is 10× faster than reading and editing the executed result. Most "Claude did the wrong thing" stories are "I never asked Claude to write a plan."

## Slides and presentations

Don't ask Claude to generate `.pptx` files directly — its PowerPoint XML knowledge is shaky and the output is a coin flip. Ask it to build the deck in React (or plain HTML/CSS) instead, then export to PowerPoint at the end if you need it.

Reasons:
- React component layout is in the model's strong suit; PowerPoint XML isn't.
- You can iterate on a single slide visually in the browser instead of regenerating the whole file.
- Branding, fonts, layouts are far easier to control with CSS than with PowerPoint themes.

For visual QA on the rendered result, hook up the Playwright MCP server. Claude can then literally see the slides and check them against requirements.

## Tool switching

Don't switch tools because of a single bad output — models leapfrog each other constantly and the cost of context-switching is real. But:

- If you're banging your head against the same wall in Claude for an hour, run the same prompt in another tool (Codex, Cursor, etc.) as a sanity check. Sometimes a different model just nails it.
- Native plugins matter. If you're stuck on Excel parsing, the tool with a native Excel plugin will outperform the one without, regardless of which underlying model is "smarter."
- Stick with one tool as your daily driver. The alt is for unblocking, not for habitual use.

## Skills hygiene

Most "Claude is ignoring my skill" problems are author problems. Three rules:

1. **Keep skill bodies short.** ~500–1000 characters of instruction. Long skills get pattern-matched and partially ignored.
2. **Sharpen the description.** The frontmatter description is what triggers the skill. If it doesn't name a specific situation, Claude won't invoke it.
3. **Read your skills by hand.** Don't ask Claude to grade your skills — it'll tell you they're great. Skim each one yourself, twice a quarter, and delete or rewrite anything that isn't pulling its weight.

A small library of sharp skills beats a big library of fluffy ones, every time.

## QA: stop trusting "done"

The single most common Claude failure is **claiming completion when work isn't actually done**. The same session that implemented the work cannot reliably verify it — it has already convinced itself.

The fix: spawn a fresh sub-agent to verify. Give it only the original requirements and the artifact, none of the implementation context. Make it produce a checklist with evidence (quoted lines, counts, screenshots) for each item.

This single habit catches more bugs than any prompt-tuning ever will. See `skills/qa-multi-agent.md` for the full pattern.

## CLAUDE.md and project memory

`CLAUDE.md` at your project root is loaded into every session. It's the most powerful per-project lever you have. Use it for:

- Things Claude consistently gets wrong in this project ("never use X, always use Y")
- Non-obvious constraints (deploy targets, API quirks, naming conventions)
- The shape of the project (where things live, what each folder is for)

Don't use it for:

- Documentation a human needs (that goes in README)
- Things that change every week (they'll go stale)
- Long lists of "do this" rules — Claude pattern-matches the first few and ignores the rest

Treat `CLAUDE.md` as a tight, curated rulebook. Edit it the way you'd edit a contract: every word matters.

## Desktop vs terminal

Claude Code's desktop app and terminal are functionally similar. If you're new, start in the desktop app — it's friendlier and the UI is cleaner. The MCP configuration mechanism differs slightly between the two, so when you wire up integrations, follow the docs for the surface you're actually on.

Plan mode and skill invocation work identically in both.

## When to ask for help

You're closer than you think when Claude is failing repeatedly — usually within one or two structural changes of the problem clicking. The pattern:

1. Stop iterating on the prompt.
2. Restate the goal in plain language.
3. Run plan mode and read the plan critically.
4. If the plan is right and execution still fails, the issue is almost always: too much context, the wrong file format, or a skill that's confusing the model. Address one of those, not the prompt.

The temptation is to keep retrying. Don't. Step back, change something structural, and the model usually finds it on the next pass.
