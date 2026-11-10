# Thegram for API-key agents

Thegram is the AI knowledge base for enterprise AX: it turns scattered documents and decisions into linked goals, decisions, requirements, evidence and sources that people, Thegram AI and coding agents read as one record, with every change kept as history. This plugin connects coding agents and backend services to Thegram with a scoped API key instead of OAuth, and gives them the same memory skill: find an earlier decision, list a project's open risks, count records exactly, or remember what you tell it to keep.

Use it when the host cannot complete a browser sign-in: Claude Code on a server, Codex, OpenCode, OMP, or a service that speaks MCP over HTTP. If you can sign in with your Thegram account, use the OAuth plugin for your host instead.

| Host | Plugin | Sign-in |
| --- | --- | --- |
| Claude: claude.ai, Claude desktop, Claude Code, the Claude directory | https://github.com/thegram-one/thegram-plugin | OAuth |
| Codex and ChatGPT | https://github.com/thegram-one/thegram-plugin-codex | OAuth |
| Claude Code, Codex, OpenCode, OMP and services with a scoped API key (this repository) | https://github.com/thegram-one/thegram-plugin-api | API key |

Install one Thegram plugin per host. Two plugins that both register the `thegram` server give the agent two sets of the same tools.

## 1. Issue a scoped key

Sign in at https://vault.thegram.one/, open **Settings > Agents > API keys**, name the key for the agent or service, select only the vaults it needs and grant the minimum permissions. Prefer read access unless writes are required. Writing needs write access; Google Drive needs its own connection and grant. A key expires within 90 days and its secret is shown once. It cannot exceed your vault membership or your organization's policy, and a vault grant is not a sandbox for single documents.

Use a separate key for each agent or service. Never paste the key into a chat, a command argument, a repository, shell history or a URL.

## 2. Connect the host

Every host uses one endpoint, `https://vault.thegram.one/mcp` (Streamable HTTP), and sends the key as `Authorization: Bearer <key>`.

### Claude Code

```sh
claude plugin marketplace add thegram-one/thegram-plugin-api
claude plugin install thegram-api@thegram-api
```

The install reports that one required option is not set. In Claude Code run `/plugin configure thegram-api@thegram-api` and enter the key at the masked prompt; Claude Code keeps it in the system credential store, not in this plugin's files. Do not pass it with `--config api_key=...` on the command line, which puts it in your shell history. To replace the key later, run the same command, or open `/plugin`, select **thegram-api** on the **Installed** tab and choose **Configure options**. The skill appears as `/thegram-api:thegram-cloud`. Do not install the OAuth plugin `thegram-cloud` beside it.

### Codex

Store the key as `THEGRAM_API_KEY` in your own shell profile or secret manager, then:

```sh
codex mcp remove thegram >/dev/null 2>&1 || true
codex mcp add thegram --url https://vault.thegram.one/mcp --bearer-token-env-var THEGRAM_API_KEY
```

The command passes the name of the variable, not its value; Codex reads the key when it runs. Do not install the OAuth plugin `thegram-cloud` from `thegram-plugin-codex`, which registers a second `thegram` server.

### OpenCode

```sh
opencode mcp add thegram --url https://vault.thegram.one/mcp --header 'Authorization=Bearer {env:THEGRAM_API_KEY}'
```

### OMP

OMP has no add command. In a session open its MCP settings (`/mcp`) and add an HTTP server named `thegram` with URL `https://vault.thegram.one/mcp` and the header `Authorization: Bearer ${THEGRAM_API_KEY}`.

### Skill for Codex, OpenCode and OMP

The plugin form installs the skill only on Claude Code. For the other hosts copy `skills/thegram-cloud/SKILL.md` from this repository into the host's skills directory. An MCP connection alone does not install the skill.

### Other MCP clients and backend services

| Setting | Value |
| --- | --- |
| MCP server URL | `https://vault.thegram.one/mcp` |
| Transport | Streamable HTTP |
| Authentication | API key / Bearer token |
| Header name | `Authorization` |
| Header value | `Bearer <key>` |

If the client has a Bearer-token field, enter just the key without a second `Bearer ` prefix. This is MCP over HTTP (JSON-RPC 2.0), not a REST API: use the client's MCP SDK, let it send `initialize`, negotiate the protocol version and carry the returned session headers, and include the Bearer header on every request. Do not guess routes from tool names. Keep the key in an encrypted credential store or a server-side secret manager, send it only to `vault.thegram.one`, and never follow a redirect with it. Only connect services you trust: they receive the key and can reach the authorized data. A client that cannot send a custom `Authorization` header must use an OAuth connector instead of exposing the key in a frontend.

## 3. Verify without writing

Start a new agent session; installation does not change a running one. Ask:

```text
Which Thegram vaults can you read? Read only: do not create or change any records.
```

The agent should call `vault_ask` with `op` `browse` and list the vaults the key reaches. If it fails, stop: do not fall back to a local app or to a different vault. HTTP 401 means the key is missing, malformed, expired or revoked; `AGENT_SCOPE_DENIED` means the key was not granted that vault. Do not create test records in real memory to prove the connection.

## 4. Revoke and rotate

Revoking the key in **Settings > Agents** stops server access; removing only the host's connection does not revoke it. To rotate, issue a new scoped key, update the host, verify read-only access, then revoke the old key. Never share a key to resolve another person's error.

## Data and permissions

Memory requests go to `https://vault.thegram.one/mcp`. Requests can contain search questions, selected document and record identifiers, and content and source references the user authorizes the agent to save. The server returns the authorized vault or separately connected Drive data requested through that key. Each agent has its own access scope; manage grants and disconnect agents in **Settings > Connected agents** at https://vault.thegram.one/settings/agents.

Retrieval and drafts stay read-only. Writes stay within the approved purpose and destination, and require the user's authorization. The plugin itself does not fetch Slack, Gmail or unrelated connected-app data. This plugin declares no local MCP command, lifecycle hook or script, and bundles no credential: the only secret is the key you enter, and Claude Code keeps it in the system credential store.

- Privacy: https://vault.thegram.one/legal/privacy
- Terms: https://vault.thegram.one/legal/terms
- Docs: https://docs.thegram.one
- Support: https://github.com/thegram-one/thegram-plugin-api/issues

License: [Apache-2.0](LICENSE). [NOTICE](NOTICE) reserves the Thegram name and logo; the Thegram service and apps are provided under their own [terms](https://vault.thegram.one/legal/terms).
