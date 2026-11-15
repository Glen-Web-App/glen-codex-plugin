---
name: onboard
description: 'Get started with glen: learn what it does and take the next steps for this account. Use when the user says "get me started with glen", "onboard me", "what can glen do", right after `glen login`, or when the glen server''s instructions offer it. For a broken install, use the setup skill.'
---

## Purpose

Explain glen to the user and walk them through what is left to set up in their
active glen org: turning recording on, backfilling past sessions, trying a
search, inviting teammates and sharing local skills. glen
decides which steps apply to this account and returns them as text.

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

The CLI command is `glen onboard`; the MCP tool is `glen_onboard`.

## Supported actions

1. Get the steps for this account:
   - CLI: `glen onboard`
   - MCP: `glen_onboard` with no arguments.
2. Relay the text it returns to the user, then follow it step by step. Ask
   before anything that writes, such as an import, an upload or an invite.
   When you have walked through the steps, end your message with the text's
   last line, word for word, so the user knows they can send feedback. Put any
   question you ask above it.
3. After a step, run `glen onboard` (or call `glen_onboard`) again to see what
   is left. Finished steps drop out.
4. When a step fails, follow the text's feedback line: offer to report it with
   the step and the error, and ask before sending.
   - CLI:

     ```
     glen bug "<title>" --description "<step and error>"
     ```

   - MCP: `glen_feedback`.

## Limits

- It never sends invites or feedback by itself: each one waits for the user.
- A session with only the glen MCP connection cannot import past sessions or
  upload local files; the text says what to do from a terminal agent instead.
