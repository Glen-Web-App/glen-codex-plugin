---
name: controls
description: Control glen memory - turn glen on/off, go off the record (incognito, account-wide), or switch which organization's memory is active. Use ONLY when the user explicitly asks (e.g. "turn glen off", "go off the record", "stop recording", "switch to my personal org").
---

glen has three modes. Always confirm the change to the user.

- **On:** `glen on` — glen injects context and records everything (the default).
  **Account-wide:** resumes recording on every machine, agent, and connected client.
- **Off:** `glen off` — glen injects nothing AND records nothing **on this machine**
  until `glen on`. Use when the user wants glen fully paused/disabled here.
- **Incognito (off the record):** `glen incognito` — glen keeps recalling/injecting
  but saves nothing to the team's memory until `glen on` (content may be retained
  for glen staff to troubleshoot the service). **Account-wide and server-enforced:** the
  server blocks memory writes for every machine, agent, and connected client, not
  just this one; the same switch lives on the dashboard's Memories page. Use for
  "go off the record" / "stop recording".

`glen on` and `glen incognito` need to reach the glen server (the server is the
source of truth); if the command fails, the mode did NOT change — tell the user.

Pick by intent: "stop recording but keep helping" → `glen incognito`; "turn glen off
entirely / disable glen" → `glen off`; "turn glen back on" → `glen on`.

- Switch org: run `glen org switch <slug>` (or `glen org list` to see options).
  NEVER switch organizations unless the user explicitly asked — it changes which
  tenant receives their data.
- Show state: `glen status`.
