# Changelog

## 4.8.0 — 2026-11-10

- Skill: rewritten as seven short rules, 3 KB instead of 6.5 KB and eighteen rules. It recalls with `vault_recall {query}` before acting and remembers with `vault_note {text}` when the person asks; the server files a note. Hubs, exact counts and document edits keep one rule each. The bundled `thegram-cloud` skill is the 4.8.0 release of the shared skill source, byte for byte (SHA256 `c9e07f4094b938fc0d898f6aca42692b49e5ea4421cfc7e3718d3dcabbc1e06a`). Versions of the three plugins move together.
- Publish after the server release that lists `vault_recall` and `vault_note` and drops the `guide` and `feedback` ops (thegram-one/thegram-vault #416, #417).

## 4.7.5 — 2026-11-09

- Skill: a move, delete or restore takes no `request_id` and refuses one; only a create and an edit carry it. The bundled `thegram-cloud` skill is now the 4.7.5 release of the shared skill source, byte for byte (SHA256 `1164747b037d14ec829e6bd1ba39d9adbf4702ed909fa33c13dcf1babaa573e8`). Versions of the three plugins move together.

## 4.7.4 — 2026-11-07

- First release of the API-key plugin as its own repository. Until 4.7.3 the API-key setup lived in the shared `thegram-one/thegram-plugin` repository as README commands; the Claude directory held that repository for a credential read from the user's environment, so the API-key path now has its own plugin and the Claude plugin carries none of it.
- Claude Code: the `thegram-api` plugin declares the `thegram` MCP server with an `Authorization: Bearer ${user_config.api_key}` header and a sensitive, required `api_key` option. `/plugin configure thegram-api@thegram-api` takes the key at a masked prompt and Claude Code keeps it in the system credential store, so the key never sits in a plugin file or an environment variable.
- Codex, OpenCode and OMP: the README gives the connection commands (`--bearer-token-env-var THEGRAM_API_KEY`, `--header`, and the `/mcp` settings) and the settings for any other MCP client or backend service.
- Bundle the same `thegram-cloud` skill as the other two plugins, the 4.7.2 release of the shared skill source, byte for byte (SHA256 `f1a54ec2ed391e5db10618c6e37441d005246b3be6907c9355dd0adf350088f0`). Versions of the three plugins move together.
