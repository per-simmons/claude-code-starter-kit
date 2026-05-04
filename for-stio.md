# For Mike & Danny at Stio

Hey guys —

Pulled together from our call. Everything in this repo is generic so you can share it with anyone on your team without the context of what we talked about. This page is the part that's actually for you.

## Read in this order

1. **[tips/claude-code-essentials.md](tips/claude-code-essentials.md)** — the one-pager. Covers the CSV-vs-Excel rule, plan mode (`Shift+Tab`), why React beats PowerPoint for slide gen, and the skill-hygiene basics. 5 minutes. Read it first.

2. **[skills/qa-multi-agent.md](skills/qa-multi-agent.md)** — this is the fix for "Claude said it did it but didn't." Spawn a fresh-context agent to verify each requirement with evidence. Use this on the OPEX analysis once you get past the parsing layer.

3. **[skills/writing-good-skills.md](skills/writing-good-skills.md)** — Mike, this one's for you specifically. The skills you've already built are probably 80% there but bloated; this is the rulebook for tightening them so Claude actually follows them. Pair it with the official `skill-creator` plugin from Anthropic's marketplace.

4. **[workflows/phased-workflow.md](workflows/phased-workflow.md)** — the research → plan → implement → validate → review → compound mental model. Apply it to the monthly financial report or the multi-year OPEX deck and you'll stop spinning in the implement phase.

5. **[skills/compound-learning.md](skills/compound-learning.md)** — once you start solving the gnarly stuff, capture the lessons here so you don't pay full price every month.

## What I'd do this week

- **Step 1:** On the OPEX analysis, ask Claude to convert all 125 Excel files to CSV first. Don't analyze the Excel directly. See if accuracy jumps. (My bet: it will, dramatically.)
- **Step 2:** Run the QA multi-agent pattern on the output. Fresh agent, evidence per row.
- **Step 3:** If you're still stuck after that, run the same task in Codex as an experiment — its native Excel handling is currently the best around. Not a permanent switch, just a sanity check.
- **Step 4:** Open every skill you've already written and apply the `writing-good-skills` rules. Most will get shorter; some will get split. Expect this to take an afternoon.

## Danny — for Claude Code onboarding

Stick with the desktop app for now, it's the cleaner UX. Plan mode (`Shift+Tab`) and skill invocation work the same as terminal. The MCP setup process is slightly different between desktop and terminal — when you wire up MCPs, just make sure you're following docs for the surface you're on.

## When you get stuck

If you've tried the patterns above and Claude is still spinning, send me a Loom or a transcript dump. The pattern of failure usually tells me exactly which lever to pull. Don't burn a week on it.

— Pat
