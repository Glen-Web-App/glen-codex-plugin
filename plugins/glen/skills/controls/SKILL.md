---
name: controls
description: Control glen memory - turn glen on/off, go off the record (incognito, account-wide), go silent (keep recording, surface nothing), toggle glen's skill suggestions, or switch which organization's memory is active. Use ONLY when the user explicitly asks (e.g. "turn glen off", "go off the record", "stop recording", "stop suggesting skills", "just record, don't inject anything", "switch to my personal org").
---

## Purpose

Change how glen records and recalls for this user, only when they explicitly
ask. Every mode and suggestion setting is account-wide and server-enforced: it
applies on every machine, agent, and connected client, and the same switches
live in the dashboard under Settings > Memory.

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

| Intent | MCP tool | CLI |
| --- | --- | --- |
| Show current settings | `glen_status` (mode and suggestion settings) | `glen status` (org and mode; it does not show suggestion settings) |
| On: recall and record (the default) | `glen_set_recording` `{mode: "on"}` | `glen on` |
| Off: recall nothing, record nothing | `glen_set_recording` `{mode: "off"}` | `glen off` |
| Incognito: keep recalling, record nothing ("go off the record", "stop recording") | `glen_set_recording` `{mode: "incognito"}` | `glen incognito` |
| Silent: keep recording, surface nothing ("just record, stay out of my way") | `glen_set_recording` `{mode: "silent"}` | `glen silent` |
| Skill suggestions on or off | `glen_set_suggestions` `{skillCreation}` and/or `{skillUse}` | `glen suggestions skill-create on\|off`, `glen suggestions skill-use on\|off` |
| See or switch the active org | `glen_list_orgs` (read only) | `glen org list`, `glen org switch <slug>` |

**Before turning capture on** (on, or silent, from off or incognito), tell the
user in a few lines:

- **Recording vs silent vs incognito.** On records and recalls. Silent records
  everything but surfaces nothing: no recall, no suggestions, and search
  returns nothing. Incognito recalls but adds nothing to team memory.
- **Which org receives it.** The active org: the `org:` line of `glen status`,
  or the `active: true` entry of `glen_list_orgs`. Everyone in that org can
  search what is recorded.
- **What leaves the machine.** Where the glen plugin's hooks run, each turn is
  sent to the glen server: the user's prompts, the agent's replies, its
  thinking where the agent exposes it, tool calls and tool results, plus the
  agent name, model, working directory, repo, branch, commit, and git remote.
  Secrets are redacted on the server before anything is stored. A session with only the MCP connection
  and no hooks is not captured; glen receives only what the agent passes to a
  glen tool.

Then make the change and confirm the new state to the user.

Switch orgs only when the user explicitly asks: it changes which org receives
their data. An MCP connection cannot switch orgs; the user reconnects and picks
another org (or runs `glen org switch` on a machine with the CLI).

## Limits

- Every change must reach the glen server. If the tool or command fails, the
  setting did NOT change; tell the user.
- Owners and admins can see each member's current mode and suggestion
  settings in the dashboard under Admin > Controls.
