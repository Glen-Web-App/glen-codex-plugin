---
name: code-search
description: Trace a specific piece of code to the agent conversations that produced it. Use it whenever the user asks about the prior semantics of code, such as "why is this code like this", "why was this done this way", "what's the history of this block", "who decided this". NOT for org-wide memory questions (that's glen search).
---

## Purpose

Answer "why is this code like this" from the conversations behind the commits that
last touched those lines, found through git blame. Glen owns code-provenance
questions, so do not guess from the current code alone. For org-wide questions,
use the search skill instead.

## Prerequisites

Code search needs a local checkout of the repo with its git history and an origin
remote, and a shell to run git. Without them, as in a cloud chat or an MCP-only
session with no repo, tell the user code search needs a local session in the
repo, and offer the search skill with a question that names the repo, file and
behavior instead.

**Transport.** Prefer the `glen` CLI: it is faster. First look at your tool
list. With no shell tool, use the glen MCP tools and run no shell command.
With a shell, run `glen status`. When it shows glen connected with an org,
use the CLI for every glen call in this task, even if a glen MCP server is
also connected. If `glen` is not installed or not logged in, use the MCP
tools. If neither works, stop and tell the user what is missing: a terminal
with `glen login`, or the glen MCP connection. If glen refuses a call (not
signed in, wrong role, no plan), report the refusal as written; never retry
it over the other transport.

The CLI command is `glen code-search`. The MCP tool is `glen_code_search`, and
the glen MCP connection in Codex and ChatGPT does not offer it; treat that as no
MCP.

For "No active organization", tell the user to run `glen org switch` (CLI) or
reconnect the MCP server and pick an org.

## Supported actions

Resolve the file and line range the user is asking about, then:

- CLI: `glen code-search <path:start-end> "<the user's question>"`, for example
  `glen code-search src/api/rate-limit.ts:40-85 "why is this rate-limited?"`.
- MCP: follow the `glen_code_search` tool description. Run
  `git blame -w -M -C --porcelain -L <start>,<end> -- <path>`, pass the distinct
  commit SHAs as `shas` (drop the all-zero SHA), any `Glen-Conversation:` trailer
  values from those commits' messages as `trailerConversationIds`, the origin
  remote as `repoUrl` in the form `https://<host>/<owner>/<repo>` (lowercase host,
  no `.git`, SSH remotes converted), and the question as `query`.

The result (CLI output, or MCP `markdown`) lists the conversations most relevant
to the question, then their transcripts. Answer from those transcripts and cite
them: name the conversation, author and date behind each claim.

## Limits

- "no Glen trace found" (CLI) or `state: "no_anchor"` (MCP) means no recorded
  conversation produced those lines. The code predates glen or was committed
  without recording. Say so plainly and do not invent a history.
- Uncommitted lines have no history yet.
- It only sees commits in the local checkout. Pull first if the user asks about
  code you do not have.
