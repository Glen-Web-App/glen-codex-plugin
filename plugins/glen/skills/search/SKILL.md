---
name: search
description: Search the team's shared glen memory for a SPECIFIC fact, decision, or past discussion that is not already in the auto-injected context. Do not use for things already recalled this turn.
---

Run `glen search "<query>"` via Bash — **start with ONE comprehensive query, not a
fan-out of variants**.

Each search takes ~10 seconds of real user-facing wait, and the server ranks results
with an LLM that reads your query as free-text intent — so one detailed, self-contained
query beats several keyword variants. Pack everything you know into a single query:
who/what/where, the repo or system involved, and what kind of answer you need (a
decision, an owner, a rationale, setup steps).

- ✅ `glen search "Shams's recent work on the usage-page redesign in the dashboard repo — what was decided, current status, and who owns it"`
- ❌ `glen search "Shams"` … `glen search "usage page redesign"` … `glen search "dashboard"`

If a hook or reminder suggests multiple search questions, merge the related ones into
one query and run that single search.

Follow-up searches are fine for going DEEPER — when the first result surfaces
something new worth chasing (a project you didn't know to name, a decision that
points at another decision), and the answer would genuinely change what you do next.
But every extra search is another ~10s the user sits waiting, so don't overdo it:
ask yourself whether the follow-up will provide real value the first result didn't,
and skip it if you already have enough to act. Never re-run a mere rephrase of a
query that returned little — same memory, same cost, nothing new.

The output is a dated context block — treat it as memory, cite dates when relevant,
and quote the `[m:…]` id tags when the user may want to follow up on (or correct) a
specific memory. If it prints no results, say so rather than re-searching variants.
If it prints "No active organization", tell the user to run `glen org switch`.
