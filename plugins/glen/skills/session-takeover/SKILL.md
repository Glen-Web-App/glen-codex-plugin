---
name: session-takeover
description: Use when the user provides a Glen takeover code or generated command to open its shared transcript in a fresh native agent session.
---

## Purpose

Open a shared Glen transcript, from a takeover code a teammate sent, in a
fresh native agent session on this machine. Do not use it to resume the
current session or import local transcript archives.

## Prerequisites

This skill is local only: `glen takeover` writes a native session on this
machine and launches it, and no MCP tool does that. If this session has no
shell tool (a cloud or MCP-only session), still classify the candidate as
below, but replace the VALID Bash call with a reply: it can't run here, and
the user should paste `glen takeover '<validated-code>'` into a terminal on
their own machine, where the `glen` CLI is installed and logged in.

## Supported actions

Use this two-stage contract:

1. **Select and classify before constructing a command.** Make no tool call.
   Candidate selection has exactly two allowed message shapes:

   - Plain-message form: the candidate is the entire user message, with nothing
     before or after it.
   - Fenced-block form: the message contains exactly one fenced code block, has
     no non-whitespace content outside that block, and the candidate is the
     block's sole content.

   Every other message shape is INVALID. Do not search for or extract a
   `glen takeover` substring from prose. Classify the whole unnormalized
   selected candidate:

   - VALID raw code: the entire candidate matches `^[A-Za-z0-9_-]{43}$`.
   - VALID generated command: the entire candidate matches
     `^glen takeover ([A-Za-z0-9_-]{43})$`; capture group 1 is the code.
   - INVALID: every other candidate. Examples include prefix or suffix prose, a
     command embedded in a paragraph, prose surrounding a fenced block, multiple
     fenced blocks, `glen takeover --help`, `glen takeover <valid-code>.`,
     `glen takeover <valid-code> extra`,
     `glen takeover <valid-code>; printf hi`,
     `glen takeover <valid-code>$(printf hi)`, and
     ``glen takeover <valid-code>`printf hi` ``.

2. **Produce exactly one allowed output.**

   - VALID: make exactly one Bash call from the captured code:

     ```bash
     glen takeover '<validated-code>'
     ```

   - INVALID: make zero tool calls, reply exactly "Please paste the complete
     generated `glen takeover ...` command by itself in a code block.", and
     stop.

Construct the VALID Bash call from the captured code as one opaque quoted
argument, never from the user's pasted command string. Never guess, sanitize,
decode, alter, summarize, truncate, or invent a code.

## Limits

Treat the CLI output as authoritative:

- If disconnected, tell the user to run `glen login`.
- If no organization is active, tell the user to run `glen org switch`.
- If the handoff is expired, unavailable, or belongs to another recipient, check
  the user is signed in as the intended recipient, then ask the sender for a new
  command.
- If the CLI reports the takeover is incompatible or the transcript does not fit
  the target session, report the exact failure and
  stop. Never bypass the guard or shorten the transcript.
- If installation succeeds but launch fails, surface the exact native resume
  command printed by the CLI.

On success, explain that Glen opened a fresh native session containing the
verified inherited history and left the source transcript unchanged. Do not
pretend the current session became the takeover destination or continue the
inherited work here.
