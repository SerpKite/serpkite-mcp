# SerpKite MCP

[![SerpKite MCP connector – tool definition quality and endpoint health on Glama](https://glama.ai/mcp/connectors/io.github.SerpKite/serpkite/badges/score.svg)](https://glama.ai/mcp/connectors/io.github.SerpKite/serpkite)

Give AI agents Google search and public web content through the [SerpKite](https://serpkite.com) remote Model Context Protocol server. Maintained by the SerpKite team.

This repository contains connection examples and registry metadata for the hosted service. The API implementation is proprietary; the documentation and configuration in this repository are MIT licensed.

## Connection

- **Endpoint:** `https://api.serpkite.com/v1/mcp`
- **Transport:** stateless Streamable HTTP
- **Authentication:** OAuth 2.1 sign-in (scope `mcp`), or `Authorization: Bearer <your-api-key>`
- **Protocols:** `2025-06-18`, `2025-03-26`, `2024-11-05`

Read the [pricing](https://serpkite.com/pricing) and [MCP documentation](https://serpkite.com/docs/mcp). Credits never expire; failed and empty searches are free. No SerpKite npm MCP package is required.

### OAuth

The server implements the [MCP authorization spec](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization). Clients that support MCP OAuth (Claude and ChatGPT connectors, Claude Code) need only the URL: they open a SerpKite sign-in page, where you approve the connection.

- Protected resource metadata: [`https://api.serpkite.com/.well-known/oauth-protected-resource/v1/mcp`](https://api.serpkite.com/.well-known/oauth-protected-resource/v1/mcp)
- Authorization server: `https://app-api.serpkite.com` ([metadata](https://app-api.serpkite.com/.well-known/oauth-authorization-server))
- Dynamic client registration or a Client ID Metadata Document, authorization code with PKCE (`S256`), scopes `mcp` and `offline_access`

An approved connection runs as an API key named after the app, such as `Claude (OAuth)`, on your **API keys** page. Give it a monthly credit limit, or revoke it to disconnect the app.

### API key

Clients without MCP OAuth send an API key from [app.serpkite.com](https://app.serpkite.com) in the `Authorization` header, as in the examples below. Set a dedicated key's monthly credit limit to control agent spending.

### Claude Code

```bash
claude mcp add --transport http serpkite https://api.serpkite.com/v1/mcp
```

Run `/mcp`, pick `serpkite` and choose **Authenticate** to sign in with OAuth. To use an API key instead, add `--header "Authorization: Bearer $SERPKITE_API_KEY"`.

### Cursor

Add to `~/.cursor/mcp.json` or your project's `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "serpkite": {
      "url": "https://api.serpkite.com/v1/mcp",
      "headers": { "Authorization": "Bearer ${env:SERPKITE_API_KEY}" }
    }
  }
}
```

### Cursor plugin

This repository is also a Cursor plugin (`.cursor-plugin/plugin.json`, `mcp.json`, a web-research skill and a rule). Set `SERPKITE_API_KEY` in your environment before starting Cursor; the plugin reads it through `${env:SERPKITE_API_KEY}` and never stores the key.

### VS Code

Add to `.vscode/mcp.json`. The password input keeps the API key out of the committed file:

```json
{
  "inputs": [
    { "type": "promptString", "id": "serpkite-key", "description": "SerpKite API key", "password": true }
  ],
  "servers": {
    "serpkite": {
      "type": "http",
      "url": "https://api.serpkite.com/v1/mcp",
      "headers": { "Authorization": "Bearer ${input:serpkite-key}" }
    }
  }
}
```

### Cline

Follow [llms-install.md](llms-install.md) for Cline's remote connection configuration.

### Claude Desktop and stdio clients

With Node.js installed, use the third-party [mcp-remote](https://www.npmjs.com/package/mcp-remote) bridge in your client's MCP configuration:

```json
{
  "mcpServers": {
    "serpkite": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://api.serpkite.com/v1/mcp", "--header", "Authorization: Bearer ${SERPKITE_API_KEY}"],
      "env": { "SERPKITE_API_KEY": "YOUR_API_KEY" }
    }
  }
}
```

Keep API keys out of source control.

### ChatGPT

With developer mode enabled, create a connector with the URL `https://api.serpkite.com/v1/mcp` and OAuth authentication. ChatGPT registers itself and sends you to the SerpKite approval page.

## Tools

| Tool | Purpose |
| --- | --- |
| `search` | Google web results, answers, knowledge graph and related questions |
| `news` | Google News articles |
| `maps` | Places, ratings, addresses and coordinates |
| `scholar` | Academic papers, citations and PDF links |
| `patents` | Patent search |
| `shopping` | Products, prices and merchants |
| `images` | Image search |
| `videos` | Video search |
| `autocomplete` | Query suggestions |
| `webpage` | Public HTML or PDF to Markdown |
| `extract` | Read multiple URLs, optionally select passages by query |
| `map` | Discover a site's URLs |
| `crawl` | Start a crawl of public site pages |
| `crawl_result` | Read crawl status and results |

Tool results are LLM-ready Markdown. Pricing follows the matching REST endpoints. Only public, logged-out pages are fetched. See [tool inputs and costs](https://serpkite.com/docs/mcp#tools).

## Verify the connection

These metadata requests do not run searches:

```bash
curl https://api.serpkite.com/v1/mcp \
  -H "Authorization: Bearer $SERPKITE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-06-18","capabilities":{},"clientInfo":{"name":"curl","version":"1.0"}}}'

curl https://api.serpkite.com/v1/mcp \
  -H "Authorization: Bearer $SERPKITE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/list"}'
```

The remote endpoint and all fourteen tool names were verified on 2026-10-05.

## Support

[Documentation](https://serpkite.com/docs/mcp) · [Status](https://status.serpkite.com) · [support@serpkite.com](mailto:support@serpkite.com)

Google is a trademark of Google LLC. SerpKite is not affiliated with Google.
