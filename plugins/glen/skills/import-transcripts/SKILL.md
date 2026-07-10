---
name: import-transcripts
description: Import the user's old local coding-agent transcripts (Claude Code, Codex, Cursor, or Pi sessions from before glen was installed) into glen's shared team memory. Use when the user asks to import, backfill, or migrate past sessions/conversations/chat history into glen.
---

Run `glen import <path…>` via Bash. It parses local transcripts, replays them
into glen turn by turn, and is safe to re-run — already-imported turns are
skipped automatically.

Where old transcripts live:

- Claude Code sessions: `~/.claude/projects` (one `<session-uuid>.jsonl` per
  session, grouped by project directory)
- Codex sessions: `~/.codex/sessions`
- Pi sessions: `~/.pi/agent/sessions` (one `<timestamp>_<uuid>.jsonl` per session,
  grouped by project directory)
- Cursor chats: `~/.config/Cursor/User/globalStorage/state.vscdb` on Linux,
  `~/Library/Application Support/Cursor/User/globalStorage/state.vscdb` on
  macOS (needs Node 22.5+; close Cursor first for the freshest data)

A path can be a single file or a directory (scanned recursively; formats are
auto-detected per file). Preview with `glen import --dry-run <path…>` — it
lists every conversation and turn count without writing anything. For large
directories, offer the dry run first so the user can confirm the scope.

If it prints "no active organization", tell the user to run `glen org switch`.
If it prints "not connected", tell the user to run `glen login`.
