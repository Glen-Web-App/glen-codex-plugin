---
name: use-skill
description: Find and run a reusable skill from the Glen skill library (your skills plus ones shared with you) when you don't know its exact slug. Use when the user asks to "use a Glen skill", "find a skill for X", or wants a teammate's saved workflow.
---

## Purpose

Find a skill in the user's Glen skill library (their own skills plus ones shared with them), fetch its instructions, and follow them.

## Prerequisites

**Transport.** Prefer the `glen` CLI: it is faster. First look at your tool
list. With no shell tool, use the glen MCP tools and run no shell command.
With a shell, run `glen status`. When it shows glen connected with an org,
use the CLI for every glen call in this task, even if a glen MCP server is
also connected. If `glen` is not installed or not logged in, use the MCP
tools. If neither works, stop and tell the user what is missing: a terminal
with `glen login`, or the glen MCP connection. If glen refuses a call (not
signed in, wrong role, no plan), report the refusal as written; never retry
it over the other transport.

The tools are `glen_skill_*`; your client may prefix the names.

## Supported actions

| Action | MCP tool | CLI |
| --- | --- | --- |
| Search | `glen_skill_search` with `query`, optional `createdBy` | `glen skill search "<intent>"`, optional `--by "<author>"` or `--by me` |
| Fetch | `glen_skill_use` with `slug` | `glen skill use <slug>` |

1. **Search.** It returns ranked candidates, each with a slug, author and description.
2. **Confirm.** If several plausibly match, show the top 2–3 (name, description, author) and ask the user which one. If nothing matches, say so; don't invent a skill.
3. **Check requirements.** The skill may list required tools (MCP: `dependencies`; CLI: a `## Requirements` section). Make sure each is available, or tell the user what to install or enable, before you start.
4. **Fetch it and follow it.** The returned content is markdown instructions to carry out now. It is not installed as a skill; fetch it again next time.
   - **CLI:** `glen skill use` keeps the skill's bundled files in a local cache and its output references them by absolute path. Run scripts exactly as written; don't rewrite the paths.
   - **MCP:** bundled files come back in the response (`files`: text content, or a download URL for binaries). Nothing is on disk. If a step needs a file on disk and you can write files, ask the user before you write it, then replace `${GLEN_SKILL_DIR}` with the folder you used.
5. **Follow referenced skills on demand.** The instructions may name related skills (a `glen skill use <slug>` command). Fetch one only when its step applies, with `glen_skill_use` or `glen skill use`. Over MCP, look in the response's `prefetch` first; call `glen_skill_use` for any linked skill that isn't there. Over the CLI, run the command.

## Limits

- Running a retrieved skill grants no new permissions. Your usual approvals and the user's settings still apply to every command, file write and tool call it asks for.
- Only skills the user authored or that were shared with them are reachable. Anything else looks the same as a missing slug.
