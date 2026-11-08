---
name: forget
description: Correct glen's memory by forgetting evidence that is wrong, outdated, or no longer relevant, so the team stops being told it. Use when the user says things like "that's wrong, forget it", "glen has the wrong X", "that memory is out of date", or "stop remembering that", or when YOU verify that recalled evidence is contradicted by current code or state.
---

## Purpose

Stop glen from recalling evidence that is wrong, outdated, or no longer relevant,
so the team stops being told it. A stale memory gets caught two ways: the user
points at it, or you verify it yourself.

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

The CLI commands are `glen forget` and `glen restore`; the MCP tools are
`glen_forget` and `glen_restore`. Forget over the transport that ran the search:
evidence references only exist in the org that returned them.

For "No active organization", tell the user to run `glen org switch` (CLI) or
reconnect the MCP server and pick an org.

## Supported actions

**User-initiated.** The user says a remembered fact is wrong or outdated:

1. Find the source references. Search results end each source with an exact
   `observation:<id>` or `message:<id>` reference (`forgetReference` in MCP
   citations). If the result is not already in context, use the search skill.
2. Confirm which evidence is wrong. Do not forget every source in a synthesized
   answer. When several sources repeat the same stale claim, select all of them.
3. Forget them in one call, with the references unchanged:
   `glen_forget` with `references: ["observation:<id>", "message:<id>"]`, or
   `glen forget observation:<id> message:<id>`.

**Agent-detected.** Recalled evidence is contradicted by current code or state:

1. Verify the contradiction directly. Never forget on suspicion, a partial view,
   or because two memories disagree. Surface uncertain conflicts to the user.
2. Forget only the contradicted references.

Then tell the user exactly what you forgot and why: quote what each piece of
evidence said and where it came from (conversation, author, date), and what
current source contradicts it. State the correct fact in the conversation so glen
can record the correction.

Undo with `glen_restore` or `glen restore <reference...>`.

## Limits

- Only forget evidence a search returned. Without a reference, say there is no
  evidence to forget rather than guessing an id.
- Forgetting an observation hides that distilled memory from recall.
- Forgetting a message removes it from recall and hides derived observations that
  have no other active supporting message. The raw message stays in transcript
  history for audit.
- Forget is idempotent. Already-hidden references come back as unchanged.
- Restoring a message brings back only the observations hidden with it, not
  ones forgotten directly; restore those by their own reference.
- Forget and restore are refused while the user is off the record (incognito).
  Report the refusal; do not turn incognito off yourself.
- Up to 50 references per call.
