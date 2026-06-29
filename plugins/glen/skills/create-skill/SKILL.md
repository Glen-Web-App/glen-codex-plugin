---
name: create-skill
description: Turn what you just did (or a markdown prompt the user provides) into a reusable skill — either a local SKILL.md on this machine or a shared Glen skill saved to the org's library. Use when the user says "make this a skill", "save this as a skill", "save this as a Glen skill", or wants to capture a repeatable workflow.
---

You are authoring a reusable **skill** — a focused markdown prompt another agent will follow later, with no memory of this session.

1. **Pick the destination FIRST — local file or shared Glen skill.** A skill is either a **local `SKILL.md`** on this machine (a Codex skill, visible only to you) or a **Glen skill** saved to the org's shared library (recorded, reusable by your whole team).
   - **Check incognito before anything else:** run `glen status` via Bash. If it reports `incognito: ON`, a shared Glen skill is off the table — nothing is recorded off the record — so go straight to a **local skill** and tell the user that's why (e.g. "you're off the record, so I'll save this as a local skill").
   - **Otherwise, ask the user which they want** before authoring anything: a local `SKILL.md` (a Codex skill on this machine) or a shared Glen skill (saved to your org's library). Don't start writing until they answer.
2. **Gather the real workflow.** Read this conversation for the steps that actually worked. For anything done in an earlier session, run `glen search "<topic>"` via Bash to pull the relevant memory. Ask one or two clarifying questions only if the goal or trigger is genuinely ambiguous.
3. **Name + describe it.** A short imperative name and a one-sentence description that says *exactly when to trigger it* (this drives discovery).
4. **Write a tight SKILL.md body.** Best practices:
   - Lead with *when to use it* and the end goal.
   - Give concrete, ordered steps — exact commands, file paths, gotchas — not vague advice.
   - Keep it self-contained: assume zero prior context. Cut anything that isn't actionable.
   - **Reference other skills** when a step is itself a saved skill: write a markdown link `[Display text](glen:<slug>)` (find the slug with `glen skill search`). Glen tracks the link automatically and the reader will get a runnable `glen skill use <slug>` command exactly there — keep each referenced skill focused and say *when* to reach for it.
   - **If the skill is large, split it into a root + subskills** instead of shipping a monolith. When the body would be long or spans several distinct modes/topics, write a short *root* skill that says *when to reach for each part*, and move each part into its own focused skill the root references via `[Name](glen:<slug>)`. Create the subskills FIRST (so their slugs exist), then reference them from the root — the reader loads the small root and pulls only the subskill it needs on demand (progressive disclosure). For a Glen skill, each subskill is its own `glen skill create`; for a local skill, each is its own `SKILL.md`.
5. **Finish based on the destination chosen in step 1:**

   **Local skill** (and always the incognito path) — nothing leaves the machine:
   - Save it to `~/.codex/skills/<name>/SKILL.md` (Codex discovers personal skills there; `~/.agents/skills/<name>/SKILL.md` works as the cross-runtime alias).
   - Start the file with YAML frontmatter (`name`, and the `description` from step 3), then the body.
   - No `glen skill create`, no recording. Confirm the saved path to the user.

   **Glen shared skill** (only when not incognito):
   - **Declare its dependencies.** List the external tools/runtimes/MCPs the skill needs (node, an MCP, a CLI, …). For each: run `glen dependency search "<tool>"` via Bash — if a confident match exists, note its `[id]`; otherwise web-search the canonical source/install page and note `{ name, description, sourceUrl, kind }` (kind = mcp|cli|runtime|library|service). Write them to a JSON array file, e.g. `/tmp/glen-deps.json`, mixing `{ "dependencyId": "<id>" }` (reuse) and new objects.
   - **Save it** via Bash — write the body to a file (e.g. `/tmp/glen-skill.md`), then run (add `--deps /tmp/glen-deps.json` if there are any):

     ```
     glen skill create --name "<name>" --description "<one-line>" --file /tmp/glen-skill.md --deps /tmp/glen-deps.json
     ```

     On a name collision it errors — pick a different name and retry, or pass `--update` to overwrite a skill you previously authored.
   - The skill is shared with your whole organization. Confirm the saved slug back to the user.
