---
schema_version: "1.0.0"
document_id: "653e4c67c2b984061854fd7c5e8c86d02020fcc10ea0df200ba09d56d4f114d1"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/equivalized-volume-eq"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-24T00:54:45.437387+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:3002d58a2a8ff56db691532c54082a9ff88943f402852844b3076a75c64493fd"
---

# EQ (Equivalized) Volume & Units Explained

EQ sales, or equivalized sales, restate volume in a single common unit, typically fluid ounces, dry ounces, or a standard equivalent case, so that products in different pack configurations can be compared on an apples-to-apples basis. A 12-pack of 12 oz cans contains 144 oz of beverage; a 6-pack of the same cans contains 72 oz. Raw unit counts say one sold twice as many "units" as the other, even when the same amount of liquid moved. EQ volume collapses that ambiguity.


This post covers how EQ factors are set, when equivalized volume matters more than raw units or dollars, and how to work through a concrete conversion from a mixed-pack assortment. It is written for brand managers and sales analysts who see EQ in[syndicated data](https://www.cpgscout.ai/learn/syndicated-vs-panel-data) reports and want to know exactly what the number means.


## What is equivalized (EQ) volume?


Equivalized volume is a calculated metric that converts each SKU's unit sales into a standardized quantity using a predetermined EQ factor. The factor expresses how many "base units" one sellable unit contains. If the base unit is a 12 oz can, a 24-pack gets an EQ factor of 24, and a single 12 oz can gets an EQ factor of 1.


Syndicated data providers like SPINS, Circana (formerly IRI), and NielsenIQ all carry an EQ volume column alongside raw unit volume and dollar sales. The specific base unit varies by category: beverages typically use fluid ounces, dry grocery uses net weight ounces or pounds, and household products often use sheet counts or fluid ounces depending on the subcategory. When you pull a category report from[What Is Syndicated Data?](https://www.cpgscout.ai/blog/what-is-syndicated-data) , the EQ column is almost always present.


## Why brands normalize to EQ


Three practical problems push analysts toward EQ rather than raw units.


### Pack-size proliferation


Most CPG categories carry a wide pack range at any given retailer. A single beer brand might sell 6-packs, 12-packs, 18-packs, 24-packs, and loose singles all at the same time. If a promotional event drives a temporary shift from 12-packs to 18-packs, raw unit velocity will appear to drop even though total liquid volume moved up. EQ makes the trend line accurate.


### Fair competitive comparison


Brand A leads in raw units because it sells mostly small-count packs. Brand B trails in raw units but leads in volume because it sells large multi-packs. Category managers care about actual throughput, not who has the more fragmented pack architecture. EQ volume surfaces who is actually winning on the shelf.


### Pricing and velocity benchmarks


Price per EQ unit is the standard way to set everyday shelf pricing and to model promotional price elasticity. A brand priced at $0.08 per EQ oz is cheaper than one at $0.10 per EQ oz regardless of what the retail price tag says.[Sales Velocity in CPG](https://www.cpgscout.ai/blog/cpg-sales-velocity) metrics like sales per point of distribution are most meaningful when the numerator is EQ volume, not raw units.


## EQ vs. units vs. cases vs. dollars


These four measures answer different questions. Knowing which one to use in a given analysis prevents common reporting errors.


Metric Definition Best used for Blind spot


Raw units Count of sellable packages sold Facings, inventory planning, reorder quantities Hides pack-size mix shifts


EQ units / EQ volume Units converted to a common base quantity Volume trends, category share, price-per-unit benchmarks Requires knowing the EQ factor for every SKU


Cases Wholesale shipping unit (e.g., 24 retail units per case) Distributor sell-in, warehouse slot planning Case counts vary by item; not consumer-facing


Dollars Retail sales in currency Revenue share, promotional ROI, gross margin modeling Inflation and price changes distort trend comparisons


In practice, a strong category review uses all four. Dollars show revenue share. EQ volume shows consumption share. Raw units show shelf-slot productivity. Cases tie back to distributor sell-in data. The metric you lead with depends on the audience: a buyer cares about dollars and velocity; a supply chain manager cares about cases; a brand strategist cares about EQ share.


## How EQ factors are set


EQ factors are defined by the data provider at the item level and stored in the item master. For beverages, the most common approach is to set the base unit as one fluid ounce. Each UPC gets a factor equal to its total fluid ounce content. A 6-pack of 12 oz cans (72 oz total) gets an EQ factor of 72. A 2-liter bottle (67.6 oz) gets a factor of 67.6.


Some categories use a different convention. Beer and cider often normalize to a 24-unit equivalent case, so a 12-pack gets an EQ factor of 0.5 (half a standard case) and a 6-pack gets 0.25. Spirits may use 750 ml as the base. Before using EQ volume in any analysis, confirm the base unit so you know what a number like "2.3 million EQ units" actually represents.


Providers update EQ factors when a UPC's formulation or fill level changes. A common trap: a brand reformulates from 16 oz cans to 15 oz cans, the item master is updated, and historical EQ volume for the old UPC is not retroactively restated. The apparent volume dip is a measurement artifact, not a real consumption change. This is one reason[WTD, CWW & Other CPG Data Acronyms](https://www.cpgscout.ai/blog/wtd-cww-cpg-acronyms) matter for understanding what a period-over-period EQ comparison is actually measuring.


## A worked example: converting mixed pack sizes to EQ units


Suppose a craft soda brand sells four SKUs at a regional grocery chain during a four-week period. The base unit is one fluid ounce. Here are the raw results:


SKU Pack size EQ factor (fl oz) Units sold EQ volume (fl oz)


Single can 12 oz 1 x 12 oz 12 4,800 57,600


6-pack 12 oz cans 6 x 12 oz 72 3,100 223,200


12-pack 12 oz cans 12 x 12 oz 144 1,400 201,600


2-liter bottle 1 x 67.6 oz 67.6 900 60,840


Total 10,200 543,240


Raw units say the single can is the top-selling SKU with 4,800 units, nearly 47% of volume. EQ volume tells a completely different story: the 6-pack and the 12-pack together account for 78% of actual fluid ounces sold. If you benchmarked velocity or priced against units, you would underweight the packs that drive most of the consumption.


Now suppose a competitor's flagship 6-pack moved 3,900 units in the same period. At a glance, your brand's 3,100 6-pack units look weaker. But your brand's total EQ volume of 543,240 oz vs. the competitor's 3,900 x 72 = 280,800 oz tells the real picture: your brand moved nearly twice the liquid volume when you account for the full assortment.


This is exactly the kind of cross-SKU rollup that becomes error-prone when EQ factors are applied manually. Scout normalizes EQ factors from SPINS and Circana item masters automatically, so the EQ column in any category view already reflects the correct per-UPC factors rather than a rough approximation.


## EQ sales in practice: common pitfalls


### Mixing base units across providers


SPINS and Circana may define the EQ base unit differently for the same category. One may use fluid ounces; the other may use a 24-count equivalent case. If you blend data from two providers without aligning base units, your total EQ volume will be meaningless. Always check the item-master documentation or the "unit of measure" field before combining feeds.


### New items with placeholder EQ factors


When a UPC first appears in the syndicated data universe, the provider sometimes assigns a default EQ factor of 1 until the item master is fully populated. This can make a new 24-pack look like a single unit in EQ volume for the first few reporting periods. Watch for any item showing an EQ factor of exactly 1 when the pack size is clearly larger.


### Using EQ for categories where it doesn't apply


EQ volume is most useful in categories with significant pack-size variation: beverages, condiments, household cleaners, and personal care. In categories where nearly every SKU ships in a single pack configuration (certain confections, single-serve items), raw units and EQ units will be nearly identical, and the distinction is not worth the complexity. Know your category before defaulting to EQ.


## Where EQ fits in a broader sales analysis


EQ volume is one of several normalized metrics analysts use alongside ACV-weighted distribution, velocity, and price per unit. The standard flow for a category review starts with distribution (how many stores carry the item, weighted by ACV), moves to velocity (EQ volume per point of distribution), then drills into price-per-EQ-unit positioning versus competitors. This sequence lets you separate placement gaps from consumption gaps from pricing gaps.


Velocity in particular only makes sense as an EQ metric. "Units per point of distribution" conflates a 6-pack with a 12-pack. "EQ ounces per point of distribution" tells you how much product consumers actually took off the shelf per carrying store, normalized for size. That is the number a buyer benchmarks when deciding whether to expand distribution or cut a facing.


For a fuller picture of how these metrics relate to each other in[What Is Syndicated Data?](https://www.cpgscout.ai/blog/what-is-syndicated-data) , EQ volume is covered as a standard output column alongside dollar sales, unit sales, and distribution measures.


## Frequently asked questions


What does EQ sales mean in CPG?EQ sales stands for equivalized sales. It converts unit sales of different pack sizes into a single common unit (such as fluid ounces) so products with different configurations can be compared on volume rather than package count. A 12-pack and a 6-pack of the same product have different raw unit counts but can be expressed in the same EQ volume.


What is equivalized volume and how is it calculated?Equivalized volume multiplies each SKU's unit sales by its EQ factor, where the factor represents how many base units (e.g., ounces) one retail package contains. For example, if the base unit is 1 fl oz and a SKU is a 6-pack of 12 oz cans (72 oz total), its EQ factor is 72. Selling 1,000 of those packs yields 72,000 EQ oz. Add up EQ volume across all SKUs to get a category or brand total.


What is the difference between unit sales and EQ units?Unit sales count the number of packages sold regardless of size. EQ units (equivalized units) restate that count in a common base quantity. If you sell 500 six-packs and 500 twelve-packs, unit sales are 1,000. EQ units at a per-can basis are 3,000 (500 x 6 + 500 x 12). Unit sales are useful for shelf planning and reorder quantities; EQ units are more accurate for measuring consumption and benchmarking velocity.


What are equivalized units vs. cases?Equivalized units normalize to a consumer base unit (like fluid ounces or a standard equivalent case defined by the data provider). Cases are a wholesale shipping unit, typically a fixed count of retail packages per master carton. They serve different purposes: EQ units support consumer-facing analysis (velocity, share, pricing), while cases support supply chain and distributor sell-in analysis. Some beverage categories define their EQ unit as a 24-equivalent case, which blurs the line, so always confirm the base unit with your data provider.


Why do some reports show EQ volume instead of units as the main metric?Category managers and retailers prefer EQ volume when the assortment has wide pack-size variation because it reflects actual consumption more accurately than raw counts. A shift in pack-size mix (say, consumers trading from 6-packs to 12-packs) can show a false "unit decline" that doesn't reflect a real drop in sales. EQ volume captures the true trend. For a deeper look at how velocity and distribution metrics work alongside EQ, see[Sales Velocity in CPG](https://www.cpgscout.ai/blog/cpg-sales-velocity) .
