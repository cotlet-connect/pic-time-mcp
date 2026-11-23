# Pic-Time API integration and MCP server: connect Pic-Time to ChatGPT, Claude and any AI agent

Pic-Time API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in Pic-Time account: client galleries, visitors, favorites, print store orders, sales automations, expiry and archiving, on the same screens your login opens. No developer and no Pic-Time API key needed. Cotlet is independent and is not affiliated with Pic-Time.

**Does Pic-Time have an API or an MCP server?** Pic-Time has no public API (checked 23 Nov 2026); its integrations with HoneyBook, Studio Ninja and others are private partner links. Its Zapier app, live since 17 Apr 2025, has three triggers (New Gallery Visitor, New Order Placed, Main Client Gallery Invite Sent) and one action, Create Gallery. Pic-Time has no MCP server and no ChatGPT or Claude app; its own AI does face and keyword search, image filters and album design.

## What photographers ask their AI to do in Pic-Time

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Galleries that expire this month | "List galleries that expire this month and draft a 'keep your gallery online' email for each client. Send nothing yet." | Monthly |
| Which automation earns money | "Compare last quarter's orders by sales automation and tell me which ones earn and which to drop." | Quarterly |
| Print markups | "Work out my print markups from these lab costs so each product earns at least 60%." | When lab prices change |
| Check migrated galleries | "Check my galleries moved from Pixieset for download and expiry settings that changed, and list the ones to fix." | Once |
| Galleries in bulk | "Create 30 galleries for Saturday's mini sessions from this client list, with the same expiry and price list. Show me first." | Per event |
| Bulk settings change | "Turn on high-res downloads on all galleries from 2025. Show me the list before you save." | When you change a rule |
| Favorites to Lightroom | "List the photos Anna marked as favorites, as file names for Lightroom." | When a client finishes picking |
| Orders for your accountant | "Export this month's orders, payouts and client emails to a spreadsheet for my accountant." | Monthly |
| Visitors to your mailing list | "Add this month's gallery visitors who opted in to my Mailchimp list." | Your own endpoint, called from Mailchimp or Zapier |
| Back up before you leave | "Download every gallery as a backup before I cancel." | Once |

Your AI asks you before it sends, books or changes anything. Payments stay in your hands: Cotlet refuses payments.

## Your own Pic-Time API, built with Cotlet

Pic-Time gives you no public API; its Zapier app has one action, Create Gallery. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, QuickBooks, Xero, Mailchimp, Dubsado, Zapier, Make, n8n, your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `expiring-galleries` | galleries that expire soon, with client and date |
| `orders-for-period` | print and digital orders with client, product, amount and automation |
| `gallery-visitors` | gallery visitors with email and visit date |
| `client-favorites` | file names of the photos a client marked as favorites |
| `gallery-settings` | download, expiry and price list settings per gallery |
| `create-gallery` | creates a gallery with the settings you name (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/expiring-galleries \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=pic-time

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to Pic-Time once in your Cloud Browser, then add Cotlet to your AI.

### Claude

Settings → Connectors → Add custom connector, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev.

### ChatGPT

Open Plugins in ChatGPT, choose Create as a plugin, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev. No API key needed.

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

## Questions from Pic-Time photographers
**Does Pic-Time have an API?**

No public one (checked 23 Nov 2026). Its HoneyBook and Studio Ninja links are private partner integrations, and its Zapier app has one action, Create Gallery. With Cotlet you build your own endpoints in your signed-in Pic-Time account.

**Is there an official way to connect ChatGPT or Claude to Pic-Time?**

No. Pic-Time's own AI works inside Pic-Time. Cotlet is an unofficial MCP server for the ChatGPT and Claude apps and any MCP client.

**Can AI email clients before their Pic-Time galleries expire?**

Yes. Your AI lists galleries that expire this month and drafts an email for each client. Nothing goes out until you say yes.

**Can AI tell me which Pic-Time sales automations earn money?**

Yes. It compares orders by automation for the period you choose and shows which ones earn and which to drop.

**How do I get Pic-Time orders into QuickBooks or Xero?**

Your AI exports the month's orders, payouts and client emails to a spreadsheet for your accountant or a QuickBooks or Xero import.

**Can I change settings on all my old Pic-Time galleries at once?**

Yes. Your AI changes download or expiry settings on the galleries you name and shows you the list before it saves.

**How do I back up all my Pic-Time galleries?**

Your AI downloads each gallery your login can open into its own folder.

## Find it on

- [Pic-Time MCP server on Cotlet.dev](https://cotlet.dev/integrations/pic-time-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=pic-time&utm_content=find)
- [Pic-Time API with Cotlet](https://cotlet.dev/integrations/api/pic-time?utm_source=github&utm_medium=readme&utm_campaign=pic-time&utm_content=find)
- [Does Pic-Time have an API or MCP server?](https://cotlet.dev/api-mcp-status/pic-time?utm_source=github&utm_medium=readme&utm_campaign=pic-time&utm_content=find)
- [PyPI package pic-time-mcp](https://pypi.org/project/pic-time-mcp/)

## More photography apps on Cotlet

- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [CloudSpot MCP](https://github.com/cotlet-connect/cloudspot-mcp)
- [Studio Ninja MCP](https://github.com/cotlet-connect/studio-ninja-mcp)
