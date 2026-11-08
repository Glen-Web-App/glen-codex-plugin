---
name: import-transcripts
description: Import the user's old local coding-agent transcripts (Claude Code, Codex, Cursor, or Pi sessions from before glen was installed) into glen's shared team memory. Use when the user asks to import, backfill, or migrate past sessions/conversations/chat history into glen.
---

## Purpose

Backfill the user's past coding-agent sessions from before glen was installed
into the active org's shared memory.

## Prerequisites

This skill is local only: it reads transcript files on the user's machine.
It needs a shell on that machine with the `glen` CLI. With a shell, run
`glen status`; if it is not connected, tell the user to run `glen login`. If this session has no
shell tool (a cloud or MCP-only session), make no tool call. Tell the user the
import can't run here and to run `glen import <path>` in a terminal on the
machine that has the transcripts (after `glen login` if needed).

## Supported actions

- **Preview:** `glen import --dry-run <path…>` lists every conversation and
  turn count without writing anything. Offer it first for large directories so
  the user can confirm the scope.
- **Import:** `glen import <path…>` parses the transcripts, starts the import in
  the background, and returns right away. Tell the user it is running. Pass
  `--wait` only if the user wants it in the foreground.
- **Check progress:** `glen import --status` says running, finished, or
  stopped, with counts. glen does not tell you when it finishes.
- **Resume or retry:** re-run the same command. Turns already imported are
  skipped.

A path can be a file or a directory (scanned recursively, format detected per
file). Default locations:

- Claude Code: `~/.claude/projects`
- Codex: `~/.codex/sessions`
- Pi: `~/.pi/agent/sessions`
- Cursor: `~/Library/Application Support/Cursor/User/globalStorage/state.vscdb`
  on macOS, `~/.config/Cursor/User/globalStorage/state.vscdb` on Linux (needs
  Node 22.5+; close Cursor first for the freshest data)

## Limits

- The import lands in the active org: the `org:` line of `glen status`.
- "No active organization": tell the user to run `glen org switch`. "Not
  connected" or "connection expired": tell them to run `glen login`. Then
  re-run the same import.
- The `glen_import` MCP tool takes one turn per call and is not offered on
  the glen MCP connection in Codex and ChatGPT. Don't use it in place of `glen import`.
