---
schema_version: "1.0.0"
document_id: "a38265adc3b262e584f386a768c97d070aa44c82dc5d17731e540f128ee538f7"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/trade-merchandising-measurement"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-24T00:54:45.437387+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:fd34b58d84e6a9c69d7e1db62a65d960975e930dd8290ac403d02aba4f3c9cd4"
---

# Trade Merchandising: Feature, Display & TPR

Trade merchandising refers to the set of in-store (and circular) vehicles a retailer uses to support a product during a promotional period: Feature ads, Display placements, and TPRs (temporary price reductions). Each vehicle type signals something different to shoppers, costs the brand a different amount, and produces a different sales lift profile. Knowing which vehicle drove your volume is table stakes for any honest[trade promotion evaluation](https://www.cpgscout.ai/blog/trade-promotion-effectiveness) .


This post defines each vehicle, explains how Circana and NielsenIQ report them in weekly[syndicated data](https://www.cpgscout.ai/blog/what-is-syndicated-data) , walks through how to read %ACV with merchandising, and shows a worked example of lift attribution. It is written for brand managers and insights analysts who pull weekly POS reports and need to explain results to sales leadership.


## What Trade Merchandising Measurement Covers


When a brand runs a[trade promotion](https://www.cpgscout.ai/blog/what-is-trade-promotion) , the retailer executes it through one or more merchandising vehicles. Syndicated data providers track those vehicles at the store level, then aggregate them into %ACV (percent of All-Commodity Volume) metrics. %ACV tells you what share of total retail dollar volume was sold through stores that ran a given vehicle in a given week.


For example, if your item scanned 42% ACV with Feature, that means stores accounting for 42% of the market's total category dollars ran a Feature ad on your item during that period. It is a size-weighted measure, not a simple store count, which matters because a single large-format Kroger division can be worth 8-10% ACV on its own.


The core metrics you will encounter are:


- % ACV with Feature (sometimes "% ACV with Ad")
- % ACV with Display
- % ACV with TPR
- % ACV with Any Merchandising (the union of all three, plus combo vehicles)


Each one is reported independently, so you can see Feature without Display, Display without TPR, and so on. The Any Merchandising flag is useful as a quick read on overall promo reach, but it can mask important differences in vehicle mix that affect lift magnitude.


## Feature (Ad): The Retailer Circular Vehicle


In CPG syndicated data, a Feature refers to a retailer advertisement, typically a printed circular, digital feature, or in-app promotion that highlights the product at a specific price point. Feature does not mean a physical in-store display stand. The defining characteristic is that the retailer communicates the item and price proactively to shoppers before or during their trip.


Feature ads are expensive to earn. Most major grocers sell feature pages on a pay-to-play basis, and front-page or front-cover placement commands a significant premium over back-page mentions. For a mid-tier natural brand, buying a single regional feature page across a chain like Albertsons might require a per-store allowance in the range of $0.25-$1.00, multiplied across hundreds of stores.


Feature typically generates the highest incremental volume lift of any single vehicle, because it drives trip-planning behavior. Shoppers see the price and add the item to their list before they arrive at the store. Typical Feature lift for a snack or beverage SKU runs 150-400% above baseline, depending on price depth and brand equity. Niche or emerging brands tend to see the lower end of that range.


### Feature in Circana and NielsenIQ


Circana captures Feature flags at the store-week level by collecting retailer ad flyers and digital feeds. NielsenIQ uses a similar combination of retailer-provided data and field audits. The flag is binary at the store-week level (either an item was featured or it was not), then rolled up to %ACV at the account, market, or national level for your weekly POS pull.


One common reporting nuance: some retailers run feature ads mid-week rather than on a Sunday cycle. If your data panel week runs Sunday-Saturday and the feature breaks on a Wednesday, you will see partial-week lift that can look like a soft feature. Always check the retailer's ad cadence before diagnosing a weak Feature response.


## Display: Secondary Shelf Placement


A Display is a secondary product placement in the store, separate from the item's home shelf location. Classic examples are end-caps (end of a grocery aisle), floor stands, power wings, freestanding racks near the checkout, and pallet displays in club channels. The defining characteristic is physical presence in a high-traffic secondary location.


Display captures impulse shoppers who were not already planning to buy the item. A shopper walking past a carbonated beverage end-cap sees your new energy drink stacked three cases high and decides to try it. That visit would not have been captured by a Feature alone because the shopper never saw the ad.


Display lift tends to be meaningful but lower than Feature in most grocery categories, roughly 80-200% above baseline for a typical snack or beverage item. In club and mass channels, Display lift can be much higher because the secondary placement is often the primary in-store discovery mechanism.


### How Display Is Measured


Unlike Feature, Display is captured primarily through field audits and store-check data rather than ad-feed scraping. Circana's retail measurement service sends auditors into stores on a rolling basis to record whether items have secondary placements, and at what store count. This introduces a lag and some coverage noise: a display that went up on Monday might not be recorded until the Thursday store check.


For brands that rely on a direct-store-delivery (DSD) network, Display compliance is a critical execution metric. Your broker might sell in 200-store end-cap placement across a regional chain, but if only 140 stores actually build the display, your %ACV with Display will be proportionally lower than the trade plan projected. That gap is a direct hit to ROI.


## TPR: Temporary Price Reduction


A TPR (temporary price reduction) is a shelf price reduction that runs without any accompanying Feature ad or Display placement. The price drops on the shelf tag, sometimes with a "Sale" flag, but the retailer does not actively advertise the item in a circular or give it a secondary location. TPR is the most common and often least efficient merchandising vehicle.


TPRs are common because they are operationally easy for retailers to execute and require minimal commitment. They are also commonly funded by trade spending: a brand offers a per-case scan-down allowance, the retailer passes some or all of it through to the shelf price, and the result shows up in syndicated data as a TPR week. The brand may not have explicitly requested a TPR. It received one because that is how the retailer chose to deploy the allowance.


TPR lift is generally the lowest of the three vehicles, often in the 30-100% range for grocery, because it only reaches shoppers who were already in that aisle and noticed the price difference. It does not change trip planning or capture impulse traffic. For price-sensitive categories like paper goods or private-label-adjacent items, TPR lift can be higher because the category is highly elastic.


One important distinction: a TPR without Feature or Display is sometimes called a "pure TPR" or "TPR only." Syndicated data will flag these separately from Feature+TPR or Display+TPR combos, which is relevant when you are trying to understand whether the price reduction alone was driving volume or whether the retail support was doing the heavy lifting. This distinction matters significantly for[trade promotion effectiveness](https://www.cpgscout.ai/blog/trade-promotion-effectiveness) analysis.


## How Promos Are Reported in Circana and Nielsen


Both Circana and NielsenIQ report merchandising in a consistent hierarchy of vehicle combinations. The standard reporting buckets are:


Vehicle What It Is Typical Lift Direction Primary Driver


Feature only Retailer ad/circular, no display or price reduction High (150-400%+) Trip planning, pre-store awareness


Display only Secondary shelf placement, no ad or price reduction Moderate (80-200%) In-store impulse, aisle interception


TPR only Shelf price cut, no ad or display Low-moderate (30-100%) Price-aware shoppers already in aisle


Feature + Display Ad plus secondary placement in same week Highest (200-500%+) Pre-trip intent plus in-store reinforcement


Feature + TPR Ad plus price reduction High (150-350%) Trip planning plus price incentive


Display + TPR Secondary placement plus price reduction Moderate-high (100-250%) Impulse purchase plus price signal


Feature + Display + TPR All three vehicles active Very high (250-600%+) Full promo execution


Any Merch Union of all vehicles above Varies Aggregate reach metric


The "Any Merch" metric is useful for a quick read on what percentage of your sales volume ran through some form of promotion in a given period. But it is not a substitute for vehicle-level analysis. A brand with 60% ACV Any Merch could have very different economics depending on whether those stores ran Feature+Display or TPR-only.


Retailers also sometimes run a Price Reduction flag that differs slightly from TPR. Circana typically uses a price-index threshold to determine whether a week qualifies as TPR (commonly a price 5% or more below the item's 52-week average shelf price). Be aware of this when comparing year-over-year numbers after a price increase: after a list price increase, a "normal" promotional depth might not trigger the TPR flag until the base price index recalibrates.


## Reading Merch Lift


Lift measures how much incremental volume a merchandising activity generated relative to what the item would have sold without any promotion. The standard formula compares promoted velocity (units per store per week, or UPSWW) against a baseline derived from non-promoted weeks.


### A Worked Example


Suppose you are a brand manager for a natural chip SKU. Your item's baseline velocity is 4.2 units per store per week (UPSWW) across 1,800 stores. You run a regional promotion across 400 stores in the Southeast with a Feature+Display execution. In the promoted week, those 400 stores average 17.8 UPSWW.


Incremental lift = (17.8 - 4.2) / 4.2 = 324% lift. The 400 promoted stores sold roughly 5,440 incremental units (13.6 UPSWW incremental x 400 stores) versus a baseline of 1,680 units. That incremental 5,440 units is what your trade spend bought.


Now check the %ACV: if those 400 stores represent 18% of the market's total ACV, your syndicated report will show 18% ACV with Feature+Display. If your total national distribution is 38% ACV, the promo ran in roughly half your distribution, which is a meaningful activation. If it ran in only 12% of your distribution footprint, the trade efficiency picture looks different.


### Watch for Post-Event Trough


Heavy Feature+Display executions often create a post-event sales dip in the weeks immediately following the promotion. Shoppers who bought three bags instead of one have pantry-loaded. Your baseline estimate needs to account for this: if you measure the two weeks after a major event as "non-promoted," your baseline will look artificially suppressed, which will inflate your calculated lift on the next event. Circana's proprietary baseline methodologies attempt to smooth this, but manual analysis is still worth checking.


### Scout and Harmonized Merch Data


One challenge with vehicle-level merch analysis is that Circana and NielsenIQ data sometimes arrive in different formats or on different panel weeks, particularly when a brand pulls from both providers across different retail accounts. Scout harmonizes those feeds so you can see Feature, Display, and TPR metrics alongside velocity and distribution trends in a single view, without manually re-indexing each source's vehicle flags. That matters most when you are comparing a Circana-reported Kroger event against a NielsenIQ-reported Albertsons event and need a consistent lift methodology across both.


## Merchandising Vehicles and Trade Planning


Understanding the historical lift profile by vehicle type is what separates a credible trade plan from a spreadsheet with round numbers. Before you commit to a Feature buy, you need to know what Feature alone has historically done for your item at that account, at what price depth, and in what season.


The distinction between TPO and TPM frameworks also affects how you categorize these vehicles. A Feature may fall under a trade promotion optimization (TPO) model if you are dynamically allocating budget across vehicle types, while a trade promotion management (TPM) system focuses on executing the plan as agreed with the retailer. The[TPO vs. TPM](https://www.cpgscout.ai/blog/tpo-vs-tpm) distinction matters most when you start asking why your Feature lift was below plan, since the answer often lies in whether the execution matched what was contracted.


For brands trying to improve trade ROI, the practical starting point is usually the same: audit whether your TPR-only spending is delivering acceptable lift, and redirect the budget toward Feature or Display where the vehicle-level lift history supports it. Many brands discover that 30-40% of their trade spend sits in TPR-only events with lifts well below what a Feature or Display at the same retailer could generate.


A complete view of this analysis benefits from the kind of cross-retailer, cross-provider data that harmonized syndicated platforms surface. When you can see that your Feature lift at Retailer A averages 280% while Feature lift at Retailer B averages 140%, the implication for next year's trade allocation is immediate. That kind of retailer-level vehicle benchmarking is the foundation of any serious[trade promotion](https://www.cpgscout.ai/blog/what-is-trade-promotion) investment strategy.


## Frequently asked questions


What is a Feature in CPG merchandising?In the CPG context, a Feature (sometimes called an Ad) is a retailer advertisement that highlights a product at a promotional price point. It typically appears in a printed circular, a digital ad, or an in-app promotion. Feature does not refer to a physical in-store display stand. Syndicated data providers like Circana and NielsenIQ capture Feature flags at the store-week level by collecting retailer ad feeds, and report them as % ACV with Feature.


What does %ACV with merchandising mean?% ACV with merchandising (e.g., % ACV with Feature or % ACV with Display) tells you what share of a market's total All-Commodity Volume sold through stores that ran a specific vehicle on your item in a given week. It is a size-weighted measure: a store that does $10M per week in total sales counts for more than a store that does $1M per week. A reading of 25% ACV with Feature means stores representing 25% of the market's total ACV ran a Feature ad on your item.


What is the difference between a TPR and a Display?A TPR (temporary price reduction) is a shelf price cut with no accompanying ad or secondary placement. The price drops on the tag, but the retailer does not actively promote the item. A Display is a secondary shelf location (end-cap, floor stand, power wing) that puts the item in front of shoppers outside its home aisle. Both can run simultaneously or independently. TPR typically produces lower lift than Display because it only reaches shoppers already in the item's home aisle, while Display captures cross-aisle impulse traffic.


How do I evaluate trade promotion effectiveness by vehicle type?Start by pulling your item's velocity (units per store per week) for promoted versus non-promoted weeks, segmented by vehicle. Calculate lift as (promoted UPSWW minus baseline UPSWW) divided by baseline UPSWW. Then compare lift across vehicles: Feature, Display, TPR, and combo events. Most brands find that Feature+Display combos deliver the highest lift but also carry the highest cost, while TPR-only events deliver the smallest lift at a lower cost per event. Use this history to build forward trade plans. The[How to Measure Trade Promotion Effectiveness](https://www.cpgscout.ai/blog/trade-promotion-effectiveness) post covers the full methodology.


What is "Any Merchandising" in syndicated data?"Any Merchandising" (or "Any Merch") is the union of all promotional vehicle flags: Feature, Display, TPR, and any combination of those three. A store qualifies as "Any Merch" if it ran at least one of those vehicles on a given item in a given week. It is a useful summary metric for understanding what share of your volume moved through some form of promotional support, but it does not tell you which vehicles drove the lift. Always decompose Any Merch into individual vehicle types before drawing conclusions about trade efficiency.
