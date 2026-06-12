# glen — Codex plugin

Shared team memory for coding agents. Glen automatically recalls relevant context at
the start of every turn and captures what you build, so your whole team's agents share
the same institutional knowledge.

## What it does

When you submit a prompt in Codex, the glen plugin fires two hooks:

- **SessionStart** — announces the session to glen and injects any incognito/org
  status as a system message.
- **UserPromptSubmit** — sends the prompt (plus the prior assistant turn and workspace
  context: repo, branch, agent name) to your glen org, retrieves matching memories, and
  injects them as additional context for the model.

Nothing is sent while incognito mode is on (`glen incognito on`). Glen never reads your
filesystem directly — only what you send via prompts and assistant turns.

## Install

1. Install the glen CLI (the plugin's hooks call it):

   ```bash
   npm install -g @tryglen/cli
   glen login
   ```

2. Register the plugin and its hooks:

   ```bash
   glen install
   ```

   `glen install` runs the Codex marketplace/plugin steps AND registers
   glen's hooks in Codex's user config layer (`~/.codex/hooks.json`), then
   auto-trusts them (`[hooks.state]` records in `~/.codex/config.toml`).

After login your active organization is saved locally. Switch orgs at any time with
`glen org switch`.

## Why hooks live in `~/.codex/hooks.json`

Codex does not fire hooks declared by plugin manifests
([openai/codex#16430](https://github.com/openai/codex/issues/16430)) — the
manifest in this package enumerates them (and will start working when the
upstream fix ships), but today the only layer Codex's runtime actually
executes is the user config layer. `glen install` writes glen's two hook
lines there:

- `SessionStart` → `glen session-start --agent codex` (injects org/session context)
- `UserPromptSubmit` → `glen ingest --agent codex` (memory recall in, capture out)

Both always exit 0 — a glen outage can never block your Codex session.
Re-running `glen install` repairs the registration idempotently; other
tools' hooks in the same file are preserved. If auto-trust fails, open
Codex, run `/hooks`, and trust the glen hooks manually.

## Uninstall

```bash
glen uninstall        # removes glen's hook entries + trust records
codex plugin remove glen
```

## What data is sent

On every `UserPromptSubmit` hook, glen sends to your glen org:

- The current user prompt
- The prior assistant turn (from `last_assistant_message`)
- Workspace metadata: repo name, branch, commit hash, remote URL
- Agent name (`codex`) and session details

**Nothing is sent while incognito is on.** Recall still works — glen fetches relevant
memories but writes nothing back. Toggle with `glen incognito on` / `glen incognito off`.

Glen never sends data to any third party. All memory is stored in your org's private
glen instance.

## Updating

To update the plugin:

```sh
codex plugin marketplace upgrade glen
```

Glen's `session-start` hook also checks for a stale plugin version in the background
on each session start and nudges you when an upgrade is available.

To update the glen CLI itself:

```sh
glen update
```

The CLI also checks for updates daily in the background and upgrades automatically
when installed via npm global.

## Troubleshooting

**Check session status (statusline, org, incognito):**

```sh
glen statusline
glen status
```

**Full diagnostics:**

```sh
glen doctor
```

**Hooks not firing:** Re-run `glen install` — it idempotently repairs the hook
registration in `~/.codex/hooks.json` and the trust records in `~/.codex/config.toml`.
Codex silently skips untrusted hooks; if auto-trust failed, open Codex, run `/hooks`,
and trust the glen hooks manually.

**Stale update lock** (if `glen update` hangs): remove the lock file and retry:

```sh
rm -rf ~/.glen/update.lock
glen update
```

**No active organization error:** run `glen org switch` to select an org, or
`glen org list` to see your memberships. If the list itself fails with an auth error,
run `glen login` to reconnect.

---

> **Note:** This repository is generated from the
> [glen monorepo](https://github.com/Glen-Web-App/glen) (`packages/codex-plugin`).
> Please open pull requests and issues there, not here.

---

## Maintainer note — verified plugin.json shape

The `plugin.json` manifest shape was verified against the Rust deserializer struct in
the openai/codex source on **2026-06-11**:

**Source:** `codex-rs/core-plugins/src/manifest.rs` — `RawPluginManifest` struct

Verified fields (all optional via `#[serde(default)]` except `name`):

| JSON key | Rust type | Notes |
|---|---|---|
| `name` | `String` | Required (defaults to dir name if empty) |
| `version` | `Option<String>` | |
| `description` | `Option<String>` | |
| `keywords` | `Vec<String>` | |
| `skills` | `Option<RawPluginManifestPath>` | **Single path string** (e.g. `"./skills"`), not an array |
| `mcpServers` | `Option<String>` | camelCase from `mcp_servers` |
| `apps` | `Option<String>` | |
| `hooks` | `Option<RawPluginManifestHooks>` | Path string, path array, inline object, or inline array |
| `interface` | `Option<RawPluginManifestInterface>` | Nested display metadata |

**Fields NOT in the struct** (no `deny_unknown_fields` so they are silently ignored):
`author`, `homepage`, `repository`, `license`.

**Path resolution:** `load_plugin_manifest(plugin_root)` resolves every manifest path
(`skills`, `hooks`, `mcpServers`, `apps`) against the **plugin root directory**, not
against `.codex-plugin/` where this manifest lives — so `"./hooks/hooks.json"` and
`"./skills"` are correct as written. Do not "fix" them to `../`.

The plan's plugin.json proposed `"author": "Glen <https://tryglen.com>"` and `"license": "MIT"` —
these are harmless but not consumed by Codex. They were omitted from the final plugin.json
to keep the manifest minimal and avoid drift if the struct ever adds conflicting fields.

**marketplace.json** verified against `RawMarketplaceManifest` in
`codex-rs/core-plugins/src/marketplace.rs` on the same date:

| JSON key | Rust type | Notes |
|---|---|---|
| `name` | `String` | Required |
| `interface` | `Option<RawMarketplaceManifestInterface>` | Optional; only `displayName` inside |
| `plugins` | `Vec<RawMarketplaceManifestPlugin>` | Required |

Plugin entry (`RawMarketplaceManifestPlugin`): `name` (required), `source` (required),
`policy` (optional), `category` (optional). **No `description` field on plugin entries.**

The plan's marketplace.json included a `description` field on the plugin entry — this
is silently ignored (no `deny_unknown_fields`). It was omitted from the final file.

The plan also proposed an `owner` field at the marketplace root — this does not exist in
the struct and was omitted.

`source` uses an untagged enum; the `{ "source": "local", "path": "..." }` object form
maps to `RawMarketplaceManifestPluginSourceObject::Local`.
