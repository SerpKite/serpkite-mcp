# Install SerpKite MCP in Cline

SerpKite is a hosted, stateless Streamable HTTP MCP server. No server checkout, build or local runtime is needed.

1. Ask the user for a SerpKite API key from https://app.serpkite.com. Do not create an account or expose the key in logs.
2. Open Cline's MCP Servers panel and Configure MCP Servers. Merge this entry into the existing `mcpServers` object, preserving all other servers:

```json
{
  "mcpServers": {
    "serpkite": {
      "type": "streamableHttp",
      "url": "https://api.serpkite.com/v1/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Replace `YOUR_API_KEY` privately with the user's key. Keep configuration containing keys out of source control. Use the exact camel-case transport name `streamableHttp`; Cline defaults URL-only entries to SSE, which this endpoint does not use.

3. Refresh the server connection. Confirm `initialize` succeeds and fourteen tools appear: `search`, `news`, `maps`, `scholar`, `patents`, `shopping`, `images`, `videos`, `autocomplete`, `webpage`, `extract`, `map`, `crawl`, `crawl_result`.
4. Leave tool auto-approval disabled. Tool calls use the user's SerpKite credits; initialization and listing tools do not run searches. Explain the cost before a live search if testing requires one. Dedicated keys with monthly credit limits are recommended.

For authentication failures, confirm the key and the `Bearer ` prefix. For 405/SSE errors, confirm the exact transport value. OAuth is not currently supported.

Sources: https://serpkite.com/docs/mcp and https://github.com/cline/cline/blob/main/docs/mcp/mcp-overview.mdx.
