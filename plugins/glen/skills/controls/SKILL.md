---
name: controls
description: Control glen memory - turn glen on/off, go off the record (incognito, account-wide), go silent (keep recording, surface nothing), toggle glen's skill suggestions, or switch which organization's memory is active. Use ONLY when the user explicitly asks (e.g. "turn glen off", "go off the record", "stop recording", "stop suggesting skills", "just record, don't inject anything", "switch to my personal org").
---

glen has four modes. Always confirm the change to the user.

- **On:** `glen on` — glen injects context and records everything (the default).
  **Account-wide:** resumes recording on every machine, agent, and connected client.
- **Off:** `glen off` — glen injects nothing AND records nothing **on this machine**
  until `glen on`. Use when the user wants glen fully paused/disabled here.
- **Incognito (off the record):** `glen incognito` — glen keeps recalling/injecting
  but records nothing until `glen on`. **Account-wide and server-enforced:** the
  server blocks memory writes for every machine, agent, and connected client, not
  just this one; the same switch lives on the dashboard's Memories page. Use for
  "go off the record" / "stop recording".
- **Silent:** `glen silent` — glen keeps recording everything but surfaces
  nothing: no recall, no suggestions, and `glen search` returns nothing, until
  `glen on`. **Account-wide and server-enforced** (the server returns empty
  results for every machine, agent, and connected client; the same switch lives
  on the dashboard's Memories page). Use for "just collect / stay out of my
  way / record but don't inject".

`glen on`, `glen incognito`, and `glen silent` need to reach the glen server (the
server is the source of truth); if the command fails, the mode did NOT change —
tell the user.

Pick by intent: "stop recording but keep helping" → `glen incognito`; "record but
don't help/inject" → `glen silent`; "turn glen off entirely / disable glen" →
`glen off`; "turn glen back on" → `glen on`.

- Skill nudges (two per-user toggles, both on by default):
  `glen suggestions skill-create off` stops glen suggesting that finished work be
  turned into a new team skill; `glen suggestions skill-use off` stops glen
  recommending existing team skills. Re-enable with `… on`. **Account-wide and
  server-enforced** (the same toggles live in the Memories page's Suggestions
  menu). Use for "stop suggesting skills" / "don't recommend skills". Like the
  modes, a failed command means the preference did NOT change — tell the user.
- Switch org: run `glen org switch <slug>` (or `glen org list` to see options).
  NEVER switch organizations unless the user explicitly asked — it changes which
  tenant receives their data.
- Show state: `glen status`.
