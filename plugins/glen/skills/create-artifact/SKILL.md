---
name: create-artifact
description: Save a document you produced (a report, runbook, spec, dashboard, HTML page) to your Glen artifact library, private to you by default, sharable with teammates or the whole org. Use when the user says "save this as an artifact", or asks to share or delete an artifact they saved. When you've generated a substantial standalone document, offer to save it; don't save it unasked.
---

## Purpose

Save a durable, standalone document to the user's Glen artifact library: Markdown prose, or a self-contained HTML+CSS page with no JavaScript. It is private to the user by default and can be shared with teammates or the whole org. Skip ephemeral chat answers.

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

The tools are `glen_artifact_*`; your client may prefix the names.

## Supported actions

Save only when the user clearly asked for it. Sharing and deleting need their own clear request too.

| Action | MCP tool | CLI |
| --- | --- | --- |
| Save | `glen_artifact_create` with `title`, `format` (`markdown` or `html`), `content` | `glen artifact create --file <path> --title "<title>" --format auto` |
| Overwrite your own artifact | add `update: true` | add `--update` |
| Share with the whole org on save | add `shareWithOrg: true` | add `--org` |
| Share later | `glen_artifact_share` with `slug`, `emails` and/or `org: true` | `glen artifact share <slug> --with <email>[,<email>…]` and/or `--org` |
| Delete | `glen_artifact_delete` with `slug` | `glen artifact delete <slug>` |

1. **Prepare the document.** To link another artifact or skill, write `[label](glen:artifact/<slug>)` or `[label](glen:skill/<slug>)`. Over MCP, pass the document as `content`. Over the CLI, write it to a local file (`.md` or `.html`) first, or pipe it with `--file -` (then `--title` is required).
2. **Save it.** Over MCP, call `glen_artifact_create` with `title`, `format` and `content`. Over the CLI, run `glen artifact create --file <path> --title "<title>" --format auto`. The title goes only in `--title`, never as a bare word.
3. **Check references.** If the result lists unresolved references (`unresolvedSlugs`), a linked slug does not exist yet. Tell the user, then fix the link or create the missing artifact or skill.
4. **Confirm.** Tell the user the slug and its visibility: private by default (only they can see it, at `https://app.tryglen.com/artifacts/<slug>`), or shared with the whole org. To share with specific teammates or get a link, they can also click **Share** on the artifact's page.
5. **Share or delete (author only, when asked).** Sharing is additive: existing access is kept, and removing access is done in the dashboard's Share dialog. Deleting is permanent: confirm the slug with the user first.

## Limits

- Markdown or HTML+CSS only, no JavaScript, up to 200,000 characters. Over MCP, `format` must be `markdown` or `html`; only the CLI infers it (`auto`).
- The title becomes the slug, unique in the org. A taken name is a conflict: pick another title, or pass update for an artifact you authored.
- Only the author can update, share or delete an artifact. An update without the org flag keeps the current sharing.
