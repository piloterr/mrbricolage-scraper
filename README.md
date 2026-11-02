<p align="center">
  <a href="https://www.piloterr.com"><img src="images/banner.png" alt="Mr.Bricolage Scraper API by Piloterr – scrape Mr.Bricolage data as JSON" width="100%"></a>
</p>

# Mr.Bricolage Scraper API

[![Discord](https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white)](https://discord.gg/CFj3ph8aHU) [![Get API key](https://img.shields.io/badge/Piloterr-Get%20a%20free%20API%20key-111827)](https://app.piloterr.com) [![API docs](https://img.shields.io/badge/Docs-OpenAPI-0EA5E9)](https://app.piloterr.com/openapi) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

The **Piloterr Mr.Bricolage Scraper API** lets you extract public data from Mr.Bricolage as **clean, structured JSON** — no proxies, headless browsers or HTML parsing to maintain. Endpoints available in production:

- **Mr.Bricolage Suggest API** — Live Mr.Bricolage product suggestions for a search keyword or EAN, including title, URL, image, price and currency.

Use it for price monitoring, assortment tracking, competitor analysis, product research or catalog enrichment.

| Endpoint | Path | Credits | What you get |
|---|---|---|---|
| [Mr.Bricolage Suggest API](#mrbricolage-suggest-api) | `/v2/mrbricolage/suggest` | 3 | Live Mr.Bricolage product suggestions for a search keyword or EAN, including title, URL, image, price and currency. |

## Table of contents

- [How it works](#how-it-works)
- [Mr.Bricolage Suggest API](#mrbricolage-suggest-api)
- [Code examples in other languages](#code-examples-in-other-languages)
- [Practical use cases](#practical-use-cases)
- [Why Piloterr?](#why-piloterr)
- [FAQ](#faq)
- [Related scrapers](#related-scrapers)
- [Support](#support)

## How it works

1. **Create a free Piloterr account** and copy your API key from the [dashboard](https://app.piloterr.com).
2. **Send a `GET` (or `POST`) request** to `https://api.piloterr.com/v2/...` with the header `x-api-key: YOUR_API_KEY`.
3. **Receive structured JSON** — Piloterr handles rendering, proxies, retries and parsing.

```bash
export PILOTERR_API_KEY="your-api-key"
pip install requests        # Python
# TypeScript: Node.js 18+ (native fetch), run with `npx tsx file.ts`
```

> Every endpoint accepts both `GET` (query string) and `POST` (JSON body with the same fields).

## Mr.Bricolage Suggest API

Live Mr.Bricolage product suggestions for a search keyword or EAN, including title, URL, image, price and currency.

`GET | POST https://api.piloterr.com/v2/mrbricolage/suggest` · **3 credits per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Search keyword or EAN used to query the Mr.Bricolage live suggest endpoint. Accepts product names, partial titles or numeric identifiers such as EAN barcodes. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/mrbricolage/suggest",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "marteau",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type MrBricolageSuggest = {
  total?: number;
  results?: {
    url?: string;
    type?: string;
    image?: string;
    price?: number;
    title?: string;
    currency?: string;
  }[];
};

const params = new URLSearchParams({
  "query": "marteau",
});

const response = await fetch(`https://api.piloterr.com/v2/mrbricolage/suggest?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as MrBricolageSuggest;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/mrbricolage/suggest
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "marteau"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "total": 5,
  "results": [
    {
      "url": "https://www.mr-bricolage.fr/marteau-perforateur-rh800-800-w.html",
      "type": "product",
      "image": "https://media.mr-bricolage.fr/media/catalog/product/cache/3a46d67a25d94f618cdcf20d0c3383a5/3/6/3603745041702_b1_ad04.jpg",
      "price": 92.9,
      "title": "Marteau perforateur rh800 800 w",
      "currency": "EUR"
    },
    {
      "url": "https://www.mr-bricolage.fr/coffret-marteau-burineur-4-burins-pour-compresseur-michelin.html",
      "type": "product",
      "image": "https://media.mr-bricolage.fr/media/catalog/product/cache/3a46d67a25d94f618cdcf20d0c3383a5/8/0/8020119077245_b1_b661.jpg",
      "price": 37.9,
      "title": "Coffret marteau burineur 4 burins pour compresseur - MICHELIN",
      "currency": "EUR"
    }
  ]
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `total` | integer |  |
| `results[]` | object[] |  |
| `results[].url` | string |  |
| `results[].type` | string |  |
| `results[].image` | string |  |
| `results[].price` | number |  |
| `results[].title` | string |  |
| `results[].currency` | string |  |

## Code examples in other languages

Ready-to-run examples live in the [`examples/`](examples) folder. Each one reads your key from the `PILOTERR_API_KEY` environment variable.

| Language | Suggest |
|---|---|
| cURL (Bash) | [`suggest`](examples/curl/suggest.sh) |
| PHP | [`suggest`](examples/php/suggest.php) |
| C# | [`suggest`](examples/csharp/Suggest.cs) |
| Go | [`suggest`](examples/go/suggest.go) |
| Java | [`suggest`](examples/java/SuggestExample.java) |
| Ruby | [`suggest`](examples/ruby/suggest.rb) |
| Node.js | [`suggest`](examples/nodejs/suggest.mjs) |

## Practical use cases

- **Price monitoring** — Track prices, promotions and availability over time to react to competitors.
- **Assortment & catalog tracking** — Follow new products, delisted items and category changes.
- **Competitor analysis** — Benchmark ranges, brands and positioning across retailers.
- **Product research** — Spot best-sellers and trends from search results.
- **Catalog enrichment** — Pull titles, images and attributes into your PIM or marketplace listings.

## Why Piloterr?

- **No infrastructure** — no proxies, headless browsers or CAPTCHA solving to run yourself.
- **Structured JSON** — stable, documented fields instead of brittle HTML selectors.
- **Pay per request** — transparent credit cost per endpoint, failed requests are retried for you.
- **One API key, hundreds of sources** — Mr.Bricolage plus many other e-commerce, search and company data endpoints in the [Piloterr library](https://www.piloterr.com/library).
- **Any language** — plain HTTPS + JSON, with examples for Python, TypeScript, cURL, PHP, C#, Go, Java, Ruby and Node.js.

## FAQ

### Is it legal to scrape Mr.Bricolage?

Collecting publicly available data is generally allowed in many jurisdictions, but rules depend on your country, the data you collect and how you use it. Avoid personal data unless you have a lawful basis, respect the website's terms where they apply, and get legal advice for your specific use case. This is not legal advice.

### Do I need proxies or a headless browser?

No. Piloterr handles rendering, proxy rotation and retries on its side. You only send an HTTPS request with your `x-api-key` header and receive JSON.

### How much does a request cost?

Credits per call: `/v2/mrbricolage/suggest` **3**. See the [Piloterr website](https://www.piloterr.com) for plans.

### What data does the Mr.Bricolage API return?

Each endpoint returns structured JSON: **Mr.Bricolage Suggest** — `total`, `results[]`, `results[].url`, `results[].type`, `results[].image`, `results[].price`.

### Which languages are supported?

Any language that can make an HTTP request. This README shows Python and TypeScript; the [`examples/`](examples) folder contains cURL, PHP, C#, Go, Java, Ruby and Node.js.

## Related scrapers

- [About You Scraper API](https://github.com/piloterr/aboutyou-scraper)
- [Action Scraper API](https://github.com/piloterr/action-scraper)
- [Aldi Scraper API](https://github.com/piloterr/aldi-scraper)
- [Alibaba Scraper API](https://github.com/piloterr/alibaba-scraper)
- [AliExpress Scraper API](https://github.com/piloterr/aliexpress-scraper)
- [Allegro Scraper API](https://github.com/piloterr/allegro-scraper)
- [All Piloterr scrapers on GitHub](https://github.com/piloterr)

## Support

- 💬 Join the [Piloterr Discord](https://discord.gg/CFj3ph8aHU) to ask questions and share what you build.
- 📚 Browse the [API reference (OpenAPI)](https://app.piloterr.com/openapi) and the [Piloterr library](https://www.piloterr.com/library).
- 🔑 [Get your API key](https://app.piloterr.com).

---

<p align="center">
  <a href="https://www.piloterr.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="images/logo-dark.svg">
      <img src="images/logo.svg" alt="Piloterr" height="32">
    </picture>
  </a>
  <br>
  <sub>Built by <a href="https://www.piloterr.com">Piloterr</a> — web scraping APIs for e-commerce, search, real estate and more, one API key.</sub>
</p>

<sub>Mr.Bricolage is a trademark of its respective owner. This repository is not affiliated with or endorsed by Mr.Bricolage. Examples are released under the MIT license.</sub>
