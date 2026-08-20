---
schema_version: "1.0.0"
document_id: "5e8e2c8a59ed235d088731c3ff58991bb16775974e309a4a4d8bdc4f71155c2c"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/grocery-cpg-demand-forecasting"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:6fd4a0c0937150e850bebb18bdf0ff0b0416b2267494076efc9a3fbbfa5a3220"
---

# Demand forecasting in Grocery & CPG: the attribute layer your models are missing

A new snack SKU launches with zero sales history. The planning system needs a demand curve for it by next Monday's replenishment run. Where does that curve come from? Not from the SKU itself, it has no past. It comes from whatever the system can infer about products like it, and that inference runs entirely on attributes: category, pack size, flavor family, shelf life, private label versus national brand. If those fields are thin, wrong, or buried in a free-text description, the model borrows from the wrong analogs, and the first eight to twelve weeks of forecast, the exact window when a new item either earns permanent shelf space or gets delisted, are wrong from day one.


This is the part of demand forecasting that planning teams talk about least and fight with most. Everyone budgets for better statistical models. Almost nobody budgets for the fact that the attributes those models read are often the last thing anyone cleaned up.


## A forecast is an aggregation, not a single number


Grocery and CPG demand plans are built from rollups: category to subcategory to segment to item, then re-sliced by pack type, channel, and region. Every one of those rollups depends on an attribute being populated the same way across every SKU in the group. If "pack size" is stored as` 12oz` on one item,` 12 OZ` on another, and` 340g` on a third because a supplier fed metric units, the aggregation either silently splits into three buckets or a human has to reconcile it before the forecast run. Neither outcome is what the planner wanted, and both erode the accuracy of the number leadership sees.


The[GS1 US guidance on product data quality](https://www.help.gs1us.org/product-data-quality) exists precisely because this problem is structural to the industry: retailers and manufacturers exchange product data across systems that were never designed to agree on how to represent the same physical fact. Net content,[unit of measure](https://www.anglera.com/glossary/uom-unit-of-measure) , and packaging hierarchy (each, inner pack, case, pallet) all need to be expressed consistently for a forecast to roll up cleanly from case-level shipment data to shelf-level consumption. When those fields drift, the forecast doesn't fail loudly. It just gets quietly less accurate at every level above the SKU.


## Where new items actually get their forecast from


New product introductions are the hardest forecasting problem in the category, full stop. There's no sales history to anchor against, so the system has to borrow a demand curve from something else, usually a set of attribute-similar items with an established sales pattern. This is often called analog forecasting or cold-start forecasting, and the quality of the analog match is entirely a function of attribute completeness.


Consider a plant-based yogurt line extension. A forecasting engine trying to find its analogs needs to know, at minimum: dairy-alternative category, oat versus almond versus coconut base, cup size, flavored versus plain, and whether it's positioned as private label or national brand. If the base ingredient sits inside a free-text product description instead of a structured field ("Made with real oats and probiotic cultures!"), the model can't filter on it, and it defaults to a broader, noisier peer group, maybe every yogurt in the case, not just the oat-milk ones. The resulting forecast is a blend of curves that don't actually resemble the new item, and the retailer either over-orders and eats markdowns or under-orders and stocks out during the exact launch window that determines whether the item survives its first reset.


## Three attributes that quietly break grocery forecasts


**Shelf life and perishability class.** A fresh, short-dated item and a shelf-stable version of a similar product need completely different forecast logic, tighter review cycles, smaller order quantities, markdown timing baked in near the expiration window. When shelf-life data is missing or approximate, planning systems apply generic reorder logic to perishables, which is a direct driver of the waste and forced-markdown problem grocery already struggles with. Recent research on[discounted sales of expiring perishables](https://arxiv.org/pdf/2602.04464) frames this as a core forecasting challenge specific to grocery retail practice, not a generic retail problem, because the cost of a bad forecast compounds daily as product ages.


**Pack size and unit of measure, normalized.** As above: if the same physical count isn't expressed the same way across every SKU in a category, rollups fragment and analogs get missed. This sounds like a minor formatting issue. At scale, across tens of thousands of SKUs from hundreds of suppliers, it's one of the largest sources of silent forecast error, because nobody notices a rollup that's slightly wrong; they just see a number that's directionally plausible and move on.


**Private label versus national brand equivalency.** Store brand and branded versions of essentially the same product (frozen vegetable blend, paper towel count, canned tomato variety) often need to be forecast as related but distinct demand streams, since price elasticity and promotional response differ even when the underlying[product attributes](https://www.anglera.com/glossary/product-attributes) are nearly identical. If that brand-tier flag isn't a clean, consistent field, forecasting tools either merge streams that should stay separate or split streams that should inform each other.


## The accuracy math planners already live with


None of this is theoretical for the teams running these systems. Grocery and CPG planners typically target forecast error (MAPE) in the low double digits for high-velocity items, and new items miss that bar by a wide margin precisely because of the cold-start problem above. Vendors across the[demand forecasting landscape](https://www.relexsolutions.com/resources/demand-forecasting/) , from RELEX to Blue Yonder to o9 to newer AI-forecasting entrants, are all converging on the same conclusion: better models help less than better inputs. A more sophisticated algorithm run against inconsistent pack sizes and free-text ingredient descriptions still produces a forecast built on a fragmented peer group. Garbage attributes in, confidently-wrong rollup out.


Attribute state What the forecast engine does Practical effect


Structured, consistent (oat-milk, 32oz, private label) Matches tight analog set, clean rollup Accurate cold-start curve, right initial order


Free-text or missing (base ingredient buried in a marketing blurb) Falls back to broad category average Blended, noisy forecast, over- or under-stock


Inconsistent units across suppliers (oz vs g vs mL) Rollup fragments or requires manual reconciliation Slower planning cycle, silent accuracy loss


None of this requires ripping out the planning system or the PIM that feeds it. It requires the attributes underneath both to be extracted consistently, validated against source documents like spec sheets and nutrition panels, and kept current as new items and reformulations arrive. That's the layer Anglera works on: pulling structured, quality-scored attributes out of the tech packs, imagery, and legacy fields retailers and CPG brands already have, normalizing them against the categories forecasting and planning tools actually read, and flagging conflicts instead of silently picking a side. The forecast doesn't get smarter. The data it's built on finally holds still.
