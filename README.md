# Shop OS MCP

Connect **ChatGPT**, **Claude**, or **Cursor** to your Shop OS store.

This public repo is the **install package** merchants share.  
The live connector runs at:

```text
https://api.gritticon.com/mcp
```

You never copy a token. After you add the connector, Shop OS opens an **Allow** screen (OAuth).

---

## Install from this GitHub repo

### Option A — ChatGPT / Claude (custom connector)

1. Open this repo: [Gritticon/ShopOS-MCP](https://github.com/Gritticon/ShopOS-MCP)
2. Copy the connector URL from [`mcp.json`](./mcp.json) → `url`  
   (**`https://api.gritticon.com/mcp`**)
3. In ChatGPT or Claude: **Settings → Connectors / Connected apps → Add custom connector**
4. Paste that URL
5. Sign in to Shop OS if asked → click **Allow**
6. Ask: “List my catalog” or “Show today’s orders”

### Option B — Cursor / Claude Code (config file)

Copy [`mcp.json`](./mcp.json) into your project as `.mcp.json` (or merge into your existing MCP config):

```json
{
  "mcpServers": {
    "shop-os": {
      "type": "http",
      "url": "https://api.gritticon.com/mcp"
    }
  }
}
```

Or with Claude Code:

```bash
claude mcp add --transport http shop-os https://api.gritticon.com/mcp
```

On first tool use the client should open Shop OS OAuth (**Allow**).

### Option C — Share only the repo link

Send merchants:

```text
https://github.com/Gritticon/ShopOS-MCP
```

They follow **Option A** or **B** above. The repo always documents the current connector URL.

---

## What you need

- A Shop OS merchant account  
- ChatGPT / Claude with custom connectors, **or** Cursor / Claude Code with MCP enabled

## After you connect

In the Shop OS merchant app ([shopos.gritticon.com](https://shopos.gritticon.com)):

- **Settings → AI connector** — connected apps, revoke one, or **Disable all AI access**
- **Activity** — every assistant change, with a diff and **Undo**

## What the assistant can do

With scopes you approve (defaults: catalog read/write, orders read, analytics read):

- Browse and update products and categories  
- Check orders and storefront status  
- Propose bulk or price changes — you confirm before they apply  

Shop OS **never** grants payouts, account deletion, or login credentials to the connector.

## Gemini

Custom connectors may be limited by region. If unavailable, use ChatGPT or Claude.

## Support

If Allow fails: **Settings → AI connector** → revoke → reconnect.  
Operator contact: your Shop OS admin.

## Repo layout

| File | Purpose |
|---|---|
| [`mcp.json`](./mcp.json) | Canonical connector URL for clients |
| [`cursor-mcp.example.json`](./cursor-mcp.example.json) | Cursor-shaped example |
| [`AGENT-PLAYBOOK.md`](./AGENT-PLAYBOOK.md) | How connected AIs should run the store (menu model + UI paths) |
| [`LICENSE`](./LICENSE) | Docs license — **no server source** here |

Server source is private (`shop-os-mcp-ecs`). This repo is documentation + install manifests only.
