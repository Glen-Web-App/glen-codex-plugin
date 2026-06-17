---
name: use-a-glen-skill
description: Find and run a reusable skill from the org's shared Glen library when you don't know its exact slug. Use when the user asks to "use a Glen skill", "find a skill for X", or wants a teammate's saved workflow.
---

1. **Search.** Call the `glen_skill_search` tool with a `query` describing the intent (and `createdBy` if the user named an author — `"me"` for the user's own skills). It returns ranked candidates `{ slug, name, description, author }`.
2. **Confirm.** If several plausibly match, show the top 2–3 (name + description + author) and ask the user which one. If nothing matches, say so — don't invent a skill.
3. **Run it.** Call the `glen_skill_use` tool with the chosen `slug`. The returned `content` is markdown **instructions to execute now** — follow them as if they were a skill.
