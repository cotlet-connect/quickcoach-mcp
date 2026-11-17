# QuickCoach MCP server through Cotlet.dev

Connect QuickCoach to ChatGPT, Claude, Codex and Claude Code through Cotlet.dev. Read your coaching roster, prepare client plans and review check-ins in your own signed-in coach account.

This is an unofficial integration, not affiliated with QuickCoach. Cotlet.dev hosts the connection through your Cloud Browser. This repository provides an optional Python stdio MCP bridge.

## Connect your account

1. [Request access to QuickCoach](https://cotlet.dev/signup?app=quickcoach).
2. Open https://app.quickcoach.fit/ in your Cotlet.dev Cloud Browser. Enter your coach email and type the code QuickCoach sends you, then enable QuickCoach for agents.
3. Connect your agent. Refresh the connector if you enabled QuickCoach after connecting.

### ChatGPT

Open Settings > Apps & Connectors > Create. Enter `https://cotlet.dev/mcp/v1/connect` as the MCP server URL and choose OAuth.

### Claude

Open Settings > Connectors > Add custom connector and enter `https://cotlet.dev/mcp/v1/connect`.

### Codex

Create a Cotlet.dev API key in your console and set `COTLET_API_KEY` in your environment. Add:

```toml
[mcp_servers.cotlet]
url = "https://cotlet.dev/mcp/v1"
bearer_token_env_var = "COTLET_API_KEY"
```

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

## Coaching workflows

Examples from the [live QuickCoach integration reference](https://cotlet.dev/integrations/quickcoach-api-mcp-server):

- Find clients with few upcoming plans and prepare their next drafts.
- Read client feedback and workout history before proposing a plan change.
- Publish and reorder plans after reviewing the changes.
- Set client macros and review food logs when your QuickCoach plan includes them.
- Prepare a push notification for your approval.
- Review Client Billing and prepare a package payment link.

The live reference lists 26 hosted capabilities: 6 read and 20 write. They are available through `quickcoach_read` and `quickcoach_write`. Writes ask for confirmation by default, configurable per tool. Review client names, plan changes, notification recipients and payment amounts before approving.

QuickCoach clients work through an ordered plan queue rather than a calendar. Edits to published plans reach clients immediately. QuickCoach has no message inbox; replies use plans or push notifications. Pro features require the corresponding QuickCoach plan. Your Stripe connection, payouts, QuickCoach subscription and sign-in email remain outside these tools.

## Optional local stdio bridge

Python 3.10 or newer is required, with no runtime dependencies. Clone this repository and configure a client that supports stdio:

```json
{"mcpServers":{"quickcoach":{"command":"python3","args":["/absolute/path/to/quickcoach-mcp/quickcoach_agent.py"],"env":{"COTLET_API_KEY":"your-cotlet-key"}}}}
```

The local bridge lists seven browser helpers: `quickcoach_guide`, `quickcoach_status`, `quickcoach_open`, `quickcoach_read`, `quickcoach_click`, `quickcoach_type` and `quickcoach_close`. They forward to Cotlet.dev's shared browser tools. The local helper names are separate from the hosted workflow groups and their 26 capabilities. Call the guide first and close the browser when finished.

Discovery works without a key. Executing a tool requires your Cotlet.dev API key and connected account.

```bash
python3 quickcoach_agent.py connect
python3 quickcoach_agent.py guide
python3 quickcoach_agent.py status
python3 quickcoach_agent.py serve
```

You can also build the included Dockerfile and pass `COTLET_API_KEY` as an environment variable.

## Reference and access

- [QuickCoach connection guide and operation reference](https://cotlet.dev/integrations/quickcoach-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=quickcoach)
- [Request access](https://cotlet.dev/signup?app=quickcoach)
- [Cotlet.dev console](https://cotlet.dev/app)

Licensed under Elastic License 2.0. See LICENSE.
