---
name: use-artifact
description: Fetch and display an artifact from the user's Glen library (their own artifacts plus ones shared with them). Use when the user asks to open, view, render, or download a saved Glen artifact.
---

1. **Find it.** If you don't know the slug, run `glen artifact search "<intent>"` via Bash and confirm the right one with the user. Search covers the user's own artifacts plus ones shared with them (directly or org-wide) — a teammate's unshared artifact won't appear.
2. **Fetch it.** Run `glen artifact get <slug>` via Bash. The output is the document (Markdown or HTML+CSS).
3. **Show it.**
   - **Markdown:** present it inline.
   - **HTML:** write it to a local `.html` file, then open it in the in-app browser (or run the system `open <file>`); if neither is available, share the dashboard link below.
4. **Always** share the dashboard link so the user has the canonical rendered copy: `https://app.tryglen.com/artifacts/<slug>`.
