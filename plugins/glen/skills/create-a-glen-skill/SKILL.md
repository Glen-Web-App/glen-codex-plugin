---
name: create-a-glen-skill
description: Turn what you just did (or a markdown prompt the user provides) into a high-quality reusable Glen skill, then save it to the org's shared library so teammates can reuse it. Use when the user says "make this a skill", "save this as a Glen skill", or wants to capture a repeatable workflow.
---

You are authoring a reusable **skill** — a focused markdown prompt another agent will follow later, with no memory of this session.

1. **Gather the real workflow.** Read this conversation for the steps that actually worked. For anything done in an earlier session, run `glen search "<topic>"` via Bash to pull the relevant memory. Ask the user one or two clarifying questions only if the goal or trigger is genuinely ambiguous.
2. **Write a tight SKILL.md body.** Best practices:
   - Lead with *when to use it* and the end goal.
   - Give concrete, ordered steps — exact commands, file paths, gotchas — not vague advice.
   - Keep it self-contained: assume zero prior context.
   - Cut anything that isn't actionable.
3. **Name + describe it.** A short imperative `name` and a one-sentence `description` that says *exactly when to trigger it* (this drives search).
4. **Save it** by calling the `glen_skill_create` tool with `{ name, description, content }` (the full markdown body). On a name collision it returns an error — pick a different name and retry. To revise a skill you previously authored, pass `update: true`.

The skill is shared with your whole organization. Confirm the saved slug back to the user.
