# Proshop Denmark Scraper

Extract structured data from [proshop.dk](https://proshop.dk) — proshop.dk products by keyword or category: name, brand, price incl. and excl. VAT, discount and stock status. Export clean JSON of Danish electronics listings for price monitoring and catalog research across the shop.

**[Proshop Denmark Scraper on Apify →](https://apify.com/blackfalcondata/proshop-dk-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from proshop.dk on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from proshop.dk.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "laptop",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Keywords to search the proshop.dk catalogue for, e.g. "laptop", "rtx 5080", "logitech mx". Required unless you provide a category or Start URLs. Use a JSON array for several searches in one run, e.g. ["laptop","ssd"]. |
| `category` | string | — | Optional. Browse a whole proshop.dk category instead of searching — use the category path from the site URL, e.g. "Baerbar" for www.proshop.dk/Baerbar. A category REPLACES the search term (proshop.dk cannot combine the two). The run fails fast if the category does not exist. Use a JSON array for several categories. |
| `maxResults` | integer | `100` | Maximum total products (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw proshop.dk search or category URLs, e.g. https://www.proshop.dk/?s=laptop or https://www.proshop.dk/Baerbar. Each URL becomes its own task; results are merged and deduped by product ID across all URLs. When provided, startUrls REPLACE the search term and category fields (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Open each product page to add EAN (barcode), the canonical manufacturer part number and the large product image. Off = list data only, which is much faster. |
| `descriptionMaxLength` | integer | `0` | Truncate the short product description to N characters. 0 = no truncation. |
| `compact` | boolean | `false` | Price and stock fields only (for AI-agent/MCP workflows and price monitoring). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state and report each product as NEW / UPDATED / UNCHANGED / EXPIRED, including price and stock changes. stateKey is optional — it defaults to a value derived from your search inputs (search terms, categories, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked product universe. Leave empty to auto-generate from the search inputs. |
| `emitUnchanged` | boolean | `false` | When incremental, also return products whose data has not changed since the last run. Off = only NEW and UPDATED products are returned. |
| `emitExpired` | boolean | `false` | When incremental, scan the whole selection (Max Results does not stop the scan) so products that have disappeared from proshop.dk are marked EXPIRED in the saved state. |
| `skipReposts` | boolean | `false` | When incremental, skip products whose content matches a product that disappeared from the catalogue in a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED products (e.g. price drops). Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of products included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (product, brand, price, stock, URL). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every product returns the same 29-field schema. Missing values are `null` — never omitted.

- `listingId`
- `productId`
- `name`
- `brand`
- `category`
- `price`
- `originalPrice`
- `priceExVat`
- `currency`
- `onSale`
- `discountPercent`
- `inStock`
- `stockStatus`
- `badges`
- `mpn`
- `ean`
- `rating`
- `reviewCount`
- `description`
- `imageUrl`
- `url`
- `portalUrl`
- `scrapedAt`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "listingId": "079b915a0d38b05dfdc698082941e78fdad093704da9d88e94f1d44da0b34244",
  "productId": "3263642d",
  "name": "Samsonite Securipak 2.0 Rygsæk til Laptop 15.6\" | Sort *DEMO*",
  "brand": "Samsonite",
  "category": "Tasker",
  "price": 565,
  "originalPrice": 565,
  "priceExVat": 452,
  "currency": "DKK",
  "onSale": false,
  "discountPercent": null,
  "inStock": true
}
```

*Truncated — full records contain 29 fields. See Output fields for the complete schema.*

**[Try Proshop Denmark Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/proshop-dk-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.0018 |

See the [actor on Apify](https://apify.com/blackfalcondata/proshop-dk-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape proshop.dk?**
Use this actor on Apify to extract structured data from proshop.dk. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get proshop.dk data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape proshop.dk?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check proshop.dk's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Proshop Denmark Scraper](https://apify.com/blackfalcondata/proshop-dk-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
