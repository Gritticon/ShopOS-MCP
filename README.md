# Shop OS MCP — connect your AI assistant

Use Shop OS from **ChatGPT**, **Claude**, or **Gemini** (where available).  
You never copy a token. You paste a **connector URL** and click **Allow**.

## What you need

- A Shop OS merchant account
- The MCP connector URL from Shop OS (HTTPS) — shown in **Settings → AI connector** after setup, or given by your operator
- ChatGPT or Claude with custom connectors enabled

## ChatGPT

1. Open **Settings → Connected apps** (or **Custom connectors**)
2. Add a connector and paste your Shop OS MCP URL
3. When redirected to Shop OS, sign in if asked and click **Allow**
4. Ask the assistant something like: “List my catalog” or “Show today’s orders”

## Claude

1. Add a **custom connector** with the same MCP URL
2. Complete **Allow** on Shop OS when prompted
3. Start a chat and manage catalog / orders through the assistant

### Claude Desktop / IDE only

Hosted custom connectors are the merchant path. For local Desktop/IDE experiments, community `mcp-remote` bridges exist — that is not required for normal Shop OS merchants.

## Gemini

Custom connectors may be limited by region. If you do not see an option to add a custom app, use ChatGPT or Claude for now.

## After you connect

In the Shop OS merchant app:

- **Settings → AI connector** — see connected apps, revoke one connection, or **Disable all AI access**
- **Activity** — every change the assistant makes, with a diff and **Undo**

## What the assistant can do

With the scopes you approve at connect time (defaults are read/write catalog, read orders, read analytics):

- Browse and update products and categories
- Check orders and storefront status
- Propose bulk or price changes — you confirm before they apply

Shop OS **never** lets the connector manage payouts, delete your account, or see your login password.

## Support

If Allow fails or tools error, check **Settings → AI connector** and revoke/reconnect. Contact your Shop OS operator with the time of the failure.
