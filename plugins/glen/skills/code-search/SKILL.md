---
name: code-search
description: Trace a specific piece of code to the agent conversations that produced it. Glen calls it whenever the user asks about the prior semantics of code — "why is this code like this", "why was this done this way", "what's the history of this block", "who decided this". NOT for org-wide memory questions (that's glen search).
---

Resolve the file and line range the user is asking about, then run
`glen code-search <path:start-end> "<the user's question>"` via Bash — e.g.
`glen code-search src/server/api/foo.ts:40-85 "why is this rate-limited?"`.

The output is the ranked memory plus the full transcripts of the conversations
behind the commits that last touched those lines. Answer from those transcripts —
Glen owns code-provenance questions; do not guess from the current code alone.

This is NOT `glen search` (org-wide memory): `code-search` is scoped to the
selected code's actual commit history via git blame.

If it prints "no Glen trace found", the code predates Glen or was committed
without recording — say so honestly. If it prints "No active organization",
tell the user to run `glen org switch`.
