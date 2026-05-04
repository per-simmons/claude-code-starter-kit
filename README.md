# Claude Code Starter Kit

A small, opinionated set of patterns for getting more out of Claude Code — written for people who aren't full-time engineers but want to stop spinning wheels.

The contents here are adapted from real production setups, stripped of any project-specific wiring so you can drop them into your own repo.

## What's in here

### Skills (drop into `.claude/skills/<name>/SKILL.md`)
- **[writing-good-skills](skills/writing-good-skills.md)** — the meta-skill. How to write skills Claude actually follows, instead of skills that get silently ignored.
- **[qa-multi-agent](skills/qa-multi-agent.md)** — the pattern that fixes Claude's #1 failure mode: claiming work is done when it isn't. Uses fresh-context agents so each verifier hasn't inherited the implementer's blind spots.
- **[compound-learning](skills/compound-learning.md)** — capture solved problems once so you stop re-solving them every session.

### Workflows
- **[phased-workflow](workflows/phased-workflow.md)** — the research → plan → implement → validate → review → compound loop. A mental model, not a slash command. Works for code, content, analysis, anything where Claude needs structure.

### Tips
- **[claude-code-essentials](tips/claude-code-essentials.md)** — the one-pager. Plan mode, model choice, when to switch tools, how to keep your skills from rotting into AI slop, and the Excel-vs-CSV rule.

## How to use this

1. Read **claude-code-essentials** first — 5 minutes, fixes most people's biggest leaks.
2. Install the official `skill-creator` plugin from the Anthropic marketplace (this kit's `writing-good-skills` is the philosophy; `skill-creator` is the tool).
3. Pick one workflow you keep getting frustrated with. Apply the **phased-workflow** to it. Add a **qa-multi-agent** check at the end.
4. Whenever you solve a tricky problem, write a **compound-learning** entry. Future-you will thank you.

## What this kit deliberately does *not* include

No slash commands, no project orchestrators, no domain-specific pipelines. Those are bound to specific repos and tooling. The patterns here are portable — adapt them to whatever you're working on.
