# Boliga Denmark Scraper - Homes for Sale

Extract structured data from [boliga.dk](https://boliga.dk) — Danish homes for sale from boliga.dk. Each listing carries price, address and monthly costs plus the selling agency's email and phone. Search by postcode or radius or paste a boliga.dk search URL. Get structured rows ready for a spreadsheet or a CRM.

**[Boliga Denmark Scraper - Homes for Sale on Apify →](https://apify.com/blackfalcondata/boliga-dk-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch structured metadata for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N listings (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from boliga.dk on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from boliga.dk.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "København",
  "maxResults": 100,
  "includeDetails": true,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Free-text search: street, city or postcode (e.g. "Nyhavn", "Aarhus C"). Required if no startUrls provided. |
| `location` | string | — | Narrow by place: a 4-digit Danish postcode (e.g. "9500") or a 3-digit municipality code (e.g. "101" for Copenhagen). Any other text is added to the search term. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste boliga.dk search-results URLs (www.boliga.dk/resultat?...) or regional result pages (e.g. www.boliga.dk/resultat/landsdele/koebenhavn-by/alle-til-salg): set the filters on the site, then copy the address bar. Each URL becomes its own search task; results are merged and deduped by listing ID across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Also fetch each listing's detail page for the estate agent's contact details (agency name, phone, e-mail, website), the agent's own listing URL and the registered area. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `lat` | number | — | Centre of a radius filter (latitude). Set lat, lon and radiusKm together to keep only listings within that distance; listings without coordinates are left out. |
| `lon` | number | — | Centre of a radius filter (longitude). Use together with lat and radiusKm. |
| `radiusKm` | integer | — | Keep only listings within this many kilometres of lat/lon. Leave empty for no radius filter. |

---

## Output fields

Every listing returns the same 78-field schema. Missing values are `null` — never omitted.

- `id`
- `ouId`
- `guid`
- `bfeNr`
- `dawaId`
- `adresseId`
- `ouAddress`
- `url`
- `portalUrl`
- `addressUrl`
- `street`
- `cleanStreet`
- `city`
- `zipCode`
- `municipality`
- `latitude`
- `longitude`
- `area`
- `country`
- `propertyType`
- `propertyTypeName`
- `secondaryPropertyType`
- `rooms`
- `size`
- `lotSize`
- `basementSize`
- `floor`
- `buildYear`
- `energyClass`
- `registeredArea`
- `price`
- `priceCurrency`
- `squaremeterPrice`
- `priceChangePercentTotal`
- `priceChangeCashTotal`
- `downPayment`
- `exp`
- `net`
- `monthlyExpenses`
- `monthlyNet`
- `evaluationPrice`
- `lastSoldPrice`
- `lastSoldDate`
- `lastSeenPrice`
- `daysForSale`
- `views`
- `openHouse`
- `isActive`
- `isForeclosure`
- `selfsale`
- `onTheWay`
- `createdDate`
- `lastSeen`
- `agentRegId`
- `agentDisplayName`
- `domainId`
- `isPremiumAgent`
- `estateUrl`
- `agencyName`
- `agencyUrl`
- `agencyPhone`
- `agencyEmail`
- `agencyAddress`
- `imageUrl`
- `imageUrls`
- `images`
- `title`
- `entityName`
- `location`
- `listingId`
- `sourceQuery`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "id": "2373593",
  "ouId": 1763449237,
  "guid": "67AA988C-9ACC-4DDF-838A-2325925E3D51",
  "bfeNr": 104384,
  "dawaId": "1DF3184C-6BB5-61D6-E044-0003BA298018",
  "adresseId": "1DF3184C-6BB5-61D6-E044-0003BA298018",
  "ouAddress": "nyhavn-12d-1-th-1051-koebenhavn-k",
  "url": "https://www.boliga.dk/bolig/2373593/nyhavn-12d-1-th-1051-koebenhavn-k",
  "portalUrl": "https://www.boliga.dk/bolig/2373593/nyhavn-12d-1-th-1051-koebenhavn-k",
  "addressUrl": "https://www.boliga.dk/adresse/nyhavn-12d-1-th-1051-koebenhavn-k-1763449237",
  "street": "Nyhavn 12D, 1. th.",
  "cleanStreet": "Nyhavn"
}
```

*Truncated — full records contain 78 fields. See Output fields for the complete schema.*

**[Try Boliga Denmark Scraper - Homes for Sale now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/boliga-dk-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000045 |
| Result | $0.001791 |

See the [actor on Apify](https://apify.com/blackfalcondata/boliga-dk-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape boliga.dk?**
Use this actor on Apify to extract structured data from boliga.dk. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get boliga.dk data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape boliga.dk?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check boliga.dk's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Boliga Denmark Scraper - Homes for Sale](https://apify.com/blackfalcondata/boliga-dk-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
