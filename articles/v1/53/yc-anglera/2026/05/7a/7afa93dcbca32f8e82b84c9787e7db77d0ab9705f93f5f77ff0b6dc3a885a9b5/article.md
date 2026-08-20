---
schema_version: "1.0.0"
document_id: "7afa93dcbca32f8e82b84c9787e7db77d0ab9705f93f5f77ff0b6dc3a885a9b5"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/baseline-before-after-product-data"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:c7889782ab7eda5ff864b704d029fbeb4b9136c3a41841ca6b5503cd2bcc5488"
---

# Before and after: how to actually prove an enrichment project worked

"We enriched 40,000 SKUs and conversion went up." That sentence, on its own, proves nothing. It could be true. It could also be seasonality, a merchandising push that launched the same week, or noise. If you need to show finance, category managers, or a CFO that enrichment actually moved the number, you need a comparison that isolates it from everything else happening around it at the same time.


Five methods do this in practice. Each has a failure mode. Here's what breaks them, and how to run each one so the result survives scrutiny.


## Month-over-month: fast, but noisy


Compare PDP conversion, on-site search zero-result rate, or return rate for the 30 days before enrichment against the 30 days after. It's the easiest method to run — pull it straight from Google Analytics 4 or your search platform (Algolia, Bloomreach, Searchspring), segmented to the SKUs you touched.


The confound: a single month is a bad proxy for anything. Paid spend, weather, a competitor's stockout, a pricing change — any of these can swing conversion 10-20% with zero connection to data quality. Treat month-over-month as a screening tool, not a proof point. It catches an obvious win or an obvious problem early. Then you validate with a longer window.


## Year-over-year, controlling for seasonality


Compare the same SKUs in the same calendar window a year apart — March 2025 vs. March 2026, not last month vs. this month. Like-calendar comparisons strip out most seasonal noise (back-to-school, holiday, weather-driven categories) automatically.


The confound: a lot changes in a year besides your enrichment project — pricing, promotions, traffic mix, even the product lineup itself, as SKUs launch and get discontinued. Two guardrails fix this. Hold the SKU list constant, comparing only products that existed and were live in both periods. And pull a category-level index — total category revenue or traffic — as a denominator, so you can separate "the whole category grew" from "these products grew because the data got better." If your enriched SKUs outgrew the category average, that delta is your signal.


## Enriched-vs-not cohorts


Split your live catalog into two groups at a point in time: SKUs that have been through enrichment (complete attributes, corrected specs, quality-scored copy) and SKUs that haven't. Compare conversion, PDP bounce rate, and return rate between the two cohorts over the same window.


No waiting for a "before" period here — you're comparing two populations that already exist. Use a completeness or quality score as the independent variable: bucket products into quartiles (0-25% attribute-complete up through 75-100%) and plot conversion rate by bucket. A clean upward slope from low-completeness to high-completeness buckets is one of the more convincing internal proof points, precisely because you can generate it on demand, any time — not just around a launch event.


The confound: selection bias. Teams tend to enrich their best-selling or highest-margin SKUs first, so the "enriched" cohort was probably already outperforming before anyone touched it. Guard against this by matching cohorts on pre-enrichment baseline — compare each cohort's trailing 90-day conversion rate *before* the project started — or by enriching a randomized sample within a category instead of cherry-picking top sellers.


## Staged rollout


Roll enrichment out by category or brand in waves — electronics accessories in week one, hardware in week two — and track each wave's metrics from its own go-live date. That gives you several smaller before/after experiments instead of one big one. If the effect shows up consistently across waves, that consistency is itself evidence it's not a fluke.


The confound: rollout order is rarely random. Teams start with the categories most likely to show a win, or the ones easiest to enrich first, which inflates early-wave results. Guard against it by comparing each wave's lift against a category that hasn't been enriched yet in that same window — that nets out anything happening store-wide (a site redesign, a traffic shift, a pricing change) that would otherwise get credited to enrichment.


## Holdout groups


The most rigorous option: within a category, randomly assign a subset of comparable SKUs to stay unenriched as a control while the rest get the full treatment. Compare the two groups over the same period. It's the closest thing to a controlled experiment retail data work gets — random assignment means seasonality, traffic mix, pricing shifts, and competitive activity hit both groups equally. You don't model those factors out. They cancel by design.


The confound: holdouts only work if the groups are comparable going in — similar price point, similar historical velocity, similar traffic source — and large enough that normal week-to-week noise doesn't drown the signal. Plan on at least four to six weeks before drawing conclusions. And it means deliberately leaving some products under-enriched, which is a real cost. Reserve it for a pilot, not your whole catalog.


## Which one to actually run


Method What it shows Main confound Guardrail


Month-over-month Early directional signal Short-term noise (traffic, pricing, weather) Use for screening only, confirm with a longer window


Year-over-year Seasonally-adjusted trend Catalog and pricing changes over a year Hold the SKU list constant; index against category growth


Enriched-vs-not cohorts Real-time completeness-to-conversion relationship Selection bias (best sellers enriched first) Bucket by quality score; match cohorts on pre-project baseline


Staged rollout Consistency of effect across waves Non-random rollout order Compare each wave to a not-yet-enriched category in the same window


Holdout group Closest to causal proof Sample size, comparable groups Random assignment within category; run 4-6+ weeks


No single method is bulletproof. What actually convinces skeptical stakeholders is layering two: a completeness-vs-conversion cohort read for the ongoing story, plus one holdout or staged-rollout test as the rigorous checkpoint. Track the same core metrics across all of them — PDP conversion, return rate split by reason (damaged/wrong item vs. "not as described"), on-site search zero-result rate, support tickets tied to product-detail questions — so the numbers stay comparable no matter which method produced them.


Here's the part that gets skipped most: teams fix the catalog, move on, and never instrument the comparison that would prove it mattered. Anglera plugs into whatever PIM you run, or none, and keeps completeness and quality scores current as products change. That's what makes an enriched-vs-not cohort or a staged rollout something you can run any time — not just right after a one-time cleanup project.
