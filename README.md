# Sector Pulse MCP server

Live US sector rotation for agents. Sector Pulse tracks 30 US equity sector baskets (AI data center, cloud, BTC miners, nuclear, rare earth, and 25 more), ranks them by average move every session, and records the close ranks daily since July 21, 2026. The record is never backfilled, and the basket rosters are versioned, so a backtest knows exactly what a basket held on any date.

This is a hosted (remote) server over Streamable HTTP. There is nothing to install.

- Endpoint: `https://sector-pulse.app/api/mcp`
- Docs: https://sector-pulse.app/api-docs
- OpenAPI: https://sector-pulse.app/openapi.json
- Official registry: `io.github.christianhonap7-sys/sector-pulse`

![Sector Pulse MCP](mcp-server.png)

## Tools

| Tool | Key | What it returns |
|---|---|---|
| `get_sectors` | required (free key works) | The live board: all 30 baskets with average move, median, breadth, and three rank bases (session, close basis, previous close). |
| `get_roster` | required | The ticker lists behind each basket, versioned and immutable. Pass `version` for an archived roster. |
| `get_history` | required | The daily close record for a date (rank, move, breadth per sector), or 5-minute intraday rows with `intraday: true`. Daily from 2026-07-21, intraday from 2026-08-19. |

Aggregates only. No quotes, no per-ticker prices.

## Setup

Claude Desktop, Cursor, and other Streamable HTTP clients:

```json
{
  "mcpServers": {
    "sector-pulse": {
      "url": "https://sector-pulse.app/api/mcp",
      "headers": { "x-api-key": "SPK-YOURKEYHERE" }
    }
  }
}
```

Every tool needs a key. A free key (100 calls a day, every tool) is emailed from https://sector-pulse.app/api-docs#free-key; ChatGPT and claude.ai connectors take the key in the address instead: `https://sector-pulse.app/api/mcp?key=SPK-YOURKEYHERE`.

Clients that only speak stdio:

```bash
npx -y mcp-remote https://sector-pulse.app/api/mcp --header "x-api-key: SPK-YOURKEYHERE"
```

## Keys

The same key works for the REST API and the MCP server. Three ways to get one, all emailed within a minute:

- **Free key:** 100 calls a day on every tool and endpoint, no card. Enter your email at https://sector-pulse.app/api-docs#free-key.

- **Pay as you go:** $10 for 2,000 calls, $25 for 6,000. One call is one credit on every tool and endpoint; credits never expire. One email at 10% left; past zero, calls return 402 with a top-up link. API only.
- **Founding tier:** $39 a month, Sector Pulse Pro membership included, 10 founding spots, price locked for as long as you stay.

https://sector-pulse.app/pricing · top up an existing key at https://sector-pulse.app/topup

## Rate limits

10 requests a minute per key across REST and MCP. Free keys: 100 calls a day.

## Terms

Educational tool, not financial advice. Derived analytics computed by Sector Pulse, a product of Apex Infra LLC. Keys are for your own applications; redistributing the feed is not permitted. Support: support@apexinfrallc.com

## License

The documentation and configuration in this repository are MIT licensed. The hosted service is governed by the terms on the [API docs](https://sector-pulse.app/api-docs).
