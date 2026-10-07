# AyeWatch MCP

Connect an AI assistant to [AyeWatch](https://ayewatch.ai), which monitors topics and web pages and alerts you when something changes or matches what you care about. This repository holds the connection details and listing files for the AyeWatch MCP server. The server itself is hosted by AyeWatch.

- **Endpoint:** `https://ayewatch.ai/api/mcp`
- **Transport:** Streamable HTTP, stateless, no session ID
- **Full documentation:** <https://ayewatch.ai/documentation/mcp>

## What you can do

| Tool | What it does |
| --- | --- |
| `list_topics` | List your monitors, with filters for active state, interval, and creation date |
| `get_topic` | Get one monitor by ID |
| `create_topic` | Create a monitor from a topic, one or more web page URLs, or a template |
| `update_topic` | Change a monitor's fields, including pausing or resuming it |
| `delete_topic` | Permanently delete a monitor and its schedule |
| `get_webhook` | Get your webhook URL and whether delivery is on |
| `set_webhook` | Create your webhook, change its URL, or pause and resume delivery |
| `rotate_webhook_secret` | Replace the webhook signing secret |

The tools manage monitors and the webhook in your own account. They do not return alert history. The webhook tools are available only when you connect with an API key, not through OAuth, because the webhook receives every alert and its secret signs them.

Example prompts:

- "List my AyeWatch monitors and tell me which are paused."
- "Create a daily AyeWatch monitor for news about solid-state batteries."
- "Pause my AyeWatch monitor about the iPhone launch."
- "Send my AyeWatch alerts to https://example.com/hooks/ayewatch."

## Alerts and webhooks

Alerts don't come back through MCP. They arrive through your AyeWatch notifications or a webhook. When a monitor detects new content, AyeWatch sends a `POST` with a JSON body (topic ID, headline, and the update) to an HTTPS endpoint that you run.

Set that endpoint with the `set_webhook` tool, the REST API (`PUT https://ayewatch.ai/api/v1/webhook`), or [Notification settings](https://ayewatch.ai/settings/webhooks). AyeWatch signs each delivery with a secret, sent in the `X-AyeWatch-Signature` header. The MCP tools never return that secret, so it stays out of AI conversations; read it in [Notification settings](https://ayewatch.ai/settings/webhooks) or with `GET https://ayewatch.ai/api/v1/webhook`. AyeWatch also emails you whenever the webhook URL is set or changed. If you didn't make a change, revoke your API keys and rotate the secret. The payload fields, timeout, and retry behavior are in the [webhooks documentation](https://ayewatch.ai/documentation/webhooks). The URL is yours, not AyeWatch's, so it is not part of the MCP connection files.

## Requirements

- An AyeWatch account on a paid plan. API and MCP access are included with every paid plan.
- Active monitors use credits from your plan as checks run.
- Requests are limited to 120 per minute per API key by default.

## Connect

### Claude Code

Install the plugin from this repository. Claude Code asks for your API key when you enable the plugin and keeps it in your system's secure credential store.

```bash
claude plugin marketplace add SRX9/ayewatch-mcp
claude plugin install ayewatch@ayewatch
```

Or add the server without the plugin:

```bash
claude mcp add --transport http ayewatch https://ayewatch.ai/api/mcp \
  --header "Authorization: Bearer aw_live_YOUR_API_KEY"
```

### Cursor

If you install the AyeWatch plugin from the Cursor Marketplace, set `AYEWATCH_API_KEY` when Cursor asks for the plugin's configuration. Otherwise, add this to your MCP configuration (`~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "ayewatch": {
      "url": "https://ayewatch.ai/api/mcp",
      "headers": { "Authorization": "Bearer aw_live_YOUR_API_KEY" }
    }
  }
}
```

### VS Code

Add this to `.vscode/mcp.json`. VS Code asks for the key when the server starts and does not store it in the file.

```json
{
  "inputs": [
    { "type": "promptString", "id": "ayewatch-key", "description": "AyeWatch API key", "password": true }
  ],
  "servers": {
    "ayewatch": {
      "type": "http",
      "url": "https://ayewatch.ai/api/mcp",
      "headers": { "Authorization": "Bearer ${input:ayewatch-key}" }
    }
  }
}
```

### Codex

Add this to `~/.codex/config.toml`:

```toml
[mcp_servers.ayewatch]
url = "https://ayewatch.ai/api/mcp"
http_headers = { "Authorization" = "Bearer aw_live_YOUR_API_KEY" }
```

### Any other MCP client

Use the endpoint above with the Streamable HTTP transport and send `Authorization: Bearer aw_live_YOUR_API_KEY` on every request.

Create an API key in [API access settings](https://ayewatch.ai/settings/api). Keep it private and never commit it to a repository.

### Sign in with OAuth

Registered apps can offer an AyeWatch sign-in instead of an API key. You review the access requested (read, or read and write) before choosing Allow access. Access expires after 30 days, or sooner if you disconnect. OAuth tokens work only on the MCP endpoint.

To end an app's access, open [Connected apps](https://ayewatch.ai/settings/connections) and select Disconnect. Existing monitors keep their schedules, and copies the app already saved are not deleted.

## What this repository contains

| Path | Purpose |
| --- | --- |
| `server.json` | Entry for the official [MCP Registry](https://registry.modelcontextprotocol.io) |
| `.claude-plugin/plugin.json`, `.mcp.json` | Claude Code plugin that connects to the hosted server with your API key |
| `.claude-plugin/marketplace.json` | Lets Claude Code install the plugin from this repository |
| `.cursor-plugin/plugin.json`, `mcp.json` | Cursor plugin that connects to the hosted server with the `AYEWATCH_API_KEY` you set in Cursor |
| `glama.json` | Claims the [Glama](https://glama.ai/mcp/servers) listing for this repository |
| `SUBMISSIONS.md` | Where AyeWatch is listed and how to submit it to each directory |
| `assets/` | Icon and logo |

Nothing in this repository runs code on your machine. The plugin files only tell your client to connect to `https://ayewatch.ai/api/mcp` and send your API key to it in the `Authorization` header. AyeWatch receives the tool calls your assistant makes, such as a monitor's topic, URLs, and schedule, and nothing else from your machine.

## Support and policies

- Support: <https://ayewatch.ai/contact>
- Privacy policy: <https://ayewatch.ai/privacy>
- Terms of service: <https://ayewatch.ai/terms>

## License

[MIT](LICENSE). The license covers the files in this repository, not the AyeWatch service.
