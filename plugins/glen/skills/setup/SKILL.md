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
- `status: login=missing ...` → run `glen login` — a browser window opens with
  a short device code prefilled; the user approves it to connect their account.
  If no browser can open (SSH/headless), relay the printed URL and code so the
  user can approve from any device.
- `status: login=ok org=missing` → run `glen org list`, show the user the
  options, and run `glen org switch <slug>` for their choice. Never pick an
  org for them.
- `status: claude=detected plugin=not-installed` (or codex/pi) → run
  `glen install --agent <claude|codex|pi>` for the agent you are running in.
  For pi, tell the user to run `/reload` inside pi (or restart pi) afterwards
  so the glen extension loads.
- `status: codex=detected plugin=... hooks=not-registered` → run
  `glen install --agent codex`.
- `status: codex-self-heal=failed: ...` → run `glen install --agent codex`,
  then re-check.
- `status: claude=not-found` (or `status: codex=not-found` /
  `status: pi=not-found`) when the user says it IS installed → ask them to run
  `which claude` (or `which codex` / `which pi`) in their terminal, then run
  `glen install --claude-path <that path>` (or `--codex-path` / `--pi-path`).

`glen doctor` also prints one `status: ingest agent=<agent>
state=<ok|failing|never|fresh-install|idle> lastSuccess=<iso|never>` line per
detected agent — this tracks whether hooks are actually firing, separate from
whether the plugin is installed:
- `ok` / `failing` — reflects the last recorded hook run. For `failing`,
  doctor prints a remedy right after the line (check `glen status`, then
  `glen login` if disconnected).
- `fresh-install` — installed under an hour ago with no ingest yet; not a
  fault, just still warming up.
- `idle` — hooks fire here; nothing recorded yet — logged out, glen off, or
  no completed turn.
- `never` — hooks have never fired on THIS machine. Doctor prints a remedy
  right after the line: untrusted hooks (trust them in-agent and start a new
  session), a PATH gap, a failed hook-trust query, or a machine-topology
  mismatch.
- A topology mismatch usually means the agent process runs on a different
  machine than the one you're looking at: Codex `--remote` / Desktop-SSH and
  Claude Code over SSH/devcontainer both run the agent — and its hooks — on
  the REMOTE host, not locally. Install glen there and run `glen doctor` on
  that machine, as the user the agent connects as. Cloud sessions (Codex
  Cloud tasks, claude.ai/code) can't run user-level hooks at all — only
  repo-committed hooks apply there.

Finish with `glen status` and tell the user, in one sentence, what state they
are in now.
