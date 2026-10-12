# Connection and discovery

Read this when connecting an AI client or choosing a transport. Client-specific setup, including global configuration for Claude Code, Claude/Claude Desktop, Codex, OpenCode, Cursor and VS Code, is maintained in [MCP clients](https://classive.app/docs/mcp.md). Merge settings into existing configuration; do not replace other servers.

## Choose access

| Access | Endpoint | Credential | Use |
| --- | --- | --- | --- |
| MCP | `https://api.classive.app/mcp` | `cmcp_live_…` | Discover actions, quote, start, inspect jobs and control curricula |
| REST | `https://api.classive.app` | `ck_live_…` | Application integration, uploads, downloads and REST-only controls |
| JS/TS or Python SDK | Same REST API through [SDK resource methods](sdks.md) | `ck_live_…` | Typed application code, polling and file helpers |
| Dashboard | `https://classive.app/app` | Signed-in session | Human management and file access |

Use the credential already configured for the intended organisation. Owners/admins create separate MCP and REST keys on the same dashboard Keys page. Installing this skill does not install a key or connect a client.

Classive MCP uses Streamable HTTP with a fixed bearer header. It does not offer OAuth. Follow the selected client's current header/secret syntax; another client's JSON or environment expansion is not portable. For language SDK installation and a read-only check such as `voices.list()`, load [SDKs](sdks.md).

## Discover before acting

1. Find the relevant action with `search_actions({query: "…"})` and read its returned `inputSchema` and description. Queries such as `material`, `curriculum`, `quote` and `job` locate related actions.
2. Use the direct `quote_job`, `start_job` and `get_job` tools when available. Other actions use `execute_action({actionId, params})` with the discovered exact arguments.
3. Read `structuredContent` or the JSON text response and check `isError`. Do not assume HTTP 200 means the action succeeded.
4. If an operation is absent from MCP, use its documented REST equivalent with a REST credential, or the dashboard. Do not send arbitrary paths through `execute_action` or invent an action name.

Common read actions are `get_prices`, `list_voices`, `list_materials`, `get_material`, `get_quote`, `list_jobs`, `get_job`, `get_job_usage`, `get_package`, `list_curricula` and `get_curriculum`. Discover their current schemas rather than treating this list as an exhaustive contract.

## Check the connection

A discovery/read call is enough to check connectivity; paid generation is unnecessary. Use a read-only key for that check. A 401 can indicate a missing, revoked or wrong-kind key; a 403 can indicate origin, scope, organisation or edge access restrictions. Preserve the actual error code and response. Do not widen scopes or disable security to fix a connection error.

When downloads or uploads are needed after an MCP workflow, explain the separate REST/dashboard requirement. Do not mint a new credential merely because the MCP key cannot authenticate REST.

Live references: [Authentication](https://classive.app/docs/authentication.md), [REST quickstart](https://classive.app/docs/quickstart.md), [MCP guide](https://classive.app/docs/mcp.md).
