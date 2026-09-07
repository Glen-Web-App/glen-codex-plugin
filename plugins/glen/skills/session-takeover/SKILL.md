---
name: session-takeover
description: Use when the user provides a Glen takeover code or generated command to open its shared transcript in a fresh native agent session.
---

# Session Takeover

Use this skill only to open a shared Glen transcript in a fresh native session.
Do not use it to resume the current session or import local transcript archives.

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
     `glen takeover <valid-code>; printf TAKEOVER_INJECTION_PROBE`,
     `glen takeover <valid-code>$(printf TAKEOVER_INJECTION_PROBE)`, and
     ``glen takeover <valid-code>`printf TAKEOVER_INJECTION_PROBE` ``.

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

Treat the CLI output as authoritative:

- If disconnected, tell the user to run `glen login`.
- If no organization is active, tell the user to run `glen org switch`.
- If the handoff is expired, unavailable, or belongs to another recipient, ask
  the sender for a new command.
- If compatibility or context-fit validation fails, report the exact failure and
  stop. Never bypass the guard or shorten the transcript.
- If installation succeeds but launch fails, surface the exact native resume
  command printed by the CLI.

On success, explain that Glen opened a fresh native session containing the
verified inherited history and left the source transcript unchanged. Do not
pretend the current session became the takeover destination or continue the
inherited work here.
