---
schema_version: "1.0.0"
document_id: "2121c97eda9480301af676639e9de3d7e3afdc322dd8379bb1648d7259b1bb38"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/beauty-demand-forecasting"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:79686b6cdbc8c822adcd8a29b8d7f8b2f7f360c3645f4f84d2e75d86e5dd2539"
---

# Demand forecasting in Beauty & Cosmetics: the attribute layer your models are missing

A prestige skincare brand launches a vitamin C serum in three sizes and two packaging variants. The planning team has zero sales history for any of them. So the forecast falls back on the oldest trick in the book: find something similar that already sold, and borrow its curve. That trick only works if "similar" means something the system can actually compute. In most beauty catalogs, it doesn't, because the attributes that define similarity — actives concentration, finish, undertone, format — live in marketing copy, not in structured fields a forecasting model can read.


Beauty is a brutal category for this problem specifically because it launches faster than almost any other vertical in retail, at higher SKU density, with less room to be wrong. The industry's own numbers back that up: the global beauty market grew roughly 7 percent annually from 2022 to 2024 and is now cooling to an expected 5 percent through 2030, per[McKinsey's State of Beauty 2025](https://www.mckinsey.com/industries/consumer-packaged-goods/our-insights/state-of-beauty) , with executives citing uncertain consumer spending as the top risk to that growth. Slower growth means less forgiveness for excess inventory sitting in a warehouse because a forecast guessed wrong on a new shade.


## Forecasting is an aggregation problem before it's a math problem


A demand forecast is built by grouping SKUs — by category, by price tier, by "things like this one" — and projecting a rate of sale for the group, then allocating it back down to individual items. Every one of those groupings is only as good as the attribute it's grouped on. If "finish" is stored as a free-text string that includes "matte," "matte finish," "velvet matte," and blank cells, the model can't reliably pool those SKUs into one seasonality curve, even though a merchandiser looking at photos would call them the same thing instantly.


This is why attribute quality shows up as forecast accuracy or forecast error, even though nobody on the planning team touched a single line of model code. The model didn't get worse. The dimensions it aggregates along got noisier.


## Three attributes that quietly wreck beauty forecasts


**Shade and undertone.** Color cosmetics carry the widest SKU-to-demand mismatch in retail. Contract manufacturers typically require minimum order quantities in the thousands of units per shade, so a slow-moving deep-neutral foundation shade can get force-fit into the same production and planning logic as a fast-moving universal shade, according to[Eightx's analysis of beauty brand inventory planning](https://eightx.co/blog/beauty-brand-inventory-planning) . Without a clean, structured undertone and depth attribute, a forecasting model can't tell "this shade underperforms because of a real demand signal" from "this shade underperforms because the whole line is new." It just sees noise.


**Formulation and actives concentration.** A "10% niacinamide serum" and a "2% niacinamide serum" behave like entirely different products in the market, but if concentration lives inside a product title or a bullet point instead of a normalized numeric field, a like-item model treats them as near-duplicates. That corrupts both directions: it borrows the wrong demand curve for a genuinely new formulation, and it dilutes the rollup for the established one.


**Format and size tier.** Full-size, travel/deluxe-mini, refill, and value-size are often the same core SKU family with wildly different velocity and seasonality (mini formats spike around gifting windows; refills track loyalty and replenishment cadence, not launch hype). When format isn't a clean, filterable attribute, planners end up eyeballing which historical items are "close enough" to borrow a curve from — the same manual step a forecasting model is supposed to replace.


## Why this matters more in beauty than in most categories


Beauty runs a structurally high newness ratio. Public beauty brands also carry unusually slow inventory turns relative to healthy consumer businesses: e.l.f. Beauty turns inventory about 2.18 times a year (roughly 168 days on hand) and Olaplex about 2.15 times (roughly 170 days), against a healthy direct-to-consumer benchmark of 4 to 8 turns, per the same Eightx analysis. Slow turns plus constant new-shade and new-formula launches is exactly the combination that punishes bad like-item matching: you're forecasting cold-start products more often, and you're carrying the resulting misses longer before you can clear them.


Market fragmentation is adding to the load.[NIQ's State of Beauty 2025 commentary](https://nielseniq.com/global/en/insights/commentary/2025/state-of-beauty-2025/) points to platforms like TikTok Shop accelerating the rise of indie brands and "dupes" — more entrants, more near-identical formulations competing for the same demand, and more pressure on retailers to classify and compare products consistently across brands that don't share a taxonomy.


## What thin attributes cost, concretely


Attribute state What the forecast does Typical downstream cost


Free-text shade name, no undertone/depth field Groups shades by name string, not by visual similarity Overstock on slow shades, stockouts on fast ones, higher markdown rate


Actives concentration buried in title Treats different-strength formulas as the same SKU family Wrong cold-start curve for new formulations


Format/size not normalized Forecast can't isolate gifting-driven mini/travel demand Missed seasonal spikes, excess full-size safety stock


Missing skin type/concern tags Can't roll up demand by use-case across brands Weak signal for assortment and space planning


None of this requires a better forecasting algorithm. It requires the attributes underneath the algorithm to be structured, consistent, and complete enough for "similar" to mean the same thing to a model that it means to a merchandiser standing in front of a shelf.


Anglera doesn't build forecasts or replace the planning tools that consume this data. It sits ahead of them, extracting and normalizing attributes like shade, undertone, actives concentration, finish, and format directly from tech packs, product imagery, and existing catalog fields, then flagging conflicts instead of silently guessing. Your PIM still stores the data, and your planning system still runs the model. Anglera just makes sure the dimensions underneath both are clean enough to trust.
