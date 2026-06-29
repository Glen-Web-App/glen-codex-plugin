---
name: use-skill
description: Find and run a reusable skill from the org's shared Glen library when you don't know its exact slug. Use when the user asks to "use a Glen skill", "find a skill for X", or wants a teammate's saved workflow.
---

1. **Search.** Run `glen skill search "<intent>"` via Bash (add `--by "<author>"`, or `--by me` for your own skills). It prints ranked candidates, each with its **[slug]**, author, and description.
2. **Confirm.** If several plausibly match, show the top 2–3 (name + description + author) and ask the user which one. If it prints "no matching skills", say so — don't invent a skill.
3. **Check requirements.** If the output has a `## Requirements` section, make sure each listed tool is available — follow its source link to install/enable it (or tell the user) — before you start.
4. **Run it.** Run `glen skill use <slug>` via Bash. The output is markdown **instructions to execute now** — follow them as if they were a skill.
5. **Follow referenced skills on demand.** The instructions may contain runnable `glen skill use <slug>` commands for related skills. Run one via Bash only when its step applies — they're pulled on demand, not all upfront.
