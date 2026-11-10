# Changelog

## 4.8.0 — 2026-11-10

- Skill: rewritten as one page of 3.8 KB; it was eighteen rules in 6.5 KB. Its rules are the text the Thegram server sends as its instructions, byte for byte (`thegram-skill` and `thegram-vault` pin the same SHA-256), so an agent reads one contract whether it is given the skill, the instructions or both. The rules route a request to one tool, first match wins: remember with `vault_note` only when asked, in any language; a count with `vault_exact`; public data through `vault_ask {op:"hubs"}`; what can be read or what changed with `vault_ask`; a document with `vault_change` and `vault_add`; and anything else that may depend on what the person said with `vault_recall`, before asking the person for the missing detail. The bundled `thegram-cloud` skill is the 4.8.0 release of the shared skill source, byte for byte (SHA256 `f59415259a646cab725f453485ac50cdb217ec29bbbf91466a8e91bc0cc75b9e`). Versions of the three plugins move together.
- The read-only check in the README now asks which vaults the key can read, which the rules send to `vault_ask {op:"browse"}`; the former question is now answered by `vault_recall`.
- Publish after the server release that lists `vault_recall` and `vault_note`, drops the `guide` and `feedback` ops and sends these rules (thegram-one/thegram-vault #416, #417 and the unified-rules change).

## 4.7.5 — 2026-11-09

- Skill: a move, delete or restore takes no `request_id` and refuses one; only a create and an edit carry it. The bundled `thegram-cloud` skill is now the 4.7.5 release of the shared skill source, byte for byte (SHA256 `1164747b037d14ec829e6bd1ba39d9adbf4702ed909fa33c13dcf1babaa573e8`). Versions of the three plugins move together.

## 4.7.4 — 2026-11-07

- First release of the API-key plugin as its own repository. Until 4.7.3 the API-key setup lived in the shared `thegram-one/thegram-plugin` repository as README commands; the Claude directory held that repository for a credential read from the user's environment, so the API-key path now has its own plugin and the Claude plugin carries none of it.
- Claude Code: the `thegram-api` plugin declares the `thegram` MCP server with an `Authorization: Bearer ${user_config.api_key}` header and a sensitive, required `api_key` option. `/plugin configure thegram-api@thegram-api` takes the key at a masked prompt and Claude Code keeps it in the system credential store, so the key never sits in a plugin file or an environment variable.
- Codex, OpenCode and OMP: the README gives the connection commands (`--bearer-token-env-var THEGRAM_API_KEY`, `--header`, and the `/mcp` settings) and the settings for any other MCP client or backend service.
- Bundle the same `thegram-cloud` skill as the other two plugins, the 4.7.2 release of the shared skill source, byte for byte (SHA256 `f1a54ec2ed391e5db10618c6e37441d005246b3be6907c9355dd0adf350088f0`). Versions of the three plugins move together.
