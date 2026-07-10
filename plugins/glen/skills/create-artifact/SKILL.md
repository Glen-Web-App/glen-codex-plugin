---
name: create-artifact
description: Save a document you produced (a report, runbook, spec, dashboard, HTML page) to your Glen artifact library — private to you by default, sharable with teammates or the whole org. Use when the user says "save this as an artifact", or when you've generated a substantial standalone document worth keeping.
---

1. **Decide it's worth saving.** Save when the output is a durable, standalone document (Markdown prose or a self-contained HTML+CSS page — no JavaScript). Skip ephemeral chat answers.
2. **Check the record.** Run `glen status` via Bash. If incognito/off, tell the user nothing will be saved and stop unless they switch on.
3. **Write it to a file.** Save the document to a local file (`.md` for Markdown, `.html` for HTML+CSS). To link to another artifact or skill, use `[label](glen:artifact/<slug>)` or `[label](glen:skill/<slug>)`.
4. **Save it.** Run `glen artifact create --file <path> --title "<title>" --format auto` via Bash. The artifact is **private to the user by default**; add `--org` if the user asked to share it with their whole org. Add `--update` to overwrite your own same-named artifact (an update without `--org` keeps the current sharing). If it reports unresolved references, the linked slug doesn't exist yet — create it or fix the link.
5. **Confirm.** Tell the user the artifact slug and its visibility: private by default (only they can see it in the Glen dashboard under Artifacts), or shared with the org if `--org` was passed. To share it with specific teammates — or grab a shareable link — they can open the artifact's page and click **Share** (the link is access-controlled; only people it's shared with can open it).
