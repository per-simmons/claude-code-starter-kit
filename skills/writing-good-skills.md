---
name: writing-good-skills
description: Use this skill when creating or revising any other skill, or when Claude is ignoring instructions in an existing skill. It catches the most common failure: skills that are too long, too vague, or written in a way that confuses the model into thinking it followed them when it didn't.
---

# Writing Good Skills

Most "Claude is ignoring my skill" problems are actually skill-authoring problems. This document is the rulebook.

## The four failure modes

1. **The skill is too long.** ~500–1000 characters of *instruction* is the sweet spot. If your skill body is multiple pages, Claude will pattern-match the first chunk that seems relevant and skip the rest. It will then claim it followed the skill.
2. **The skill is too vague.** "Make sure the analysis is good" → ignored. "Before returning, verify every row in column B has a corresponding entry in the source CSV; report mismatches as a numbered list" → followed.
3. **The skill description doesn't trigger.** The `description` field in frontmatter is what Claude uses to *decide* to invoke the skill. If it doesn't mention the situations the skill applies to, Claude won't load it. Front-load the trigger phrases.
4. **The skill mixes instructions for multiple agents.** If your skill does QA, drafting, and review all in one file, split it. Each skill should have one job.

## What good frontmatter looks like

```yaml
---
name: descriptive-kebab-case
description: Use this skill when [specific trigger condition]. It [one-line summary of what it does and why].
---
```

The description should answer two questions in one sentence: *when does this fire?* and *what does it produce?*

Bad: `description: Helps with analysis`
Good: `description: Use this skill when the user asks for a financial summary across multiple spreadsheets. It produces a single consolidated CSV plus a written variance commentary.`

## Skill body structure

Most skills are best written as a short, ordered procedure:

```markdown
# Skill name

One-sentence purpose.

## Inputs
- What files / context this skill needs to start.

## Procedure
1. First step (specific, verifiable).
2. Second step.
3. ...

## Output
- Exactly what should be produced. Include filenames, formats.

## Done criteria
- A checklist Claude must verify before claiming completion.
```

The "Done criteria" section is the single most important part. Without it, Claude will declare success at step 3 and move on.

## How to test a skill

After writing or editing a skill, test it the same way you'd test code:

1. Open a fresh Claude Code session.
2. Trigger the skill with a realistic prompt.
3. Watch the terminal — Claude should announce it's using the skill. If it doesn't, the description didn't match. Rewrite it.
4. Inspect the output against the Done criteria *yourself*. Don't ask Claude to grade itself; it will say it passed.
5. If it failed, the fix is almost always: shorten the skill, sharpen one specific step, or move a piece of logic into a separate skill.

## Skill rot

Skills decay. Every time you change your workflow, your old skills become subtly wrong. Twice a quarter, open every skill and ask:

- Is this still how I do this?
- Is every word here pulling its weight?
- Could a fresh agent with no context understand exactly what to do?

Delete or rewrite anything that fails. A small library of sharp skills beats a big library of bloated ones.

## The official tooling

Anthropic publishes a `skill-creator` plugin in the official marketplace. Install it — it scaffolds new skills with the correct frontmatter and structure. Use it together with the rules above.

## Done criteria for this skill

Before declaring a skill "written":
- [ ] Frontmatter `description` names a specific trigger and a specific output
- [ ] Body is under ~1000 characters of instruction
- [ ] At least one step in the procedure is verifiable (produces a file, prints a count, etc.)
- [ ] A "Done criteria" checklist exists at the bottom
- [ ] Tested in a fresh session and the trigger fires
