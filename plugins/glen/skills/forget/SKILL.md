---
name: forget
description: Correct glen's memory — forget evidence that is wrong, outdated, or no longer relevant, so the team stops being told it. Use when the user says things like "that's wrong, forget it", "glen has the wrong X", "that memory is out of date", or "stop remembering that" — or when YOU verify that recalled evidence is contradicted by current code or state.
---

A stale memory gets caught two ways: the user points at it, or you verify it yourself.

**User-initiated** — the user says a remembered fact is wrong or outdated:

1. Identify the relevant source references. Glen search results end each source with
   an exact `observation:<id>` or `message:<id>` reference. If the result is not
   already in context, run `glen search "<topic>"` via Bash.
2. Confirm which evidence is wrong. Do not automatically forget every source in a
   synthesized answer. When several sources repeat the same stale claim, select all
   of those references.
3. Pass the returned references unchanged, in one command:
   `glen forget observation:<id> message:<id>`.

**Agent-detected** — recalled evidence is contradicted by current code or state:

1. Verify the contradiction directly. Never forget on suspicion, a partial view, or
   merely because two memories disagree; surface uncertain conflicts to the user.
2. Run `glen forget <reference...>` with only the contradicted source references.
3. Tell the user exactly what was forgotten and why.

Then state the correct fact in the conversation so Glen can record the correction.

Notes:

- Observation and message references can be mixed in one atomic command.
- Forgetting an observation hides that distilled memory from recall.
- Forgetting a message removes it from future recall and hides derived observations
  that have no other active supporting message. The raw message remains visible in
  transcript history for audit.
- Forget is idempotent; already-hidden references are reported as unchanged.
- Undo with `glen restore <reference...>`. Automatic restore never revives an
  observation that a user directly forgot.
- Legacy `[m:<id>]` and bare observation ids remain accepted.
- If a legacy suffix is ambiguous, retry with the full `observation:<id>` listed.
- If the CLI reports no active organization, tell the user to run
  `glen org switch`.
