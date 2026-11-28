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

## What you can ask your AI to do in Pic-Time

These are real jobs Pic-Time users asked about. Cotlet lets ChatGPT, Claude or any MCP client do them in your own signed-in Pic-Time account.

### CRM and booking links

#### How do I connect Pic-Time to HoneyBook so a gallery is created when a client books and I can pull images back into the client file?

Partly. Pic-Time's help centre lists a HoneyBook integration. With Cotlet, your agent reads new booked clients in HoneyBook and creates the gallery in Pic-Time, both signed in in your Cloud Browser. Nothing is created until you say yes.

Ask your agent: "Take the clients who booked in HoneyBook this week and create a Pic-Time gallery for each with the client name. Show me the list first."

As an API endpoint: Yes. Save the booking-to-gallery step as an endpoint in Cotlet and call it from n8n, Make or Zapier whenever HoneyBook has a new booked client.

#### How do I connect Pic-Time to Studio Ninja so galleries link to each job and Pic-Time print orders land in my income records?

Partly. Pic-Time's help centre has a Studio Ninja page, and Studio Ninja says it copies Pic-Time orders to Xero or QuickBooks. With Cotlet, your agent lists Pic-Time orders missing from your income records and adds them. Nothing is added until you say yes.

Ask your agent: "List this month's Pic-Time print orders that are not in my Studio Ninja income records and show client, date and total."

As an API endpoint: Yes. Save the missing-orders check as an endpoint in Cotlet and call it every Monday from n8n, Make or Zapier.

#### Can I connect Dubsado to Pic-Time so a gallery is created when a contract is signed?

Yes. Pic-Time has no Dubsado page, only its Zapier app with a Create Gallery action. With Cotlet, your agent reads newly signed contracts in Dubsado and creates the gallery in Pic-Time, both signed in in your Cloud Browser. Nothing is created until you say yes.

Ask your agent: "List Dubsado contracts signed this week and create a Pic-Time gallery for each client. Show me the list before you create anything."

As an API endpoint: Yes. Save the contract-to-gallery step as an endpoint in Cotlet and call it from n8n, Make or Zapier when Dubsado reports a signed contract.

#### How do I get a Pic-Time gallery created from a booking calendar (Calendly or Google Calendar) without a CRM?

Yes. Pic-Time's Zapier app can create a gallery. With Cotlet, your agent reads new bookings in Calendly or Google Calendar and creates a Pic-Time gallery for each, signed in in the same Cloud Browser. Nothing is created until you say yes.

Ask your agent: "Read this week's new Google Calendar bookings tagged shoot and create a Pic-Time gallery for each client. Show me the list first."

As an API endpoint: Yes. Save the booking read as an endpoint in Cotlet and call it every morning from n8n, Make or Zapier to create the day's galleries.

### Orders and accounting

#### How do I connect Pic-Time to Zapier so every new print order is added to a Google Sheet?

Yes. Pic-Time's Zapier app has a New Order Placed trigger. With Cotlet, your agent can also read the Orders screen and add each order as a row in your Google Sheet, without building a Zap. Nothing is added until you say yes.

Ask your agent: "Add this month's Pic-Time orders to my Google Sheet, one row each with client, date, items and total."

As an API endpoint: Yes. Save the order read as an endpoint in Cotlet and call it daily from n8n, Make or Zapier to add new orders to the sheet.

#### How do I get Pic-Time orders into QuickBooks or Xero without typing each one by hand?

Yes. Pic-Time lets you download orders as CSV, but has no QuickBooks or Xero link of its own. With Cotlet, your agent reads the month's orders and enters them in QuickBooks or Xero, signed in in the same Cloud Browser. Nothing is entered until you say yes.

Ask your agent: "Read this month's Pic-Time orders and enter each as a sales record in QuickBooks with date, client and total. Show me the list first."

As an API endpoint: Yes. Save the order read as an endpoint in Cotlet that returns JSON, and pass it to your accounting tool from n8n or Make each month.

#### How do I export all my Pic-Time orders and client emails into a spreadsheet every month for my accountant?

Yes. Pic-Time downloads orders as CSV and gallery visitor emails with Download List, one gallery at a time. With Cotlet, your agent gathers the month's orders and the visitor emails into one spreadsheet for your accountant.

Ask your agent: "Put last month's Pic-Time orders and the visitor emails of each gallery into one spreadsheet with client, date, total and email."

As an API endpoint: Yes. Save the monthly read as an endpoint in Cotlet and call it on the first of each month from n8n, Make or a script.

#### Can AI look at my Pic-Time orders and tell me which sales automation actually earns money and which one to drop?

Partly. Pic-Time sets up Sales Automations under Shop & Automations. With Cotlet, your agent reads the orders and the Sales Report for a period and sets them against each automation's dates. The agent says which orders it could match.

Ask your agent: "Read my Pic-Time orders for the last 90 days and my sales automations, and show which automation brought which sales."

As an API endpoint: Yes. Save the comparison as an endpoint in Cotlet and call it from n8n, Make or a script at the end of each quarter.

#### Can AI work out my Pic-Time print markups from my lab costs, so I stop guessing 150% or 500%?

Yes. Pic-Time downloads your price list from Shop & Automation and shows production costs in Billing History. With Cotlet, your agent reads both and works out the markup per product, then proposes a price. Nothing in your shop changes.

Ask your agent: "Read my Pic-Time price list and my lab production costs and show the markup for each product with a suggested price at 3x cost."

As an API endpoint: Yes. Save the markup table as an endpoint in Cotlet and call it from n8n, Make or a script whenever your lab changes prices.

### Galleries and expiry

#### Can ChatGPT or Claude list the Pic-Time galleries that expire this month and write a keep-your-gallery-online email for each client?

Yes. Pic-Time has a Gallery Expiration Automation that sends its own reminders. With Cotlet, your agent lists the galleries that expire this month from Gallery Setup and drafts a personal email for each client. Nothing is sent until you say yes.

Ask your agent: "List my Pic-Time galleries that expire this month with client and date, and draft a short keep-your-gallery-online email for each."

As an API endpoint: Yes. Save the expiry list as an endpoint in Cotlet and call it on the first of each month from n8n, Make or Zapier.

#### Can an AI agent check my migrated Pic-Time galleries for wrong download or expiry settings?

Yes. With Cotlet, your agent opens each gallery's setup in your signed-in Pic-Time account and lists the ones whose download or expiry settings differ from what you tell it. Nothing is changed.

Ask your agent: "Check every Pic-Time gallery migrated this year and list the ones where downloads are off or the expiry date is missing."

As an API endpoint: Yes. Save the check as an endpoint in Cotlet and call it from n8n, Make or a script after each batch of migrated galleries.

#### How do I create 30 Pic-Time galleries at once with the same settings, expiry and price list, from a list of client names?

Yes. Pic-Time's Zapier app can create a gallery one at a time. With Cotlet, your agent creates each gallery from your list in your signed-in Pic-Time account and sets the same expiry and price list. Nothing is created until you say yes.

Ask your agent: "Create a Pic-Time gallery for each client name in this list with the same expiry and price list as my last gallery. Show me the first three first."

As an API endpoint: Yes. Save the gallery step as an endpoint in Cotlet that takes a client name, and call it for each row of a sheet from n8n or Make.

#### How do I change expiry or download settings on all my old Pic-Time galleries in one go?

Yes. With Cotlet, your agent opens each gallery you name in Pic-Time, sets the new expiry or download option and shows you the list first. Nothing changes until you say yes.

Ask your agent: "Set downloads to on and the expiry to December 2027 for every Pic-Time gallery from 2023. Show me the list before you save."

#### How do I automatically reopen an archived Pic-Time gallery for a past client and send them an invoice for the reopening fee?

Partly. Pic-Time has an Unarchive step for older galleries. With Cotlet, your agent unarchives the gallery you name and drafts the invoice for your reopening fee in QuickBooks or your invoicing tool. Nothing changes or is sent until you say yes.

Ask your agent: "Unarchive the Hartley wedding gallery in Pic-Time and draft a reopening fee invoice for Anna in QuickBooks. Show me both first."

As an API endpoint: Yes. Save the reopen step as an endpoint in Cotlet that takes a gallery name, and call it from n8n, Make or Zapier when a client pays.

#### How do I tag my Pic-Time galleries by venue, planner and photographer and search them later?

Partly. Pic-Time's help centre shows no tag screen for galleries. With Cotlet, your agent reads each gallery's name and details and builds a searchable table of venue, planner and photographer in Google Sheets.

Ask your agent: "List all my Pic-Time galleries and build a Google Sheet with columns for client, date, venue, planner and photographer from the gallery names."

#### How do I download every gallery from Pic-Time to back it up before I leave?

Partly. Pic-Time has no export-all screen, only a download for each gallery. With Cotlet, your agent goes through your galleries and starts each download in turn. Very large galleries may not fit, so try a few first.

Ask your agent: "Download the Pic-Time galleries from 2024, one after another, and give me a list of which ones finished. Start with three."

### Client emails and downloads

#### How do I send my Pic-Time visitor emails to Mailchimp or another mailing list?

Yes. Pic-Time's Zapier app has a New Gallery Visitor trigger, and each gallery's visitor emails download as CSV. With Cotlet, your agent reads the visitor emails and adds them to your Mailchimp audience, signed in in the same Cloud Browser. Nothing is added until you say yes.

Ask your agent: "Take the visitor emails of my last five Pic-Time galleries and add them to my Mailchimp audience. Show me the list first."

As an API endpoint: Yes. Save the visitor read as an endpoint in Cotlet and call it weekly from n8n, Make or Zapier to update your mailing list.

#### Can AI write my Pic-Time gallery invitation and reminder emails and schedule them without sending the wrong one to the wrong person?

Yes. Pic-Time lets you schedule when emails are sent. With Cotlet, your agent writes each invitation or reminder, matches it to the right client and gallery and shows you the pairs. Nothing is scheduled or sent until you say yes.

Ask your agent: "Write the invitation email for each Pic-Time gallery ready this week and show me a table of gallery, client and email before anything is scheduled."

As an API endpoint: Yes. Save the gallery and client pairing as an endpoint in Cotlet and call it from n8n, Make or Zapier when a gallery is ready.

#### Can AI answer my clients' questions about downloading Pic-Time photos to their phone or Google Photos?

Partly. Pic-Time has help pages on client downloads and on saving to Google Photos. With Cotlet, your agent reads the client's email in your inbox and drafts a reply with those steps for that gallery. Nothing is sent until you say yes.

Ask your agent: "Read the latest client email about downloading photos and draft a reply with the steps to save the gallery to a phone or Google Photos."

### Lightroom and selections

#### How do I make Lightroom, Imagen and Pic-Time work as one flow, from cull to published gallery and client selections back in my catalog?

Partly. The Lightroom Classic plug-in and Imagen run on your computer, which Cotlet cannot control. With Cotlet, your agent works in Pic-Time's web screens and reads which galleries are published and what clients selected, so you see where each job stands.

Ask your agent: "List my Pic-Time galleries from this month with whether they are published and how many photos each client has selected."

As an API endpoint: Yes. Save the status list as an endpoint in Cotlet and call it every evening from n8n, Make or a script.

#### Can AI pick highlight photos and sort a Pic-Time gallery into scenes, and keep that order when I republish from Lightroom?

Partly. Pic-Time has its own AI Photo Filter and AI search for clients. With Cotlet, your agent can arrange a gallery's photos by the scenes you name. Choosing the best shots by eye is your call, and a Lightroom republish happens on your computer. Nothing changes until you say yes.

Ask your agent: "Sort the Hartley wedding gallery in Pic-Time into the scenes Getting ready, Ceremony and Reception from the photo times. Show me the plan first."

#### How do I send my clients' Pic-Time favourites and selections straight into Lightroom as a list?

Yes. Pic-Time's Lightroom plug-in update of October 2026 syncs selections as CSV. With Cotlet, your agent can also read a client's favourites in the gallery and give you the file names as a list for a Lightroom collection.

Ask your agent: "List the file names of the photos the Hartleys marked as favourites in their Pic-Time gallery, one per line."

As an API endpoint: Yes. Save the favourites read as an endpoint in Cotlet that takes a client name and returns file names, and call it from n8n or a script.

### Website and portfolio

#### How do I put my Pic-Time galleries and portfolio on my own WordPress or Squarespace site without a slow embed?

Partly. Pic-Time has a Portfolio Page embed and a Pic-Time Connect WordPress plugin. With Cotlet, your agent reads your public galleries in Pic-Time and adds the pages or links in your WordPress or Squarespace admin. Page speed is set by the embed. Nothing is published until you say yes.

Ask your agent: "List my public Pic-Time galleries and add a link page for each to my WordPress site as a draft. Show me the list first."

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

## More photography apps on Cotlet

- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [CloudSpot MCP](https://github.com/cotlet-connect/cloudspot-mcp)
- [Studio Ninja MCP](https://github.com/cotlet-connect/studio-ninja-mcp)
