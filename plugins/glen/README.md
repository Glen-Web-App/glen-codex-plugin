# glen: Codex plugin

Shared team memory for coding agents. Glen automatically recalls relevant context at
the start of every turn and captures what you build, so your whole team's agents share
the same institutional knowledge.

## What it does

`glen install` registers these hooks in `~/.codex/hooks.json`, each a one-line
`glen` CLI call:

- **SessionStart** (`glen session-start`): injects glen's status (org, mode) as
  context, shows a notice when glen is off, incognito, or silent, and runs
  `glen doctor --auto` in the background.
- **UserPromptSubmit** (`glen ingest`): sends the prompt (plus the prior assistant
  turn and workspace context: repo, branch, agent name) to your glen org, retrieves
  matching memories, and injects them as additional context for the model.
- **Stop** (`glen ingest`): records the finished turn.
- **PostToolUse** on Bash (`glen pr-link`): after a `git commit`, links the commit
  to the session in glen; when a command prints a GitHub PR URL for this clone's repo,
  has the agent offer Glen's local reviewer (`glen review <n> --detach`), once per PR.

Every hook exits 0, so a glen outage never blocks your Codex session. The plugin
itself ships only skills.

`glen off` injects and records nothing. In incognito (`glen incognito`) recall still
works but nothing is recorded to team memory. Glen reads only the hook input, the
agent's session transcript, and git metadata for the current repo.

## Skills

The plugin ships skills the agent invokes on demand:

- **search**: search the team's shared glen memory for a specific fact,
  decision, or past discussion.
- **code-search**: trace a piece of code to the agent conversations that produced it.
- **forget**: correct glen's memory by forgetting wrong or outdated evidence.
- **controls**: turn glen on/off, go off the record (incognito), go silent,
  toggle skill suggestions, or switch which organization's memory is active,
  only when you explicitly ask.
- **setup**: set up, fix, or update glen on this machine. If glen is ever
  broken (not connected, no org selected, hooks missing), just ask the agent to
  "set up glen" and it repairs whatever `glen doctor` reports.
- **create-skill** / **use-skill**: save a workflow as a skill, or find and run
  one from the Glen skill library.
- **create-artifact** / **use-artifact**: save a document to your Glen artifact
  library, or open one from it.
- **import-transcripts**: import old local agent sessions into glen memory.
- **invite**: invite a teammate to your glen organization.
- **feedback**: send a bug report or product feedback to the Glen team.
- **session-takeover**: open a shared transcript from a Glen takeover code in a
  fresh session.

## Install

1. Install the glen CLI (glen's hooks call it):

   ```bash
   npm install -g @tryglen/cli
   glen login
   ```

2. Register the plugin and glen's hooks:

   ```bash
   glen install
   ```

   `glen install` runs the Codex marketplace/plugin steps and registers
   glen's hooks in Codex's user config layer (`~/.codex/hooks.json`). Codex
   runs only hooks you have trusted, so approve the glen hooks if Codex asks
   (or open Codex and run `/hooks`).

After login your active organization is saved locally. Switch orgs at any time with
`glen org switch`.

## Uninstall

```bash
glen uninstall
```

Removes the glen plugin and marketplace, glen's hook entries and trust records,
and the rest of glen's local setup.

## What data is sent

On every `UserPromptSubmit` and `Stop` hook, glen sends to your glen org:

- The current user prompt
- The prior assistant turn (from `last_assistant_message`)
- Workspace metadata: repo name, branch, commit hash, remote URL
- Agent name (`codex`) and session details

**Nothing is recorded while incognito is on.** Recall still works: glen fetches
relevant memories but writes nothing to team memory. Admin analytics still count your
prompts as numbers only. Toggle with `glen incognito` / `glen on`. `glen off` injects
and records nothing.

The glen CLI also reports content-free error codes about itself (error code, command and flag names, CLI, OS and agent
versions; never prompts, paths, flag values or content), incognito included. Turn this off with
`glen telemetry disable` or `DO_NOT_TRACK=1`.

## Updating

To update the plugin:

```sh
codex plugin marketplace upgrade glen
```

Glen's `session-start` hook also runs this upgrade in the background on each session
start (via `glen doctor --auto`).

To update the glen CLI itself:

```sh
glen update
```

`glen update` updates the CLI **and** any installed glen plugins in one go, and
re-verifies the Codex hook registration. The CLI also checks for updates hourly in
the background and upgrades automatically when installed via npm global.

## Troubleshooting

**Check session status (statusline, org, mode):**

```sh
glen statusline
glen status
```

**Full diagnostics:**

```sh
glen doctor
```

**Broken setup?** Ask the agent to "set up glen"; the bundled setup skill
runs `glen doctor` and fixes whatever it reports.

**Hooks not firing:** Re-run `glen install`. It idempotently repairs the hook
registration in `~/.codex/hooks.json` and the trust records in `~/.codex/config.toml`.
Codex silently skips untrusted hooks, so open Codex, run `/hooks`, and trust the glen
hooks.

**Stale update lock** (if `glen update` hangs): remove the lock file and retry:

```sh
rm -rf ~/.glen/update.lock
glen update
```

**No active organization error:** run `glen org switch` to select an org, or
`glen org list` to see your memberships. If the list itself fails with an auth error,
run `glen login` to reconnect.
