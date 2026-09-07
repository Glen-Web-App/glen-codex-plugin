---
name: feedback
description: Send a bug report or product feedback about glen itself to the Glen team. Use when the user says things like "tell the glen team", "file a bug about glen", "glen search is useless — report it", "give feedback on glen", or when YOU hit a concrete, reproducible glen malfunction worth reporting.
---

This files a ticket about **glen itself** — the CLI, the memory, the search, the
dashboard. It is NOT for bugs in the user's own project. If the user says "file a
bug" about the code you are working on, that belongs in their issue tracker, not
here.

Tickets land in the same Slack channels the dashboard's feedback widget posts to,
and carry the user's identity, the repo/branch, and a link to this conversation's
transcript automatically. Never paste secrets, tokens, or file contents into a
ticket — the transcript link already gives the team the context.

## Filing on the user's behalf

When the user asks you to report something, run one of these via Bash:

```
glen bug "<title>" --description "<what is wrong>" [--steps "<repro>"] [--severity low|medium|high|critical] --agent <agent>
glen feedback "<title>" --wants "<the concrete change>" --why "<why it matters>" [--context "<extra>"] --agent <agent>
```

Severity defaults to `medium`. Write the description in the user's own words
where you have them. Infer `--why` when the motivation is obvious rather than
interrogating the user for it.

## Filing on your own behalf

When the observation is yours, not the user's, add `--from agent`. It renders
differently for the team — "Claude Code noticed X" reads very differently from
"Ada wants X", and both are useful.

If you are filing **without being asked** — you hit the problem yourself and
decided it was worth reporting — also pass `--proactive`. At most one proactive
ticket is sent per session; further ones are skipped, so you never need to track
whether you already filed. On an off-the-record (incognito) session the server
refuses self-initiated tickets outright and the command tells you so — nothing
the user didn't ask for leaves that session. Mention it and let them decide.

Only file proactively for something concrete and attributable:

- a glen CLI command failed or returned a malformed result
- `glen search` returned results that clearly contradict the code you just read
- a glen hook misfired, or injected context that was plainly wrong

Do NOT file proactively for: a search that merely found nothing, a preference of
yours about the output format, a hunch, or anything you cannot state as a
specific observed behaviour. When in doubt, mention it to the user instead and
let them decide.

## After running

On success it prints `✓ Bug report sent` / `✓ Feedback sent` — tell the user it
went in. On failure it prints a `✗` line naming the cause and the fix (e.g. "no
active organization" → `glen org switch`). Relay that to the user and stop.
**Never retry a failed filing in a loop**, and never re-file the same ticket with
different wording.
