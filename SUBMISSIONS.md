# Where to submit AyeWatch

Every listing points back to this repository and to `https://ayewatch.ai/api/mcp`. Submission forms change often, so check each site's current form before you paste.

## Copy-paste fields

| Field | Value |
| --- | --- |
| Name | AyeWatch |
| Slug / server name | `ayewatch` (registry: `io.github.SRX9/ayewatch`) |
| Tagline (94 chars) | Manage AyeWatch monitors for topics and web pages, and the webhook that receives their alerts. |
| Short description | Monitor any topic or web page |
| Category | Productivity (or Monitoring, News, or Research where offered) |
| Keywords | monitoring, alerts, web page changes, news, research, webhooks, mcp |
| MCP endpoint | `https://ayewatch.ai/api/mcp` |
| Transport | Streamable HTTP, stateless |
| Auth | API key sent as `Authorization: Bearer aw_live_...`. OAuth works only for apps AyeWatch has registered (ChatGPT). |
| Get a key | https://ayewatch.ai/settings/api |
| Website | https://ayewatch.ai |
| Docs | https://ayewatch.ai/documentation/mcp |
| Pricing | https://ayewatch.ai/pricing (API and MCP access need a paid plan) |
| Privacy | https://ayewatch.ai/privacy |
| Terms | https://ayewatch.ai/terms |
| Support | https://ayewatch.ai/contact |
| Repository | https://github.com/SRX9/ayewatch-mcp |
| Logo | `assets/logo.png` (512×512), `assets/logo-400.png` (400×400), `assets/icon.png` (192×192) |

Long description:

> AyeWatch monitors topics and web pages and alerts you when something changes or matches what you care about. From your AI assistant you can list your monitors, create a monitor for a topic or for one or more web pages, change its schedule, pause or resume it, and delete it. With an API key you can also set the webhook that receives every alert. Active monitors use credits from your AyeWatch plan as checks run.

## 1. Official MCP Registry (done)

Published as `io.github.SRX9/ayewatch` version 1.0.0. GitHub's MCP registry, VS Code's MCP gallery, PulseMCP, and other directories read from it.

To update the listing, raise `version` in [`server.json`](server.json), then run `mcp-publisher login github` and `mcp-publisher publish` from this folder. A published version can't be changed.

## 2. Claude

- **Claude Code (works today):** this repository is a plugin marketplace. Users run `claude plugin marketplace add SRX9/ayewatch-mcp`, then `claude plugin install ayewatch@ayewatch`. Claude Code asks for the API key.
- **Anthropic's plugin directory:** open https://claude.ai/directory/manage, select **Submit new**, then **Plugin bundle**, and enter this repository with the plugin at the root. Select **Validate**, fix anything marked **Blocking**, then submit. The listing works in Claude Code. claude.ai chat and Cowork can't send a user's API key, so they need OAuth that Claude can register with.
- **Anthropic's connectors directory:** waits for the same OAuth support.

## 3. Cursor Marketplace

- Files: [`.cursor-plugin/plugin.json`](.cursor-plugin/plugin.json), [`mcp.json`](mcp.json), `assets/logo.png`.
- Submit the repository URL at https://cursor.com/marketplace/publish. Cursor reviews it by hand.
- The plugin asks for `AYEWATCH_API_KEY` as a plugin variable. Cursor sets plugin variables in its dashboard, and on a team plan an admin sets one value for the whole team, so everyone on that team would use the admin's AyeWatch account. The README's manual setup gives each person their own key.
- Also list it on https://cursor.directory as an MCP server, with the manual setup from the README.

## 4. OpenAI plugin directory (ChatGPT and Codex)

- The package is in the AyeWatch app repository at `plugins/ayewatch/`, not here. It signs in with OAuth through the ChatGPT client that AyeWatch registered, so it lists only the monitor tools. The webhook tools need an API key.
- Upload a ZIP of that folder's contents at https://platform.openai.com/plugins. You need an organization owner role and individual or business verification.
- Domain check: the portal shows a challenge token. Set it as `OPENAI_APPS_CHALLENGE_TOKEN` in the web app's environment and redeploy. AyeWatch then serves it at `https://ayewatch.ai/.well-known/openai-apps-challenge`.
- Reviewers sign in to a test account. Give them a paid account that has a "Bitcoin price" monitor, because one of the package's test cases asks for it.

## 5. Glama

Submit the repository at https://glama.ai/mcp/servers. [`glama.json`](glama.json) claims the listing for the GitHub user `SRX9`.

## 6. Smithery

Add a hosted server at https://smithery.ai/new with the endpoint URL. Smithery lists tools by connecting to the server, so give it an API key from a test account.

## 7. Directories (form, issue, or pull request)

- **Cline MCP Marketplace:** open an issue at https://github.com/cline/mcp-marketplace with the repository URL and `assets/logo-400.png`.
- **awesome-mcp-servers:** open a pull request at https://github.com/punkpeye/awesome-mcp-servers adding one line under the closest category, such as "Search & Data Extraction". Mark it ☁️ because the server is hosted.
- **PulseMCP:** https://www.pulsemcp.com/submit. It also syncs from the official registry.
- **mcp.so:** submit the repository URL.
- **LobeHub MCP marketplace:** submit the repository URL at https://lobehub.com/mcp.

## Keeping this repository current

When tools change, update the tool table in [`README.md`](README.md) and raise `version` in `server.json` and both plugin manifests. Publish `server.json` again, and resubmit wherever a directory keeps its own copy.
