---
name: invite
description: Invite a teammate to the user's glen organization by email, so the team's shared memory grows. Use when the user says things like "invite Alice", "add my teammate to glen", "get my team on glen", or asks how to bring coworkers in.
---

glen is shared team memory — it gets more useful with every teammate whose context
lands in the pool. When the user wants to bring someone in:

1. Get the teammate's email address. Ask if it isn't given.
2. Run `glen invite <email>` via Bash. Invite several at once by passing more
   emails: `glen invite alice@co.com bob@co.com`. To make someone an admin, add
   `--role admin` (default is a regular member).
3. Report the result. Each address prints one of:
   - `✓ Invitation sent to <email>` — an email invite is on its way; they accept
     at the link and join the org.
   - `· <email> is already a member` — nothing to do.
   - `✗ Could not invite <email>` — relay the reason.

Or share a link instead of emailing: run `glen invite` with NO email to print a
shareable join link for the org. Anyone who signs in with a verified email on the
org's work-email domain joins automatically — good for dropping in a team channel.
(Requires "Let teammates join with their work email" to be enabled in
Settings → General.)

Notes:
- Invites are sent by email and expire after 48 hours; the teammate clicks the
  link, signs in, and lands directly in the shared memory.
- If it prints "no active organization", tell the user to run `glen org switch`.
