---
name: create-artifact
description: Save a document you produced (a report, runbook, spec, dashboard, HTML page) to the org's shared Glen artifact library so teammates can find, view, and download it. Use when the user says "save this as an artifact", or when you've generated a substantial standalone document worth keeping.
---

1. **Decide it's worth saving.** Save when the output is a durable, standalone document (Markdown prose or a self-contained HTML+CSS page — no JavaScript). Skip ephemeral chat answers.
2. **Check the record.** Run `glen status` via Bash. If incognito/off, tell the user nothing will be saved and stop unless they switch on.
3. **Write it to a file.** Save the document to a local file (`.md` for Markdown, `.html` for HTML+CSS). To link to another artifact or skill, use `[label](glen:artifact/<slug>)` or `[label](glen:skill/<slug>)`.
4. **Save it.** Run `glen artifact create --file <path> --title "<title>" --format auto` via Bash. Add `--update` to overwrite your own same-named artifact. If it reports unresolved references, the linked slug doesn't exist yet — create it or fix the link.
5. **Confirm.** Tell the user the artifact slug and that it's shared with their org (viewable in the Glen dashboard under Artifacts).
