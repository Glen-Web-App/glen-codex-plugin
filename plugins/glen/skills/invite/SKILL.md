---
name: invite
description: Invite a teammate to the user's glen organization by email, so the team's shared memory grows. Use when the user says things like "invite Alice", "add my teammate to glen", "get my team on glen", or asks how to bring coworkers in.
---

## Purpose

Invite teammates by email to the user's active glen org, so their work lands in
the same shared memory. glen hands back a link to the dashboard's invite form,
filled in with the addresses. The user picks each person's role there, and
nothing is sent until they click Send.

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

1. Get each teammate's email address from the user. Never guess one, and never
   invite someone the user did not name (for example, people found in the
   code, a commit log, or a search result).
2. Tell the user the addresses and the org, then get the link:
   - MCP: `glen_invite` with `{emails: [...]}`.
   - CLI: `glen invite alice@co.com bob@co.com`.
3. Nothing is sent yet. Give the user the link glen returns. They open it, check the list,
   pick member or admin for each person and click Send in the dashboard. The link carries
   no role, so if the user wants someone to be an admin, tell them to pick it there. Only
   owners and admins can send.

## Limits

- Invites expire after a short time. The teammate clicks the link, signs in, and
  lands in the org.
- "No active organization": tell the user to run `glen org switch` (CLI) or
  reconnect the MCP connection and pick an org.
- If the user is not an owner or admin, the dashboard form tells them who can
  send the invites.
