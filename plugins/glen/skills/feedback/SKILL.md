---
name: feedback
description: Send a bug report or product feedback about glen itself to the Glen team. Use when the user says things like "tell the glen team", "file a bug about glen", "glen search is useless, report it", "give feedback on glen", or when YOU hit a concrete, reproducible glen malfunction worth reporting.
---

## Purpose

File a ticket with the Glen team about **glen itself**: the CLI, the memory,
the search, the dashboard. It is NOT for bugs in the user's own project; those
belong in their issue tracker.

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

## Supported actions

A ticket carries the user's identity and, when glen recorded this
conversation, a link to it. Never paste secrets, tokens, or file contents into
a ticket.

**Bug.** Title, what is wrong, optional steps to reproduce, severity (low,
medium, high, critical; the CLI defaults to medium, the MCP tool requires it).

- MCP: `glen_feedback` with `ticket: {kind: "bug", title, description,
  stepsToReproduce, severity}`.
- CLI:

  ```
  glen bug "<title>" --description "<what is wrong>" [--steps "<repro>"] [--severity <level>] [--agent <agent>]
  ```

**Feedback.** Title, the concrete change wanted, and why it matters.

- MCP: `glen_feedback` with `ticket: {kind: "feedback", title,
  requestedChange, motivation, context}`.
- CLI:

  ```
  glen feedback "<title>" --wants "<change>" --why "<why>" [--context "<extra>"] [--agent <agent>]
  ```

  `--agent` takes your agent type as listed in `glen bug --help`.

Write in the user's own words where you have them. Infer the why when it is
obvious rather than interrogating the user.

**Who it is from.** When the user asked, file on their behalf (MCP
`onBehalfOf: "user"`; CLI default). When the observation is yours, file as the
agent (MCP `onBehalfOf: "agent"`; CLI `--from agent`). Over MCP, also pass
`agentName` and your session id as `conversationId`.

**Filing without being asked** (MCP `proactive: true`; CLI `--proactive`).
Only for something concrete and attributable:

- a glen command or tool failed or returned a malformed result
- glen search returned results that clearly contradict the code you just read
- a glen hook misfired, or injected context that was plainly wrong

Not for: a search that found nothing, a preference about output format, a
hunch, or anything you cannot state as a specific observed behaviour. When in
doubt, mention it to the user and let them decide.

## Limits

- While the user is off the record (incognito), self-initiated tickets are not
  sent: the CLI says so, and the MCP tool returns `skipped: "incognito"`. Tell
  the user and let them decide whether to file it.
- On success, tell the user it went in; if the MCP result has `posted: false`,
  say it was recorded but not delivered. On failure, relay the error and its
  fix (for example "no active organization" means `glen org switch`) and stop.
- Never retry a failed filing in a loop, and never re-file the same ticket
  with different wording.
