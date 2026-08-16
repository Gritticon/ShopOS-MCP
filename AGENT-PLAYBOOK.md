# Shop OS — agent playbook (for ChatGPT / Claude)

Connected AIs get a short copy of these rules from MCP `initialize.instructions`.
For best results in **ChatGPT**, also paste the block in § “Custom instructions” below.

Full skill in the Shop OS repo: `.claude/skills/shopos-merchant-ops/`.

---

## ChatGPT / Claude quirks (read this)

1. **Tools must be on in the chat.** If the model says it has no access / no memory / asks for a spreadsheet, the Shop OS connector is not attached to that turn. Enable / “use connector” / load tools for Shop OS, then ask again.
2. **“Skills” ≠ this playbook.** ChatGPT’s Skills catalog is separate. Shop OS access is the **MCP connector**, not a Skill plugin. Saying “do you have Shop OS skills?” will get a wrong answer — ask “use Shop OS tools” or just “how many products do I have?” with the connector enabled.
3. After reconnect, start a **new chat** with the connector enabled so `initialize.instructions` load.

### Custom instructions (paste into ChatGPT / Custom GPT)

```text
You have a Shop OS MCP connector for THIS merchant's store (Gritticon Shop OS — NOT Shopify, NOT "Cowork").
When Shop OS tools are enabled in the chat:
- "do you have Shop OS skill?" → Answer: Yes — it is an MCP connector, not a ChatGPT Skill. I use storefront/catalog/orders/analytics tools.
- "my business" → storefront.get_status immediately. Never ask which business.
- "how many products" → catalog.list_categories. Never ask for a spreadsheet.
- "sales today" → storefront.get_status then analytics.sales_summary.
If tools are not loaded, tell the user to enable the Shop OS connector in this chat.
Playbook: https://github.com/Gritticon/shopos-mcp/blob/main/AGENT-PLAYBOOK.md
```

---

## Who you are talking to

The OAuth token already selects **this merchant’s store**. Never ask “which business?”

1. Call `storefront.get_status` first (slug, published, accepting orders, timezone, hours).
2. For sales “today”, use that **timezone** with `analytics.sales_summary`.
3. For product counts, use `catalog.list_categories` and/or paginate `catalog.list_products` until complete — not one page of 50.

Merchant app: https://shopos.gritticon.com

---

## Catalog shape

```
Category → Product → Section → Option
```

- **Wrong:** “Burger Large” and “Burger Small” as two products.
- **Right:** one product “Burger” + section Size + options Regular / Large.

**Tool reality today**

- `catalog.create_product` and import create **flat** products only.
- After creating the base item, guide the merchant to **Catalog → product** to add sections/options and upload photos.
- `images` on tools = **hosted URL list only** (no MCP file upload).

Bulk / import / refunds: **propose → merchant confirms → apply**.

---

## Guide humans in the app

| Goal | Where |
|---|---|
| Business name / slug | **Settings → Account** (`/settings/account`) |
| Logo / colours | **Settings → Branding** (`/settings/branding`) |
| Hours / timezone | **Settings → Opening hours** (`/settings/hours`) |
| Delivery / pickup | **Settings → Delivery & pickup** (`/settings/delivery`) |
| Revoke AI / kill switch | **Settings → AI connector** (`/settings/connector`) |
| Undo a change | **Activity** (`/activity`) |
| Sizes, add-ons, photos | **Catalog** → open product (`/catalog/products/{id}`) |

---

## Sample flat import

```json
{
  "kind": "import",
  "categories": [{ "name": "Burgers" }],
  "products": [
    {
      "category_name": "Burgers",
      "name": "Classic burger",
      "description": "Add Size & Add-ons in Catalog after import",
      "base_price": 899
    }
  ]
}
```

Prices are integer **minor units** (899 = £8.99 if GBP).
