---
schema_version: "1.0.0"
document_id: "2697bd574318333ab5930f1324536c4c1e6dc796aa7c98f5d971ee4d062d3bb1"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/sporting-goods-demand-forecasting"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:4cab8df6491885451c5087f79e39fbbc33fdac6b9a22d0d184ceeeca998907aa"
---

# Demand forecasting in Sporting Goods: the attribute layer your models are missing

A buyer orders next spring's trail running line in June, working off a model that has never seen the actual shoe. The style is new. The forecast isn't guessing at a number so much as borrowing one, pulled from whatever the system decides counts as "similar." In sporting goods, where a meaningful share of every season's assortment is new colorways, new silhouettes, or genuinely new tech, that borrowing step happens constantly. And it's only as good as the attributes it borrows on.


This is the part of demand planning that gets the least attention. Everyone talks about the model, the algorithm, the ML platform. Almost nobody audits what the model is actually summing over.


## A forecast is an aggregation, not a prediction


Strip away the math and a demand forecast is a rollup: units by style, by size curve, by region, by channel, aggregated up from history and pushed back down to a buy plan. Every rollup needs a dimension to group on, and in sporting goods those dimensions are rarely just "category" or "brand." They're things like cushioning tier, insulation type, flex rating, or sport-specific fit. If those fields are missing, inconsistent, or buried in a marketing paragraph instead of a structured field, the rollup groups items that don't belong together and forecasts drift from day one.


Consider three attributes that are specific to sporting goods and specific to breaking forecasts when they're thin:


- **Footwear stack height and midsole compound.** Two shoes tagged "running shoe" can differ by 15mm of stack height and use entirely different foam chemistries (EVA versus a supercritical foam like PEBA). Those differences drive who buys the shoe, at what price, and in what season, but if the attribute lives in a product description instead of a normalized field, the forecasting model can't distinguish a max-cushion daily trainer from a low-profile racing flat. It just sees "running shoe."
- **Insulation fill power and fabric weight in outerwear.** A 700-fill jacket and a 900-fill jacket at the same GSM shell weight serve completely different climate zones and price points. Roll them into one "insulated jacket" bucket and a forecast built for a temperate-market assortment will misfire hard in a market that actually needs heavier fill.
- **Equipment flex rating and dimensional specs.** A ski's flex index and turn radius, or a bat's barrel diameter and drop weight, determine skill level and buyer segment as much as brand does. Free-text specs like "medium-stiff flex, great for intermediate skiers" can't be aggregated. A numeric flex rating field can.


None of these are edge cases. They're the attributes that make a sporting goods SKU a sporting goods SKU, and they're exactly the fields most likely to arrive as loose text in a spec sheet PDF or a legacy ERP notes field rather than a clean, structured, comparable value.


## Cold start is the norm, not the exception


Sporting goods brands compete partly on newness. Innovation cadence is one of the through-lines in[McKinsey and WFSGI's 2025 sporting goods industry report](https://www.mckinsey.com/industries/retail/our-insights/sporting-goods-industry-trends) , which points to slowing category growth and intensifying competition from smaller, faster-moving challenger brands, and argues that visible product innovation is one of the clearest differentiators between winners and everyone else. Constant newness is good for the top line and brutal for forecasting, because every "new" SKU starts with zero sales history.


The standard fix is a like-item or analogous-series approach: find historical SKUs that resemble the new one and borrow their demand curve, then adjust as real sell-through comes in.[Research on forecasting new product trial with analogous series](https://www.sciencedirect.com/science/article/abs/pii/S0148296315001460) has used exactly this logic for decades, and it depends entirely on picking the right analogues. Amazon's own forecasting team found that making product metadata an explicit input to cold-start models, rather than an implicit signal, produced[forecasts up to 45% more accurate](https://aws.amazon.com/blogs/machine-learning/generate-cold-start-forecasts-for-products-with-no-historical-data-using-amazon-forecast-now-up-to-45-more-accurate/) than earlier neural approaches. The lift didn't come from a better algorithm. It came from giving the algorithm cleaner, more complete attributes to match on.


If a new trail shoe's stack height, drop, and midsole compound are sitting in structured fields, a similarity engine can find the three closest historical analogues and borrow a demand curve with real confidence. If those specs are trapped in a hundred-word product paragraph, the engine falls back to matching on category and price band, which is a much blunter tool, and the initial buy is a guess wearing a forecast's clothing.


## Where thin attributes show up in the P&L


The consequence isn't abstract. It shows up as markdown pressure and stranded inventory, the two metrics that planning teams already track obsessively.[Toolio's guide to retail demand forecasting](https://www.toolio.com/post/demand-forecasting-in-retail-methods-tools-and-tips) notes that poor forecast accuracy pushes retailers toward reactive buying, deeper markdowns, and lower service levels, a pattern that compounds every season a brand keeps launching new styles without cleaning up how it describes the ones it already sells.


Attribute state What the model sees Forecast behavior


Structured, validated (stack height: 32mm, fill power: 800) Precise similarity match to true analogues Tighter initial buy, faster reforecast after early sell-through


Free-text ("plush cushioning, great warmth") Category and price band only Broad, generic analogue set, higher initial error


Missing entirely No usable signal Forecast defaults to category average, worst-case error


None of this argues for a new forecasting platform. Most planning teams already have one, and it's doing roughly what it was built to do. The gap is upstream: the attribute layer those tools read from is often thinner, staler, or more free-text than anyone planning a buy realizes. This is the layer Anglera works on. It plugs into whatever PIM, ERP, or flat file already holds the catalog, extracts structured values like stack height, fill power, and flex rating from spec sheets and tech packs, flags conflicting source data instead of silently picking one, and backfills a new attribute across a full catalog in days rather than the 30-45 minutes per SKU manual enrichment typically takes. Your planning system still does the forecasting. Anglera just makes sure it isn't forecasting on guesses.
