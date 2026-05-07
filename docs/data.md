# Data SDK

`@voyantjs/data-sdk` is the public TypeScript client for the Voyant Data APIs.

## Sub-products

The Voyant Data product is composed of seven Cloudflare Workers served
behind the public gateway at `https://api.voyantjs.com/data/{product}/v1/*`:

- `static` — typed reference data (countries, regions, cities, airports,
  airlines, aircraft, languages, currencies, timezones, geographic regions)
- `fx` — currency exchange (exchangerate-api.com white-label, `/data/fx/v1/fx/*`)
- `seo` — DataForSEO white-label, organized by sub-product
- `reviews` — Google Reviews / Extended Reviews / Q&A + Trustpilot
- `hotels` — Google Hotels + TripAdvisor (hotel-scoped)
- `restaurants` — TripAdvisor restaurants
- `experiences` — TripAdvisor attractions / experiences

## Current shape

Every sub-product is a top-level namespace on the client. Examples:

- `client.static.countries.list({ region: "Europe" })`
- `client.fx.latest("USD")`
- `client.seo.serp.google.organic.searches.create(input)`
- `client.seo.backlinks.summary.create(input)`
- `client.seo.onPage.siteAudits.create(input)`
- `client.reviews.google.qa.run(input)`
- `client.hotels.google.hotelSearches.create(input)`
- `client.hotels.tripadvisor.reference.locations.list({ country: "GB" })`
- `client.restaurants.tripadvisor.searches.create(input)`
- `client.experiences.tripadvisor.reviews.list({ limit: 5 })`

`list` / `search` / `nearby` return `ListResponse<T>` (`{ data, totalCount,
nextCursor? }`); `get(id)` returns `SingleResponse<T>` (`{ data }`).

## Key public types

- static: `Country`, `Region`, `City`, `Airport`, `Airline`, `Aircraft`,
  `LanguageEntry`, `CurrencyEntry`, `TimezoneEntry`, `GeographicRegion`
- envelopes: `ListResponse<T>`, `SingleResponse<T>`, `PaginationParams`
- fx: `FxLatestResponse`, `FxPairResponse`, `FxEnrichedResponse`,
  `FxHistoryResponse`, `FxCodesResponse`, `FxQuotaResponse`
- seo / serp: `Search`, `GoogleOrganicSearchInput`, `GoogleAiModeSearchInput`,
  `GoogleMapsSearchInput`, `ScreenshotResult`, `AiSummaryResult`
- verticals: `GoogleReviewsRequest`, `GoogleQaRequest`,
  `TrustpilotSearchRequest`, `GoogleHotelSearchesRequest`,
  `TripadvisorSearchRequest`, `TripadvisorReviewsRequest`,
  `TripadvisorReferenceLocation`
- errors: `DataErrorCode`

## Auth scopes

API tokens are scoped per sub-product:

- `client.static.*` requires `data:static:read`
- `client.fx.*` requires `data:fx:read`
- `client.seo.*` requires `data:seo:read`
- `client.reviews.*` requires `data:reviews:read`
- `client.hotels.*` requires `data:hotels:read`
- `client.restaurants.*` requires `data:restaurants:read`
- `client.experiences.*` requires `data:experiences:read`

## Example

```ts
import { createVoyantDataClient } from "@voyantjs/data-sdk";

const client = createVoyantDataClient({
  apiKey: process.env.VOYANT_API_KEY!,
});

const europeanCountries = await client.static.countries.list({ region: "Europe" });
const lhr = await client.static.airports.get("LHR");
const eurUsd = await client.fx.pair("EUR", "USD", 100);
```

## Why SEO is namespaced (not generic)

The DataForSEO surface is hundreds of routes. Rather than re-export each
under a typed method tree, the client groups them by DataForSEO
sub-product (`serp`, `keywordsData`, `aiOptimization`, `backlinks`,
`onPage`, `contentAnalysis`, `domainAnalytics`, `businessData`,
`dataforseoLabs`) so consumers can navigate by domain. Reference
catalogues (locations + languages) live under each sub-product because
DataForSEO scopes them per-product.
