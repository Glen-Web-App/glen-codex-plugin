---
name: setup
description: Set up, fix, or update glen on this machine. Use when the user asks to "set up glen", "fix glen", "update glen", says glen is not working / not connected / not recording, or the status line shows glen is not connected or has no org selected.
---

Run `glen doctor` first and branch on its `status:` lines. Fix ONLY what is
broken, in this order, then re-run `glen doctor` to verify and confirm to the
user what changed.

- `glen: command not found` → install the CLI: `npm install -g @tryglen/cli`,
  then run `glen install` (it sets up the plugin + hooks for this agent).
- `status: cli=<x> latest=<y> channel=<...>` where y is newer than x → run
  `glen update` (updates the CLI and any installed glen plugins in one go).
- `status: login=missing ...` → run `glen login` — tell the user a browser
  window will open to connect their account.
- `status: login=ok org=missing` → run `glen org list`, show the user the
  options, and run `glen org switch <slug>` for their choice. Never pick an
  org for them.
- `status: claude=detected plugin=not-installed` (or codex) → run
  `glen install --agent <claude|codex>` for the agent you are running in.
- `status: codex=detected plugin=... hooks=not-registered` → run
  `glen install --agent codex`.
- `status: codex-self-heal=failed: ...` → run `glen install --agent codex`,
  then re-check.
- `status: claude=not-found` (or `status: codex=not-found`) when the user says
  it IS installed → ask them to run `which claude` (or `which codex`) in their
  terminal, then run `glen install --claude-path <that path>` (or
  `--codex-path`).

Finish with `glen status` and tell the user, in one sentence, what state they
are in now.
