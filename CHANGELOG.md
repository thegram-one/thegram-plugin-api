# Changelog

## 4.7.4 — 2026-11-07

- First release of the API-key plugin as its own repository. Until 4.7.3 the API-key setup lived in the shared `thegram-one/thegram-plugin` repository as README commands; the Claude directory held that repository for a credential read from the user's environment, so the API-key path now has its own plugin and the Claude plugin carries none of it.
- Claude Code: the `thegram-api` plugin declares the `thegram` MCP server with an `Authorization: Bearer ${user_config.api_key}` header and a sensitive, required `api_key` option. `/plugin configure thegram-api@thegram-api` takes the key at a masked prompt and Claude Code keeps it in the system credential store, so the key never sits in a plugin file or an environment variable.
- Codex, OpenCode and OMP: the README gives the connection commands (`--bearer-token-env-var THEGRAM_API_KEY`, `--header`, and the `/mcp` settings) and the settings for any other MCP client or backend service.
- Bundle the same `thegram-cloud` skill as the other two plugins, the 4.7.2 release of the shared skill source, byte for byte (SHA256 `f1a54ec2ed391e5db10618c6e37441d005246b3be6907c9355dd0adf350088f0`). Versions of the three plugins move together.
