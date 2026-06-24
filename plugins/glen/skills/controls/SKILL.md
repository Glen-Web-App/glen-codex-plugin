---
name: controls
description: Control glen memory for this machine - turn glen on/off, go off the record (incognito), or switch which organization's memory is active. Use ONLY when the user explicitly asks (e.g. "turn glen off", "go off the record", "stop recording", "switch to my personal org").
---

glen has three modes for this machine. Always confirm the change to the user.

- **On:** `glen on` — glen injects context and records everything (the default).
- **Off:** `glen off` — glen injects nothing AND records nothing until `glen on`.
  Use when the user wants glen fully paused/disabled.
- **Incognito (off the record):** `glen incognito` — glen keeps recalling/injecting
  but records nothing until `glen on`. Use for "go off the record" / "stop recording".

Pick by intent: "stop recording but keep helping" → `glen incognito`; "turn glen off
entirely / disable glen" → `glen off`; "turn glen back on" → `glen on`.

- Switch org: run `glen org switch <slug>` (or `glen org list` to see options).
  NEVER switch organizations unless the user explicitly asked — it changes which
  tenant receives their data.
- Show state: `glen status`.
