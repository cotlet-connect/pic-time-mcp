# Pic-Time MCP server for ChatGPT and Claude

Pic-Time has no public API or official MCP server, according to the Cotlet.dev integration page checked 9 November 2026. Cotlet.dev connects ChatGPT, Claude and other AI agents to your own signed-in Pic-Time account for galleries, visitors, favorites, store orders and clients. Sending a gallery invite asks for approval by default.

## What you can ask

> List unpublished galleries and their clients.

> Find gallery visitors with many favorites and no order yet.

> Report store orders by gallery or month.

> Find a client and their lifetime store spend.

> Prepare a gallery invite with a personal note and ask for my approval before sending.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, use `https://cotlet.dev/mcp/v1/connect` with OAuth, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings > Connectors > Add custom connector, URL: `https://cotlet.dev/mcp/v1/connect`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Pic-Time tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Or with Docker:

```bash
docker build -t pic-time-mcp .
docker run -i --rm -e COTLET_API_KEY pic-time-mcp
```

Tools: `pic_time_guide`, `pic_time_status`, `pic_time_open`, `pic_time_read`, `pic_time_click`, `pic_time_type`, `pic_time_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install.

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 pic_time_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 pic_time_agent.py status    # is Pic-Time ready in your Cotlet.dev account
python3 pic_time_agent.py guide     # Pic-Time screens and sign-in link
python3 pic_time_agent.py read      # open Pic-Time, read the page, close the browser
python3 pic_time_agent.py tools     # list Cotlet.dev tools
python3 pic_time_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Pic-Time users

**Does Pic-Time have an official MCP server?**

The live Cotlet.dev page reports no official MCP server, public API or official ChatGPT or Claude connector, checked 9 November 2026.

**What does the hosted integration cover?**

The page describes five operations: list_galleries, list_gallery_visitors, send_gallery_invite, list_orders and list_clients. This repository provides a local adapter to Cotlet.dev browser tools.

**Does sending a gallery invite require approval?**

The hosted send_gallery_invite operation asks for approval by default. A sent email cannot be recalled.

**Is this official?**

No. Cotlet.dev is independent and is not affiliated with, endorsed by or sponsored by Pic-Time.

## Integration reference

https://cotlet.dev/integrations/pic-time-api-mcp-server?utm_source=github

---

More: https://cotlet.dev/integrations/pic-time-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=pic-time

Start free: https://cotlet.dev/signup?app=pic-time&utm_source=github&utm_medium=readme&utm_campaign=pic-time
