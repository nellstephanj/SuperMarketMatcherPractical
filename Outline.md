# Super Market Matcher

***

## 🧪 Workshop Practical: **Super Market Matcher**

**Goal:** Build a small web app that helps shoppers find the best supermarket prices and deals (Jumbo, Albert Heijn, Plus).  
**Focus:** Back-end APIs + front-end UI + NoSQL + tests + docs.  
**Constraints:**

*   Backend: any language/framework
*   Frontend: web only (any framework)
*   Database: **NoSQL** (MongoDB/CosmosDB/Firestore/etc.)
*   Must include **clear documentation** and **unit tests** (bonus: integration tests)

***

## 🗺️ High-Level Features

### 1) Data ingestion (simulated “scraping”)

*   Daily job (CLI or API) imports prices for products from **Jumbo**, **Albert Heijn**, **Plus**.
*   For the workshop, **use the provided JSON dataset** (see below) to simulate scraping.
*   Store each day’s prices as time-series entries.

### 2) Search

*   Search products by name, brand, or category (e.g., “koffie”, “halve melk”, “chocola”).
*   Support partial matches and diacritics-insensitive matching (e.g., "yoghurt" \~ "yogurt").

### 3) Product Page includes

*   Product image
*   **Cheapest price (current)**
*   **Lowest price ever**
*   **Lowest price in last 6 months**
*   **Highest price**
*   Price **chart over time** (per store)
*   Product description
*   **Price comparison across supermarkets**

### 4) Home Page includes

*   Products with **“Lower prices than normal”**
*   **Best Deals** (your defined logic; see below)

***

## ✅ Acceptance Criteria

**Functional**

*   [ ] Search returns relevant products
*   [ ] Product page shows correct price stats and chart
*   [ ] Home page shows lower-than-normal items and best deals
*   [ ] API returns consistent formats and handles empty states
*   [ ] Data model supports multi-store daily prices

**Non-functional**

*   [ ] Uses a **NoSQL** database
*   [ ] Includes **unit tests** (bonus: integration tests)
*   [ ] Has a clear **README** with setup steps and how to run/test
*   [ ] Reasonable error handling and input validation
*   [ ] Front-end responsive enough to demo

***

## 🧱 Suggested Architecture

    /app
      /backend
        src/
        tests/
      /frontend
        src/
        tests/
      /docs
      /data (synthetic dataset)
      docker-compose.yml (optional)

**Recommended stacks (pick one):**

*   **Track A (JavaScript/TypeScript)**: Node.js + Express/Fastify + MongoDB + React/Vite + Chart.js
*   **Track B (Python)**: FastAPI + MongoDB + Vue/React + Chart.js
*   **Track C (.NET)**: ASP.NET Core Minimal APIs + Cosmos DB + React + Chart.js

**CI/CD (optional):** GitHub Actions for lint + test.

***

## 🧾 NoSQL Data Model (MongoDB-style)

**Collections:** `products`, `stores`, `priceSnapshots`

**stores**

```json
{
  "_id": "ah",
  "name": "Albert Heijn",
  "slug": "albert-heijn",
  "url": "https://www.ah.nl"
}
```

**products**

```json
{
  "_id": "prod_0001",
  "name": "AH Halfvolle Melk 1L",
  "brand": "AH",
  "category": "Melk & Zuivel",
  "imageUrl": "https://example.com/images/ah-melk-1l.jpg",
  "description": "Halfvolle melk, 1 liter."
}
```

**priceSnapshots**

```json
{
  "_id": "snap_2025-12-01_ah_prod_0001",
  "productId": "prod_0001",
  "storeId": "ah",
  "date": "2025-12-01",
  "price": 1.05,
  "unit": "EUR",
  "promo": {
    "isOnPromotion": true,
    "type": "korting",
    "originalPrice": 1.19,
    "label": "15% korting"
  }
}
```

**Indexes**

*   `priceSnapshots`: `{ productId: 1, storeId: 1, date: -1 }`
*   `products`: text or compound index on `{ name: "text", brand: "text", category: "text" }` (or a case-insensitive regex strategy)

***

## 📊 Price Metrics (definitions & logic)

Given all priceSnapshots for a product:

*   **Current cheapest price**: min of latest price per store (use most recent snapshot per `storeId`)
*   **Lowest price ever**: global min price across all snapshots
*   **Lowest price in last 6 months**: min price where `date >= today - 6 months`
*   **Highest price**: global max across all snapshots
*   **Lower than normal**: current price < (rolling 6-month median or average) by a chosen threshold (e.g., **≥10% lower**)
*   **Best Deals**: rank by **discount percentage** = `(avg6m - current) / avg6m`, optionally weighted by product popularity (if available)

*Tip:* Use rolling window metrics server-side to keep the front end simple.

***

## 🔌 API Specification (minimal)

**Search**

*   `GET /api/products?query=<q>&limit=<n>`
    *   Returns `[{ _id, name, brand, category, imageUrl }]`

**Product details**

*   `GET /api/products/:id`
    *   Returns product + computed price metrics + current per-store prices
    *   Response:
        ```json
        {
          "product": { "_id": "prod_0001", "name": "...", "brand": "...", ... },
          "metrics": {
            "currentCheapest": { "storeId": "ah", "price": 1.05 },
            "lowestEver": { "price": 0.95, "date": "2025-07-12" },
            "lowest6m": { "price": 0.99, "date": "2025-11-01" },
            "highestEver": { "price": 1.39, "date": "2025-03-10" }
          },
          "comparison": [
            { "storeId": "ah", "price": 1.05 },
            { "storeId": "jumbo", "price": 1.09 },
            { "storeId": "plus", "price": 1.12 }
          ]
        }
        ```

**Price history**

*   `GET /api/products/:id/price-history?range=6m`
    *   Returns `{ "series": [{ "storeId": "ah", "points": [{ "date": "...", "price": 1.05 }, ...] }, ...] }`

**Deals & lower-than-normal**

*   `GET /api/deals?limit=<n>`
*   `GET /api/lower-than-normal?limit=<n>`

**Admin ingestion (simulated)**

*   `POST /api/admin/ingest`  
    Reads local JSON and inserts snapshots for “today”. Secure or optional for the workshop.

***

## 🖥️ Frontend Pages

### Home

*   **“Lower than normal”** section (cards)
*   **“Best deals”** section (cards)
*   Each card shows image, product name, current cheapest price, badge for discount %, and button → product page

### Product Page

*   Header: image, name, brand, category
*   Metrics tiles: **Cheapest now**, **Lowest ever**, **Lowest 6 months**, **Highest**
*   **Price comparison** table (AH / Jumbo / Plus with current price)
*   **Price chart** over time (multi-series line chart by store)
*   Description text

### Search

*   Search box in header (debounced)
*   Results list → link to product page

**UI Tips**

*   Use a chart library (Chart.js, ECharts, Recharts)
*   Color-code stores consistently (e.g., AH=blue, Jumbo=yellow, Plus=green)
*   Badge for “On Promotion”

***

## 🧰 Synthetic Dataset (for ingestion)

Create `/data/seed-products.json`:

```json
[
  {
    "_id": "prod_melk_1l",
    "name": "Halfvolle Melk 1L",
    "brand": "Huismerk",
    "category": "Melk & Zuivel",
    "imageUrl": "/images/melk1l.png",
    "description": "Halfvolle melk, 1 liter."
  },
  {
    "_id": "prod_koffie_bonen",
    "name": "Koffiebonen 500g",
    "brand": "BrandX",
    "category": "Koffie & Thee",
    "imageUrl": "/images/koffiebonen.png",
    "description": "Aromatische koffiebonen, 500 gram."
  }
]
```

Create `/data/seed-prices.json` (a few months of prices, irregular promotions):

```json
[
  { "productId": "prod_melk_1l", "storeId": "ah",    "date": "2025-09-01", "price": 1.15, "unit": "EUR", "promo": null },
  { "productId": "prod_melk_1l", "storeId": "ah",    "date": "2025-10-01", "price": 1.09, "unit": "EUR", "promo": {"isOnPromotion": true, "label":"Actie"} },
  { "productId": "prod_melk_1l", "storeId": "ah",    "date": "2025-11-01", "price": 1.12, "unit": "EUR", "promo": null },
  { "productId": "prod_melk_1l", "storeId": "ah",    "date": "2025-12-01", "price": 1.05, "unit": "EUR", "promo": {"isOnPromotion": true, "label":"-10%"} },
  { "productId": "prod_melk_1l", "storeId": "jumbo", "date": "2025-09-01", "price": 1.18, "unit": "EUR", "promo": null },
  { "productId": "prod_melk_1l", "storeId": "jumbo", "date": "2025-11-15", "price": 1.08, "unit": "EUR", "promo": {"isOnPromotion": true, "label":"Aanbieding"} },
  { "productId": "prod_melk_1l", "storeId": "plus",  "date": "2025-09-01", "price": 1.14, "unit": "EUR", "promo": null },
  { "productId": "prod_melk_1l", "storeId": "plus",  "date": "2025-12-01", "price": 1.12, "unit": "EUR", "promo": null },

  { "productId": "prod_koffie_bonen", "storeId": "ah",    "date": "2025-09-01", "price": 7.99, "unit": "EUR", "promo": null },
  { "productId": "prod_koffie_bonen", "storeId": "ah",    "date": "2025-11-01", "price": 6.99, "unit": "EUR", "promo": {"isOnPromotion": true, "label":"Aanbieding"} },
  { "productId": "prod_koffie_bonen", "storeId": "jumbo", "date": "2025-10-01", "price": 7.49, "unit": "EUR", "promo": null },
  { "productId": "prod_koffie_bonen", "storeId": "plus",  "date": "2025-12-01", "price": 6.79, "unit": "EUR", "promo": {"isOnPromotion": true, "label":"-15%"} }
]
```

*Tip:* Seed \~5–10 products to make the UI interesting.

***

## 🧮 Example Metric Calculation (TypeScript snippet)

```ts
type Snap = { date: string; price: number; storeId: string };
type SeriesByStore = Record<string, Snap[]>;

function computeMetrics(seriesByStore: SeriesByStore) {
  const all = Object.values(seriesByStore).flat();
  if (all.length === 0) return null;

  const parse = (d: string) => new Date(d).getTime();
  const now = Date.now();
  const sixMonthsAgo = new Date();
  sixMonthsAgo.setMonth(sixMonthsAgo.getMonth() - 6);

  // Latest per store
  const latestByStore = Object.fromEntries(
    Object.entries(seriesByStore).map(([store, snaps]) => {
      const latest = snaps.sort((a,b) => parse(b.date)-parse(a.date))[0];
      return [store, latest];
    })
  );

  const currentCheapest = Object.values(latestByStore).reduce((min, s) => s.price < min.price ? s : min);

  const lowestEver = all.reduce((min, s) => s.price < min.price ? s : min);
  const highestEver = all.reduce((max, s) => s.price > max.price ? s : max);

  const lowest6m = all
    .filter(s => new Date(s.date) >= sixMonthsAgo)
    .reduce((min, s) => s.price < (min?.price ?? Infinity) ? s : min, null as Snap | null);

  return { currentCheapest, lowestEver, lowest6m, highestEver };
}
```

***

## 🧪 Testing Requirements

### Unit Tests (must-have)

*   **Metrics**
    *   Calculates current cheapest correctly across stores
    *   Lowest ever / highest ever / lowest in 6 months match expected values
    *   Handles edge cases (no data in last 6 months, single store only)
*   **Search**
    *   Case-insensitive and partial match
    *   No results → empty list
*   **API**
    *   `/api/products/:id` returns all expected fields
    *   `/api/products/:id/price-history` returns well-formed series
    *   Bad `:id` → 404

### Integration Tests (bonus)

*   Spin up **MongoDB test container** or `mongodb-memory-server` (JS)
*   Seed small dataset → call API endpoints → assert responses
*   Frontend **Cypress** E2E: search → navigate to product → chart renders → metrics visible

***

## 🧰 Documentation Checklist

**README.md**

*   Project overview + architecture diagram (ASCII is fine)
*   Tech choices & rationale (NoSQL, framework)
*   Setup (env vars, DB connection)
*   Running locally (with or without Docker)
*   Running tests (unit + integration)
*   API reference (OpenAPI/Swagger JSON or a concise section in the README)
*   Data ingestion (how to load `/data/seed-*.json`)
*   Limitations & next steps

**Optional extras**

*   **OpenAPI** at `/swagger` or `/docs`
*   **ADR** (Architecture Decision Records) for major choices
*   Postman collection

***

## 🧩 Stretch Goals (if time allows)

*   **Compare basket**: let users add several products and see per-store total
*   Price alert subscriptions (email/web push on price drops)
*   **Auth** (basic) to save favourites
*   **PWA** (installable) + offline cache of last viewed products
*   **Internationalization** (NL/EN)
*   **Caching** (e.g., Redis) and rate-limiting
*   **Background scheduler** (cron/queue) for daily ingestion
*   Accessibility pass (keyboard navigation, color contrasts)
*   Sentry/Logging/Tracing

***
