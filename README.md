# Google Trends Scraper

![Google Trends Scraper](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/google-trends/banner.webp)

A Google Trends API without a browser or login: interest over time, regions, cities, related queries and the Trending now list. Runs on [Apify](https://apify.com/smoldesk/google-trends-scraper).

## What you get

- **Interest over time**: the full timeline plus average, peak, latest value, first-vs-last quarter change and a rising or falling label.
- **By region and city**: countries, sub-regions, cities and US metro areas (DMA).
- **Related queries**: top and rising, with `Breakout` or the percent growth.
- **Comparison**: up to 5 terms in one chart, as Google Trends does it.
- **Trending now**: current trending searches per country with search volume, percent increase, start time, categories and related searches.
- **Plain-English `summary`** of each result, ready to pass to an LLM.

## Run it

Open the [Google Trends Scraper on Apify](https://apify.com/smoldesk/google-trends-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~google-trends-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "searchTerms": ["bitcoin", "ethereum"],
    "geos": ["US", "GB"],
    "timeRange": "today 12-m",
    "includeRegions": true,
    "includeRelatedQueries": true,
    "trendingGeos": ["US"],
    "maxTrendingPerGeo": 25
  }'
```

You can also paste Google Trends URLs into `startUrls`.

## Sample output

```json
{
  "type": "keyword",
  "searchTerm": "bitcoin",
  "geo": "US",
  "timeRange": "today 12-m",
  "average": 34.3,
  "peakValue": 100,
  "peakDate": "2026-02-01",
  "latestValue": 15,
  "changePercent": -45.7,
  "trendDirection": "falling",
  "interestOverTime": [{"date": "2025-10-12", "value": 42, "isPartial": false}],
  "interestByRegion": [{"name": "Wyoming", "geoCode": "US-WY", "value": 100}],
  "relatedQueriesRising": [{"query": "spcx stock", "value": 14500, "growth": "Breakout"}]
}
```

```json
{
  "type": "trending",
  "title": "galatasaray vs barcelona",
  "geo": "US",
  "searchVolume": 200000,
  "increasePercent": 1000,
  "startedAt": "2026-10-13T16:50:00Z",
  "isActive": true,
  "categories": ["Sports"],
  "relatedSearches": ["galatasaray vs barcelona", "lamine yamal", "barca game"]
}
```

## Pricing

Pay per result. No monthly fee. Prices drop on paid Apify plans.

| Result (per 1,000) | Free | Starter | Scale | Business |
|---|---|---|---|---|
| Keyword or comparison | $1.50 | $1.50 | $1.30 | $1.15 |
| Trending now search | $0.50 | $0.50 | $0.50 | $0.50 |

A keyword Google has no data for comes back with `hasData: false` and is not charged.

## Limits

- Related topics come back empty for automated requests since September 2026. Related queries are not affected. `includeRelatedTopics` is off by default.
- Google samples cities, so niche keywords can have few or none. Regions and US metros cover well.
- Google rate-limits heavy use. Runs slow down or stop without charge instead of returning bad data.

## FAQ

**Is there an official Google Trends API?**
Google announced an alpha API in 2025 for a small group of approved testers. This gives you the same data over a normal API today.

**Is this a pytrends alternative?**
Yes. pytrends is no longer maintained and hits HTTP 429 quickly. This handles rate limits and retries for you, with nothing to install.

**How do I compare keywords?**
Set `compareTerms` to true with up to 5 `searchTerms`. You get one `comparison` record with all terms on the same 0 to 100 scale.

**How do I get Google Trends data for YouTube or News?**
Set `property` to `youtube`, `news`, `images` or `froogle` (Shopping).

**Can I get what is trending right now?**
Yes. Set `trendingGeos` to country codes. Use `trendingHours` for the window and `trendingCategory` to filter.

## More scrapers

Google Trends is one of 21 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
