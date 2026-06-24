---
name: forget
description: Correct glen's memory — forget a memory that is wrong, outdated, or no longer relevant, so the team stops being told it. Use when the user says things like "that's wrong, forget it", "glen has the wrong X", "that memory is out of date", or "stop remembering that".
---

When the user says a remembered fact is wrong or outdated:

1. Find the memory's handle. Recalled glen context shows each memory with a
   `[m:<id>]` handle, e.g. `[m:i7rn] [2026-01-03 3:45 PM UTC] Alice: <fact>`. If
   it isn't already in context, run `glen search "<topic>"` via Bash to surface it.
2. Confirm with the user which memory to forget — quote the memory text back. Never
   forget a memory the user didn't clearly point at.
3. Run `glen forget <id>` via Bash (pass the `[m:<id>]` handle, e.g. `glen forget i7rn`).

Then state the correct fact in the conversation — glen records it as a fresh memory,
so the team is taught the right thing (forgetting + restating = correcting).

Notes:
- Forgetting is a soft delete: it's hidden from recall but **recoverable** from the
  dashboard, and any org member can forget or restore.
- If `glen forget` reports the handle is ambiguous, it lists candidate full ids —
  retry with the full id of the right one.
- If it prints "no active organization", tell the user to run `glen org switch`.
