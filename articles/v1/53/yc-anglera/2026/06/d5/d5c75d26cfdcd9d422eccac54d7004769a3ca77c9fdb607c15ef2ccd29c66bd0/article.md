---
schema_version: "1.0.0"
document_id: "d5c75d26cfdcd9d422eccac54d7004769a3ca77c9fdb607c15ef2ccd29c66bd0"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/footwear-state"
published_at: "2026-06-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:591f0327909f708ed3fca7c6c9cee0a9ba5143d8bc740de7230f031b3d5a5d68"
---

# Footwear brands have a product-data problem — and 2026 is when it costs sales

Footwear is one of the highest-return categories in retail, and most brands still ship product pages that can't answer the one question that drives the purchase: will this fit. That gap was expensive when only humans were shopping. In 2026, with AI agents reading the feed instead of the page, it's starting to be disqualifying.


## The footwear catalog looks fuller than it is


Walk a footwear PIM and you'll usually find a size run, a color, a price, and a hero image. What you often won't find: width options beyond "standard," last shape, stack height, drop, toe box volume, break-in behavior, or which insole the shoe ships with. Those are the attributes that actually predict fit, and they're the ones most feeds treat as optional.


This isn't a small-brand problem. It shows up at scale because footwear sizing itself isn't standardized. A size 9 from one brand and a size 9 from another can differ by half a size in real length, before you even get to width or volume. Retailers that carry more than one brand inherit every brand's sizing quirks and rarely normalize them into a single, comparable spec.


Google's own Merchant Center guidance treats standardized sizing as a first-class requirement for apparel and footwear listings, not a nice-to-have, precisely because unstandardized size values break matching and search relevance ([Google Merchant Center product data specification](https://support.google.com/merchants/answer/7052112?hl=en) ). Miss it, and the product doesn't just look thin, it becomes harder to find.


## What thin footwear data actually costs


Three things break when footwear attributes are missing or inconsistent, and they compound.


**Search and discovery.** A shopper filtering for "wide toe box" or "4mm drop" never sees a shoe that qualifies but wasn't tagged that way. The product exists in the catalog and is invisible in the result set.


**Conversion.** Shoppers who can't confirm fit from the page either bounce to a competitor's listing that answers the question, or buy anyway and gamble.


**Returns.** That gamble is footwear's actual, measurable cost, and the numbers are large:


Metric Figure Source


Typical footwear online return rate 17-30%[Fittingbox, footwear return rate benchmarks](https://footwear.fittingbox.com/en/blog/footwear-return-rate-benchmarks-and-key-metrics-to-track)


Apparel and footwear brands at or above 30% return rates (2025) 56% Radial 2025, via[Fittingbox](https://footwear.fittingbox.com/en/blog/footwear-return-rate-benchmarks-and-key-metrics-to-track)


Share of returns driven by fit and sizing up to 70% of apparel returns, same dynamic in footwear[Fittingbox](https://footwear.fittingbox.com/en/blog/footwear-return-rate-benchmarks-and-key-metrics-to-track)


Overall average ecommerce return rate for comparison roughly 20%[Richpanel, ecommerce return rate benchmarks](https://www.richpanel.com/learn/ecommerce-return-rates)


Footwear is structurally rigid and interacts with real foot mechanics, so a shoe that's slightly off in length, width, arch height, or toe box isn't a minor inconvenience, it's unwearable. When the product page can't rule that out in advance, shoppers "bracket": ordering multiple sizes or widths with the intent of returning most of them. That habit is now mainstream, and it turns a data gap on the page into a fulfillment and reverse-logistics cost on the balance sheet.


## A before-and-after, on one shoe


Here's the pattern in practice, using a typical running shoe raw feed versus what a shopper (or an AI agent) actually needs to answer "will this work for me."


Attribute Raw feed value Enriched value


Size 10 US 10 / UK 9 / EU 44, true-to-size


Width (blank) Standard (D), wide (2E) available


Drop (blank) 8mm heel-to-toe


Stack height (blank) 32mm heel / 24mm forefoot


Upper material "mesh" Engineered knit mesh, breathable, non-waterproof


Best for (blank) Road running, neutral pronation, daily mileage


Break-in (blank) True to size out of box, minimal break-in


The raw version isn't wrong. It's just too thin to survive contact with a real decision, human or machine.


## Why 2026 raises the stakes


Two things changed the math this year. First, marketplace and Shopping feed requirements got stricter, not looser. Missing GTINs, unstandardized size fields, or blank required attributes now get products suppressed rather than merely down-ranked. Second, and more consequential: shopping increasingly routes through AI agents that never see your product page's design, only your structured data.


Google's Shopping Graph, which now indexes tens of billions of listings and refreshes at a pace of roughly 2 billion listings per hour, pulls from Merchant Center feeds and Schema.org markup to power AI Mode recommendations ([Paz.ai, Google Merchant Center for AI Mode](https://www.paz.ai/guides/google-merchant-center-for-ai-mode) ). When feed data and on-page markup disagree, or when either is incomplete, Google's guidance is to deprioritize the listing rather than guess. ChatGPT's shopping surfaces and Perplexity's shopping results work the same way in practice: they read the feed, not the vibe of your homepage.


Ask an AI shopping assistant to "recommend a wide-width trail running shoe with a low drop for a size-11 flat foot," and it will filter on exactly the attributes most footwear feeds leave blank. A shoe that's a perfect match but untagged for width and drop simply won't surface. The AI agent doesn't infer fit from a lifestyle photo. It reads structured fields, and if they're empty, the shoe isn't a candidate.


## The fix isn't a bigger PIM


None of this requires ripping out a footwear brand's PIM or commerce platform. The problem was never storage, it's that nobody is continuously checking, standardizing, and filling the attributes that actually predict fit across every SKU, every brand, every season.


That's the specific gap Anglera closes. It plugs into whatever PIM or catalog system a footwear retailer already runs, scores every listing against the attributes that drive both conversion and AI-agent visibility (width, last, drop, size-standardization, material), and continuously gap-fills and corrects them so the catalog stays complete as new styles and sizes land. Your PIM still stores the data. Anglera does the work of keeping it right.
