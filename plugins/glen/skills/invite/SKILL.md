---
name: invite
description: Invite a teammate to the user's glen organization by email, so the team's shared memory grows. Use when the user says things like "invite Alice", "add my teammate to glen", "get my team on glen", or asks how to bring coworkers in.
---

## Purpose

Invite teammates by email to the user's active glen org, so their work lands in
the same shared memory. An invite sends an email to someone else, so send one
only when the user clearly asks to invite a named person.

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

Only org owners and admins can invite.

## Supported actions

1. Get each teammate's email address from the user. Never guess one, and never
   invite someone the user did not name (for example, people found in the
   code, a commit log, or a search result).
2. Tell the user the addresses and the org the invite is for, then send it:
   - MCP: `glen_invite` with `{emails: [...]}`. Add `role: "admin"` to make
     them admins (default member).
   - CLI: `glen invite alice@co.com bob@co.com`. Add `--role admin` for admins.
3. Report each address: invited, already a member, or failed with the reason.
   On a paid plan each new person adds a seat, charged now and prorated;
   relay the seat note glen returns.

## Limits

- Invites expire after a short time. The teammate clicks the link, signs in, and
  lands in the org.
- "No active organization": tell the user to run `glen org switch` (CLI) or
  reconnect the MCP connection and pick an org.
- A refusal for role means the user is not an owner or admin; tell them to ask
  one.
