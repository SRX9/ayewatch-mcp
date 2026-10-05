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

The tools manage monitors in your own account. They do not return alert history; alerts keep arriving through the notifications and webhooks you already set up in AyeWatch.

Example prompts:

- "List my AyeWatch monitors and tell me which are paused."
- "Create a daily AyeWatch monitor for news about solid-state batteries."
- "Pause my AyeWatch monitor about the iPhone launch."

## Requirements

- An AyeWatch account on a paid plan. API and MCP access are included with every paid plan.
- Active monitors use credits from your plan as checks run.
- Requests are limited to 120 per minute per API key by default.

## Connect

### Claude Code

```bash
claude mcp add --transport http ayewatch https://ayewatch.ai/api/mcp \
  --header "Authorization: Bearer aw_live_YOUR_API_KEY"
```

### Cursor

Add this to your MCP configuration (`~/.cursor/mcp.json`):

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
| `.claude-plugin/plugin.json`, `.mcp.json` | Claude plugin that points at the hosted server |
| `.cursor-plugin/plugin.json`, `mcp.json` | Cursor plugin that points at the hosted server |
| `assets/` | Icon and logo |

Nothing in this repository runs code on your machine. The plugin files only tell your client to connect to `https://ayewatch.ai/api/mcp`.

## Support and policies

- Support: <https://ayewatch.ai/contact>
- Privacy policy: <https://ayewatch.ai/privacy>
- Terms of service: <https://ayewatch.ai/terms>

## License

[MIT](LICENSE). The license covers the files in this repository, not the AyeWatch service.
