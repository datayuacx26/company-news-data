---
schema_version: "1.0.0"
document_id: "22f96211a604f6194e601577bbd930f00cbe257b9fb3addff5cebc3839ac1fe1"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/apparel-demand-forecasting"
published_at: "2026-05-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:12:51.498055+00:00"
content_hash: "sha256:a412b0f61effceca695f1d0d8cb4d42206fdf5f22735ee08f7ab864296a7e92f"
---

# Demand forecasting in Apparel: the attribute layer your models are missing

A forecast for an apparel SKU that has never shipped a unit is not really a forecast. It is a guess dressed up in a model. Every planning team handles this the same way: find something in the catalog that already sold, and borrow its curve. The entire exercise lives or dies on how well "something similar" is defined, and that definition is built entirely out of[product attributes](https://www.anglera.com/glossary/product-attributes) . If those attributes are thin, inconsistent, or written in free text, the model isn't wrong because the math is bad. It's wrong because it was never given real dimensions to match on.


That matters more in apparel than almost any other category, because apparel forecasting has two problems stacked on top of each other: short selling seasons and a catalog that turns over so fast that most of what you're forecasting has no history at all.


## Newness is the default state, not the exception


In grocery or CPG, forecasting mostly means predicting more of the same. In apparel, a large share of any season's assortment is new: new colorways, new silhouettes, new fabrications replacing last year's styles that are already being marked down. Academic work on the problem frames it plainly — most demand forecasting research assumes a long sales history to learn from, but fashion retail is dominated by short life cycles and constant newness, which is exactly the condition classical forecasting methods handle worst ([Journal of Forecasting, 2025](https://onlinelibrary.wiley.com/doi/full/10.1002/for.3192) ). That's not a niche edge case for apparel planners. It's the baseline.


The industry's answer, increasingly, is attribute-similarity forecasting: match a new style's color, silhouette, fabric, and price point against the historical performance of items that share those characteristics, then start the new item's forecast from that analog curve instead of from zero. Vendors building this into fashion planning tools report real accuracy gains from doing it well — one industry writeup cites 20-40% improvement in new-product forecast accuracy from attribute-based matching versus flat or judgment-only starting forecasts ([Cart.com](https://cart.com/blog/ai-predictive-inventory-forecasting-fashion-brands) ). The mechanism is intuitive: a relaxed-fit organic cotton crewneck in a new seasonal colorway should inherit demand shape from other relaxed-fit crewnecks, not from the brand's average SKU.


The catch is that this only works if "relaxed fit" means the same thing every time it's typed. It rarely does.


## Where apparel attributes actually break


Three fields do most of the damage, because they carry the most forecasting signal and are also the ones most often left to whoever built the PDP that week.


**Fit and silhouette.** A denim line might carry skinny, straight, relaxed, wide-leg, and bootcut across a single season. If those show up in source data as "skinny," "Skinny Fit," "slim-skinny," and a blank field for the newest style, the model can't tell that three of those are the same demand cohort and the fourth needs a manual override. Rollups by silhouette silently split what should be one signal into four thin, noisy ones.


**Fabric composition and weight.** A cotton-poly blend jersey tee behaves differently in demand than a heavyweight fleece, and GSM (grams per square meter) is often the single best predictor of season-of-sale timing for a top. When composition is captured as a vague marketing description ("soft, breathable fabric") instead of a structured percentage and weight, the model loses the variable that would have told it this SKU is a fall layering piece, not a summer basic.


**Closure and rise.** For denim and outerwear, rise (high-rise, mid-rise, low-rise) and closure type (button-fly, zip-fly, snap) correlate strongly with which customer segment buys the item and how it trends season to season. Freehand product copy tends to mention these inconsistently or not at all on older SKUs, which means the historical base a new item should match against is smaller and noisier than it needs to be.


None of this is abstract. It shows up directly in the numbers apparel finance teams already track. Online apparel return rates run 25-40% of orders, roughly double the all-ecommerce average, and a large share of those returns come back out of season by the time they're restocked, turning a healthy 55% gross margin into something closer to 42% once markdown and reverse-logistics costs are counted ([Eightx](https://eightx.co/blog/apparel-financial-benchmark) ). Fit and sizing confusion is one of the largest drivers of apparel returns, and fit is exactly the attribute most likely to be inconsistent across a catalog. Poor rollups don't just cost forecast accuracy; they show up as returns, markdowns, and margin leakage downstream.


## What a clean attribute layer changes about the rollup


Attribute field Free-text as captured Normalized value Forecast impact


Fit "slim-skinny," "Skinny Fit," blank` fit: skinny` Merges cohort, thickens the analog pool


Fabric "soft, breathable fabric"` composition: 60% cotton / 40% poly` ,` weight_gsm: 180` Enables season-timing signal


Rise Not captured on older SKUs` rise: mid-rise` Restores denim-segment comparability


Closure Inconsistent, sometimes in title only` closure: zip-fly` Cleaner trend read across seasons


Turnover discipline reinforces the same point. Public apparel companies average 2-3 inventory turns a year against a healthy DTC target of 4-6, and brands that do hit higher turnover cut markdown rates by roughly 15% because faster-moving stock needs less discounting to clear ([StyleMatrix](https://stylematrix.io/inventory-turnover-in-fashion-benchmarks-analysis-and-turn-rate-strategies/) ). Better turnover starts with a tighter buy, and a tighter buy starts with a forecast that isn't guessing at what a new style actually is.


None of this argues that AI forecasting is a solved problem for apparel, or that a model can predict a trend out of thin air. It argues something narrower and more useful: the accuracy ceiling on any forecasting or planning tool is set by the attribute layer underneath it, not by the algorithm on top. Anglera doesn't replace the PIM, ERP, or planning platform a team already runs. It sits alongside them, extracting and normalizing attributes like fit, fabric composition, weight, and closure straight from tech packs, imagery, and existing catalog fields, flagging conflicts instead of guessing, so that when a planning team asks a new SKU to borrow a demand curve, it's borrowing from the right cohort.
