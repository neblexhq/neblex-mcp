# Neblex MCP Server

Remote MCP server for [Neblex Integration Fabric](https://www.neblex.com/integration-fabric), an integration platform (iPaaS). Connect an AI client such as Claude Code, Codex, Cursor or VS Code and build, test and run integrations, workflows, forms, apps, data tables and AI agents in your Neblex workspace, under the same permissions as your own account.

This repository documents the hosted server. There is nothing to install: the server runs at the endpoint below.

**Endpoint:** `https://if.neblex.com/mcp` (Streamable HTTP, OAuth)

**Official MCP Registry:** `io.github.neblexhq/neblex`

## Connect

### Claude Code

```bash
claude mcp add --transport http neblex https://if.neblex.com/mcp
```

### Cursor

Install the [Neblex plugin for Cursor](https://github.com/neblexhq/neblex-cursor-plugin), which sets up this server for you. Or add it yourself to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "neblex": {
      "url": "https://if.neblex.com/mcp"
    }
  }
}
```

### Codex

Add to your Codex config (`~/.codex/config.toml`):

```toml
[mcp_servers.neblex]
url = "https://if.neblex.com/mcp"
auth = "oauth"
default_tools_approval_mode = "writes"
```

### VS Code

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "neblex": {
      "type": "http",
      "url": "https://if.neblex.com/mcp"
    }
  }
}
```

### Other clients

Any client that supports remote MCP servers over Streamable HTTP can use the endpoint URL directly. No API key goes in your configuration: on first use the client opens the Neblex sign-in in your browser.

## Sign-in and permissions

- OAuth 2.1 with step-up and an enrolled second factor for interactive building. Clients register themselves through dynamic client registration.
- OAuth discovery:
  - `https://if.neblex.com/.well-known/oauth-protected-resource`
  - `https://if.neblex.com/.well-known/oauth-authorization-server`
- On the consent screen you choose the scopes the client gets:
  - `operate`: run and observe assigned flows
  - `author`: build flows, apps, forms, data tables, connections and agents
  - `publish`: promote to protected environments (with browser approval)
- The client acts under your own permissions; it can't do anything your account can't.
- Secrets are never returned to the client. When a connection needs credentials, the server creates it without secrets and returns a link where a person enters them in the browser.
- Publishing into a protected environment waits for a person's approval.
- A static API consumer key used with MCP can only inspect and run the flows it is granted; it can never create or publish.

## What the client can do

| Area | Examples |
| --- | --- |
| Discover | Search 300+ connectors, their operations and node types |
| Build | Create and edit flows, forms, internal apps, data tables, data models and AI agents |
| Check | Validate before saving and test before publishing |
| Run | Run granted flows and read results, run history, run events and flow health |
| Connect | Create a connection without secrets and get the link to finish it |
| Version | List versions and restore earlier ones |
| Publish | Take flows, forms, apps and agents live or offline, with approval where required |

## Example prompts

- "Build a flow that syncs new HubSpot contacts to Snowflake and removes duplicates first."
- "Why did last night's order sync fail? Show me the step where it stopped."
- "Draft an IT service desk agent that answers from our SOPs and asks before it opens a ticket."
- "Connect our Postgres database and send me the link to enter the password."

Worked examples, each built and run through this server, with the flow it produced and the result: [examples](examples).

## About Neblex Integration Fabric

- **Data quality in every flow:** deduplication (exact, Jaro-Winkler, edit distance and Soundex), validation and a quarantine for bad records run as steps inside the flow.
- **Cloud or on-premises:** flows and AI agents run in the cloud or on a single native on-premises runtime inside your network, so records from local systems stay local.
- **Replay from the failed step:** every run keeps a step-level history, and failed runs aren't billed.
- **300+ connectors**, plus HTTP, SOAP, OData, SFTP and EDI (X12, EDIFACT) over AS2.
- **AI agents** use your own model provider, and any tool can require a person's approval.

There's a free Starter plan with the full platform and 1,000 executions a month, and every plan has unlimited users: [pricing](https://www.neblex.com/integration-fabric/pricing).

## Links

- [MCP documentation](https://www.neblex.com/integration-fabric/docs/guide/mcp)
- Connected clients are listed, and can be revoked, in Neblex under API Consumers.
- [Neblex Integration Fabric](https://www.neblex.com/integration-fabric)
- [Glama connector](https://glama.ai/mcp/connectors/com.neblex.if/neblex)
- [Sign up](https://www.neblex.com/signup)

## Support

Questions or problems: open an issue in this repository, or ask Matt, the in-product assistant, inside Neblex.
