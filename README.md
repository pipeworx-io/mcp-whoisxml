# mcp-whoisxml

WhoisXML MCP — wraps WhoisXML API (whoisxmlapi.com)

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1576+ live data sources.

## Tools

| Tool | Description |
|------|-------------|
| `whoisxml_whois` | Current WHOIS for <domain> — registrar, creation/expiry dates, registrant org, name servers, and status. Example: whoisxml_whois({ domain: "google.com", _apiKey: "your-key" }) |
| `whoisxml_whois_history` | Historical WHOIS records for <domain> — past registrars, registrants, and creation/expiry dates over time. Example: whoisxml_whois_history({ domain: "google.com", _apiKey: "your-key" }) |
| `whoisxml_reverse_whois` | Find domains registered by <email/name/org> — reverse WHOIS lookup returning every domain whose current WHOIS record contains the search term. Example: whoisxml_reverse_whois({ term: "admin@google.com", _apiKey: "your-key" }) |
| `whoisxml_dns` | DNS records for <domain> — A, AAAA, MX, NS, TXT, and other records with their values and TTLs. Example: whoisxml_dns({ domain: "google.com", _apiKey: "your-key" }) |

## Quick Start

Add to your MCP client (Claude Desktop, Cursor, Windsurf, etc.):

```json
{
  "mcpServers": {
    "whoisxml": {
      "url": "https://gateway.pipeworx.io/whoisxml/mcp"
    }
  }
}
```

### What this endpoint actually serves

`tools/list` at `https://gateway.pipeworx.io/whoisxml/mcp` returns the tools in the table
above **plus the shared Pipeworx meta-tools** — `ask_pipeworx`,
`discover_tools`, `search_within`, `remember`/`recall` and the rest of the
gateway-wide set. So the tool count you see is larger than this table: a
single-pack endpoint currently lists roughly 30 shared tools alongside the
pack's own. The connection's `initialize` response states its exact scope, and
is the authoritative answer for a given day.

This is deliberate, not multiplexing by accident. The meta-tools are what let a
scoped connection answer a question this pack does not cover — via
`ask_pipeworx`, which routes across the whole catalog — without you adding a
second MCP server. There is currently no way to mount a pack endpoint without
them; if the extra schemas cost you more context than the routing is worth,
connect to the full gateway once rather than to several pack endpoints.

Or connect to the full Pipeworx gateway to get every pack's tools listed
directly, instead of just this one's:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

Both URLs reach the same gateway and the same 1576+ data sources. The
only difference is which pack's tools are listed **directly**; `ask_pipeworx`
reaches all of them from either one.

## Standalone (no gateway account)

This package also runs as a local stdio MCP server — no Pipeworx account, no
gateway round-trip:

```json
{
  "mcpServers": {
    "whoisxml": {
      "command": "npx",
      "args": ["-y", "@pipeworx/mcp-whoisxml"]
    }
  }
}
```

Or run it directly to confirm it starts:

```bash
npx -y @pipeworx/mcp-whoisxml
```

It speaks MCP over stdin/stdout and answers `initialize`/`tools/list`/`tools/call`
for **only** this pack's tools — none of the shared meta-tools the gateway
connection above adds. Same source, same tools, no ask_pipeworx routing.

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English —
this works on the pack endpoint above as well as on the full gateway:

```
ask_pipeworx({ question: "your question about Whoisxml data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT
