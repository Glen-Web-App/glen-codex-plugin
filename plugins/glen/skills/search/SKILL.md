---
name: search
description: Search the team's shared glen memory for a SPECIFIC fact, decision, or past discussion that is not already in the auto-injected context. Do not use for things already recalled this turn.
---

## Purpose

Recall what the team has already worked out: decisions, rationale, owners, past
incidents, how a named system came to be. Glen holds durable history, not live
state. For current or source-owned facts, use the source-of-truth tool first (for
example Slack, GitHub, Notion, or logs), especially when the user names it. If that
source is unavailable, say so. If it conflicts with glen, prefer it for current state.

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

The CLI command is `glen search`; the MCP tool is `glen_search`.

For "No active organization", tell the user to run `glen org switch` (CLI) or
reconnect the MCP server and pick an org.

## Supported actions

Search with ONE comprehensive query, not a fan-out of variants. Each search makes
the user wait, so do not fan out. Write the query as a natural-language question,
not keywords. Pack everything you know into one self-contained question: who, what, the
repo or system involved, and the kind of answer you need (a decision, an owner, a
rationale, setup steps).

- ✅ `"Shams's recent work on the usage-page redesign in the dashboard repo: what was decided, current status, and who owns it"`
- ❌ `"Shams"`, then `"usage page redesign"`, then `"dashboard"`

With the MCP tool, pass that question as `query`. With the CLI, run
`glen search "<query>"`.

If a hook or reminder suggests several questions, merge the related ones into one
query. A follow-up search is fine when the first result surfaces something new
that would change what you do next. Never re-run a rephrase of a query that
returned little.

Read the result as recalled memory that may be outdated or conflicting:

- The CLI prints a synthesized answer and a "Sources:" list (conversation, date,
  author, `observation:<id>` or `message:<id>`), then a "Couldn't verify:" list
  of gaps.
- The MCP tool returns `answer` (the same text, including its "Sources:" list),
  `citations` (each with a `forgetReference`), `gaps` and `confidence`.

Cite what the evidence says and where it came from: name the conversation, author
and date behind each claim you use. Mention `gaps` or low confidence when they
matter to the answer.

## Limits

- No results, or an empty answer with no citations, means glen has no evidence.
  Say that plainly. Do not guess and do not re-search variants. An empty answer
  can also mean glen is off or silent for this user.
- Glen only knows what was recorded. Work done without glen, or in another org,
  is not in it.
- Evidence that current code or state contradicts is a candidate for the forget
  skill, after you verify the contradiction.
