---
name: create-skill
description: Turn what you just did (or a markdown prompt the user provides) into a reusable skill, either a local SKILL.md on this machine or a shared Glen skill saved to the org's library. Use when the user says "make this a skill", "save this as a skill", "save this as a Glen skill", or wants to capture a repeatable workflow. Also use when the user asks to share or delete a Glen skill they authored.
---

## Purpose

Author a reusable **skill**: a focused markdown prompt another agent will follow later, with no memory of this session. It is either a **local `SKILL.md`** on this machine (a Codex skill, visible only to the user) or a **Glen skill** in the user's Glen library (private to them by default, sharable with teammates or the whole org).

## Prerequisites

A local skill needs write access to this machine's files and nothing from glen.

A Glen skill needs a glen connection.

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
| Find related memory | `glen_search` | `glen search "<topic>"` |
| Find a dependency | `glen_dependency_search` | `glen dependency search "<tool>"` |
| Find a skill to reference | `glen_skill_search` | `glen skill search "<intent>"` |
| Save | `glen_skill_create` | `glen skill create` |
| Share | `glen_skill_share` with `slug`, `emails` and/or `org: true` | `glen skill share <slug> --with <email>[,<email>…]` and/or `--org` |
| Delete | `glen_skill_delete` with `slug` | `glen skill delete <slug>` |

1. **Pick the destination first.** A request for a "glen skill", a "team skill", or a skill in their library or org is a Glen skill: save it to glen with step 5, and never write it as a local skill instead. A request for a local or personal skill file is a local skill. If the user hasn't said, ask which they want, and don't start writing until they answer. If you can't write local files (for example, a session with only the MCP tools), a Glen skill is the only destination; you can also give the user the finished `SKILL.md` text. Save a Glen skill only when the user clearly asked for it.
2. **Gather the real workflow.** Read this conversation for the steps that actually worked. For anything done in an earlier session, search glen memory. Ask one or two clarifying questions only if the goal or trigger is genuinely ambiguous.
3. **Name and describe it.** A short imperative name and a one-sentence description that says _exactly when to trigger it_ (this drives discovery).
4. **Write a tight SKILL.md body.**
   - Lead with _when to use it_ and the end goal.
   - Give concrete, ordered steps (exact commands, file paths, gotchas), not vague advice.
   - Keep it self-contained: assume zero prior context. Cut anything that isn't actionable.
   - **Reference other skills** when a step is itself a saved skill: write a markdown link `[Display text](glen:<slug>)` (find the slug with skill search). The reader gets a runnable `glen skill use <slug>` command exactly there. Say _when_ to reach for it. Reserve this for genuinely **shared** skills that also get used on their own.
   - **Bundle supporting material in one skill directory, not separate subskills.** Put long reference material in `references/*.md` and executable helpers in `scripts/` beside `SKILL.md`, and point to them from the body as `${GLEN_SKILL_DIR}/references/<name>.md` or `${GLEN_SKILL_DIR}/scripts/<name>`. When a reader fetches the skill through the CLI, `glen skill use` caches the directory locally and replaces `${GLEN_SKILL_DIR}` with its absolute path.
5. **Finish based on the destination.**

   **Local skill.** Nothing leaves the machine.
   - Save it to `~/.codex/skills/<name>/SKILL.md` (Codex discovers personal skills there; `~/.agents/skills/<name>/SKILL.md` works as the cross-runtime alias).
   - Start the file with YAML frontmatter (`name`, and the `description` from step 3), then the body.
   - Confirm the saved path to the user.

   **Glen skill.**
   - **Declare its dependencies.** List the external tools, runtimes or MCP servers the skill needs. Search each one with dependency search. If a confident match exists, reuse it as `{ "dependencyId": "<id>" }`; otherwise find the canonical install page and add `{ "name", "description", "sourceUrl", "kind" }` (kind is `mcp`, `cli`, `runtime`, `library` or `service`).
   - **Save it over MCP:** call `glen_skill_create` with `name`, `description`, `content` (the body), and `dependencies` if any. For bundled files, add `files`: each has a `path` under `scripts/`, `references/` or `assets/`, `contentType`, `content` (text) or `contentBase64` (binary), and `isExecutable: true` for scripts.
   - **Save it over the CLI:** write the dependency list to a JSON file (for example `/tmp/glen-deps.json`). For a text-only skill, write the body to a file and run:

     ```
     glen skill create --name "<name>" --description "<one-line>" --file /tmp/glen-skill.md --deps /tmp/glen-deps.json
     ```

     For a skill from an existing folder, or with bundled files, pass the folder with `--dir` (it needs `SKILL.md` at its root; the CLI reads the body from there). To build one, make a directory with `SKILL.md` at its root plus whichever of `scripts/`, `references/`, `assets/` it needs, then run:

     ```
     glen skill create --name "<name>" --description "<one-line>" --dir /tmp/glen-skill-pkg --deps /tmp/glen-deps.json
     ```

     `--file` and `--dir` are mutually exclusive. Drop `--deps` when there are none.
   - **Confirm and offer sharing.** The skill is private to the user by default. Ask whether to share it with the whole org (`org: true` / `--org` on save, or share later) or with specific teammates (share with their emails). Confirm the saved slug and its visibility.
6. **Share or delete (author only, when asked).** Sharing is additive. Deleting is permanent and removes the skill for everyone it was shared with: confirm the slug with the user first.

## Limits

- A name that is taken in the org is a conflict: pick another name, or pass update (`update: true` / `--update`) for a skill the user authored. Only the author can update, share or delete a skill.
- Bundled files: only `scripts/`, `references/` and `assets/`, at most 100 files. Keep them small; glen rejects an oversized bundle and names the file.
