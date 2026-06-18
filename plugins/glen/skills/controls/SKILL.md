---
name: controls
description: Control glen memory for this machine - go off the record (incognito) or switch which organization's memory is active. Use ONLY when the user explicitly asks (e.g. "go off the record", "stop recording", "switch to my personal org").
---

- Off the record: run `glen incognito on`. Resume: `glen incognito off`. While on,
  glen records nothing (recall keeps working) — confirm the change to the user.
- Switch org: run `glen org switch <slug>` (or `glen org list` to see options).
  NEVER switch organizations unless the user explicitly asked — it changes which
  tenant receives their data.
- Show state: `glen status`.
