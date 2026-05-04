# skill-creator

The official skill-authoring tool ships as a plugin in Anthropic's marketplace. Install it from there — it scaffolds new skills with the right frontmatter and structure. This file is the rulebook for what makes a skill actually work once you've installed it.

## Install

In Claude Code, open the plugins panel, find the official Anthropic marketplace, install `skill-creator`.

## What makes a skill work

Four things. Get any one wrong and Claude silently ignores the skill.

1. **Body is short.** ~500–1000 characters of instruction. Long skills get pattern-matched and partially skipped.
2. **Description triggers correctly.** The frontmatter `description` is what Claude uses to decide whether to load the skill. Name the trigger condition explicitly.
3. **Instructions are verifiable.** "Make it good" → ignored. "Verify every row in column B has a matching entry in source.csv" → followed.
4. **Skill has one job.** If yours mixes drafting + QA + review, split it.

## Frontmatter

```yaml
---
name: kebab-case-name
description: Use this skill when [specific trigger]. It [output].
---
```

Description should answer two questions in one sentence: when does this fire, what does it produce.

Bad: `Helps with analysis.`
Good: `Use this skill when the user asks for a multi-file CSV summary. Produces a single consolidated CSV plus a written variance commentary.`

## Body structure

```markdown
# Skill name

One-line purpose.

## Inputs
- What this skill needs to start.

## Procedure
1. Specific, verifiable step.
2. Specific, verifiable step.

## Output
- Exact filenames and formats.

## Done criteria
- Checklist Claude must verify before claiming completion.
```

The Done criteria section is the part that matters most. Without it, Claude declares success early.

## Testing a skill

1. Open a fresh session.
2. Trigger the skill with a realistic prompt.
3. Watch the terminal — Claude should announce loading the skill. If not, the description didn't match. Rewrite it.
4. Inspect output yourself against Done criteria. Don't ask Claude to grade itself — it will say it passed.

## Maintenance

Twice a quarter, open every skill. Ask: still accurate? still pulling its weight? Could a fresh agent execute it cold? Delete or rewrite anything that fails. Stale skills are worse than no skills.
