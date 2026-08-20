---
schema_version: "1.0.0"
document_id: "602dc699955c57d828c0e4f41d1d816071531008094e9e2b5ced4e27b15b9bde"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/on-shelf-availability"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-24T00:54:45.437387+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:6b5b65b884d47df501153f245f6510b7f923d1cfcf917ff386a274b2300919d9"
---

# On-Shelf Availability & Out-of-Stocks

[On-shelf availability](https://www.cpgscout.ai/glossary/on-shelf-availability) (OSA) is the percentage of time a SKU is present and purchasable at the shelf during store hours. An[out-of-stock](https://www.cpgscout.ai/learn/on-shelf-availability-benchmarks-2026) (OOS) event is any moment that percentage drops to zero: the product is gone, a shopper who wants it leaves empty-handed or buys a competitor, and the sale is lost for good. OSA is one of the most closely watched retail execution metrics in CPG because, unlike a pricing miss you can correct next week, a shelf gap is a permanent revenue leak.


This post covers what OSA and out-of-stocks actually mean, why they are deceptively hard to measure accurately, how to calculate lost sales from first principles, how modern detection methods work, and what levers brands and retailers can pull to close the gap. It is written for brand managers, field sales teams, and retail analysts who work with store-level sell-through data.


## What Is On-Shelf Availability and What Counts as an Out-of-Stock?


The formal definition: OSA = (hours product is available on shelf) / (total store open hours), expressed as a percentage. A product that is out of stock for 4 hours in a 12-hour trading day has an OSA of 67% for that day. Averaged across all stores and all days in a period, you get a chain-level OSA rate.


An out-of-stock is not the same as a distribution gap. Distribution tracks whether a SKU is authorized and listed at a store. OSA tracks whether it is physically on the shelf right now. A product can have 90% ACV distribution and still be out of stock in hundreds of locations on any given day because the backroom supply ran out, a pallet was not worked, or the shelf tag went missing and no one restocked.[POS Data](https://www.cpgscout.ai/blog/pos-data) can capture OOS signals in ways that distribution reports cannot, because it reflects what actually scanned at the register.


### Intermittent vs. chronic out-of-stocks


Intermittent OOS events happen at individual stores on specific days, often around weekends or promotional spikes when replenishment cycles are not fast enough. Chronic OOS is a structural problem: the DC is under-allocated, the planogram sets an insufficient facing count, or the forecast driving replenishment is consistently low. Intermittent gaps are execution problems. Chronic gaps are planning problems. They have different root causes and different fixes.


## Why Out-of-Stocks Hide in the Data: Phantom Inventory


Phantom inventory is the core reason retail out-of-stock detection is so difficult. The retailer's inventory management system shows units on hand, but the shelf is empty. The disconnect between the system record and physical reality is more common than most people expect, and it causes the replenishment algorithm to skip the store entirely because it believes stock is adequate.


Phantom inventory builds up through a few reliable mechanisms: items are received but scanned to the wrong location, returns are logged as restocked when they went into salvage, cycle counts are infrequent or skipped, and theft or damage is not immediately deducted. In high-velocity categories with fast turns, a phantom unit count of even 2 or 3 units can suppress an auto-replenishment order for days.


### Why phantom inventory is a brand problem as much as a retailer problem


Retailers own the inventory system, but brands feel the sales impact. If your item has phantom inventory at 150 stores across a chain, you may see a POS velocity dip that looks like a soft promotional response or a seasonal lull. The real cause is that shoppers could not find the product. Brands that do not have store-level sell-through data have no way to separate a demand problem from a supply problem. Brands that do have that data, and can align it with[Share of Shelf](https://www.cpgscout.ai/blog/share-of-shelf) metrics, can identify phantom inventory pockets much faster.


Inventory scenario System shows Shelf reality Replenishment triggered? POS signal


Normal in-stock 12 units 12 units on shelf No (stock adequate) Normal scan rate


True out-of-stock 0 units Empty shelf Yes (auto-order fires) Zero scans


Phantom inventory 8 units Empty shelf No (system thinks fine) Zero scans (looks like OOS)


Backroom not worked 20 units Empty shelf, full backroom No Zero scans


Partial fill 4 units 1-2 facings left Maybe (below par) Reduced scan rate


The table above shows why a zero-scan signal in POS data is ambiguous. It could mean true depletion, phantom inventory, a backroom that was never worked, or even a product that was temporarily moved for a reset. Context from inventory records, perpetual inventory audits, and shelf image data is needed to distinguish them.


## Measuring Lost Sales from Out-of-Stocks


Lost sales from OOS events are real revenue that does not appear anywhere in your sell-through report. You have to estimate them. The standard approach uses a baseline velocity calculation.


### Baseline velocity method: a worked example


Take a 12-oz hot sauce SKU selling at a mid-size grocery chain. In the four weeks before a promotional event, the SKU averages 3.2 units per store per week across 420 stores. During the second week of the promotion, 60 stores go out of stock for an average of 4 days each.


During a promotion, velocity is elevated. Assume the promotional lift factor is 2.2x based on the first week's observed lift. The expected daily rate per store during the promo = (3.2 x 2.2) / 7 = approximately 1.0 unit per store per day.


Lost sales estimate = 60 stores x 4 days x 1.0 unit/store/day = 240 units. At a retail price of $5.49, that is about $1,320 in lost retail sales across the chain for one week of OOS at 60 stores. At typical CPG margins, the brand cost is $400 to $500 in net revenue. That is from one SKU, one chain, one promotional week.


Scaled to a national brand with 8,000 promoted stores and an OOS rate of 12% during key promotional windows, the arithmetic gets painful quickly. Industry estimates put total CPG lost sales from OOS in the range of 4% to 8% of potential revenue in promoted periods. For a $50 million brand, that is $2 to $4 million per year sitting on the floor instead of the shelf.


### Adjusting for substitution and switching


Not every OOS event translates one-for-one into a lost sale. Research across categories finds that roughly 30% to 40% of shoppers facing an OOS will buy a competitor product, another 20% to 30% will visit a different store or purchase online, and only 30% to 40% will defer the purchase entirely. For brands with strong loyalty, the deferral and switching rates are worse: the loyal buyer may come back, but the casual trial shopper is likely gone. Estimating lost sales precisely requires knowing your brand's switching profile in the category, which comes from shopper panel data or household purchase history.


## Out-of-Stock Detection: POS Signals and Image Recognition


Two primary methods are used at scale to detect retail out-of-stocks: POS zero-scan analysis and shelf image recognition. They work best in combination.


### POS zero-scan signals


The most accessible detection method uses the fact that a store selling zero units of a normally active SKU on a given day is almost certainly experiencing either an OOS or a phantom inventory condition. A zero-scan flag is triggered when a store-day observation shows no movement for a SKU that has a baseline velocity above a set threshold, typically 0.5 to 1.0 units per day.


The algorithm needs a few refinements to be useful. It must account for store closures (holidays, remodels), low-velocity tail items that legitimately sell zero on many days, and planned delistings. Once those are filtered out, the remaining zero-scan days are strong OOS candidates. When POS data is reviewed at the store level, analysts often find that OOS events cluster around the same day of the week (typically Thursday or Friday before weekend replenishment) or the same stores in a chain (those at the end of delivery routes or with chronic understaffing).


### Shelf image recognition


Shelf image recognition (computer vision applied to in-store photos) has expanded significantly as an OOS detection tool since the early 2020s. Field reps, retailer associates, or purpose-built shelf-scanning robots capture images of the shelf. A trained model identifies empty facings, misplaced SKUs, and facing count against the authorized[planogram](https://www.cpgscout.ai/blog/what-is-a-planogram) .


The advantage of image-based detection over POS zero-scan is that it catches the problem earlier. A POS zero-scan on a Tuesday morning reflects an OOS that may have started Sunday afternoon. An image captured Sunday afternoon catches it at onset. The disadvantage is coverage: at most retailers, image capture is not continuous, and a brand can only guarantee images were taken if their own field team or a third-party service (Wiser, Trax, Shelfgram, etc.) visited every store in the window.


### Combining signals


Combining POS velocity drops with inventory level signals from the retailer portal produces the most reliable OOS flags. When the POS scan rate drops below 50% of baseline AND the inventory record shows fewer than two units on hand, the probability of an actual shelf OOS is above 85% in most categories. Brands with access to harmonized[POS Data](https://www.cpgscout.ai/blog/pos-data) feeds can automate this flag at the store-SKU level and route alerts to field teams or broker reps within 24 to 48 hours of the event starting.


## Reducing Out-of-Stocks: Practical Levers


Fixing OOS requires identifying whether the failure is in forecasting, replenishment ordering, distribution center allocation, or in-store execution. Different root causes need different responses.


Root cause Signal in data Typical lever Who owns it


Forecast too low for promo OOS starts day 2-3 of promo, spreads chain-wide Increase forward buy / pre-position inventory Brand supply chain


Phantom inventory Perpetual inventory high, zero scans, no auto-order Request cycle count; correct system record Retailer ops + brand rep


Planogram facing count too low Chronic OOS on same SKU across stores, especially weekends Submit planogram change request with velocity data Brand category management


Backroom not worked Inventory record healthy, but zero scans Field rep / broker escalation; shelf reset Retailer store ops


DC allocation cut OOS across entire region, store-level inventory near zero Work with retailer buyer on allocation increase Brand sales + retailer buyer


Delivery route frequency OOS clusters at end-of-route stores on same weekday Negotiate more frequent delivery or shipment increase Distributor / brand logistics


The most common mistake brands make is treating every OOS as a field execution problem when the actual cause is upstream. Sending a field rep to fix a shelf gap caused by an under-allocated DC order is wasted labor. Getting the root cause right requires store-level analysis of both inventory and scan velocity data before dispatching field resources.


### Facing count and shelf placement


A SKU with one facing depletes twice as fast per facing as a SKU with two facings, given the same velocity. Adding a facing is often the most durable fix for chronic OOS on high-velocity items. The business case is straightforward: present the retailer with store-level velocity data, the average OOS days per month, and the lost-sales estimate. Retailers respond to revenue math. If the data shows a facing increase recovers $200 per store per month in lost sales, a category manager can justify the space reallocation. Connecting OSA data to[Share of Shelf](https://www.cpgscout.ai/blog/share-of-shelf) analytics strengthens that case further, because it quantifies exactly how much shelf space competitors are capturing during your gaps.


### Promotional forecasting adjustments


Promotional OOS is the most preventable category. Historical lift factors by retailer, by promotion type (TPR, feature, display), and by season are available from POS history. A brand that knows its July Fourth promotion drives a 2.8x lift at a regional grocery chain but only 1.6x at a club account should be pre-positioning inventory differently for each. Many brands use a blanket lift assumption across accounts, which under-positions inventory at the high-lift accounts and over-positions at the low-lift ones. Store-level POS history, when properly harmonized across different data sources, is what enables that account-specific calibration.


### Scout and automated OOS detection


Scout surfaces OOS and lost-sales signals by ingesting store-level POS feeds alongside[syndicated data](https://www.cpgscout.ai/blog/what-is-syndicated-data) , then flagging stores where velocity drops below a configurable threshold. Instead of manually reviewing weekly sales reports, brand analysts can see OOS candidates ranked by estimated lost revenue and route them directly to field teams. The value is mostly in speed: catching a phantom inventory event on day 2 instead of day 10 recovers sales that would otherwise be written off as a demand softness.


## Key Benchmarks and What Good Looks Like


Across all FMCG categories, average OSA at major grocery retailers runs between 91% and 95% at the chain level, meaning 5% to 9% of SKU-days are out of stock at any given store. High-velocity, low-facing SKUs in impulse categories (gum, candy, single-serve beverages) tend to fall below 90%. Slower-moving specialty items sometimes look better on OSA but suffer more from phantom inventory because cycle counts are less frequent.


For branded CPG with national distribution, an OSA above 97% during non-promoted periods is achievable and is the internal target many large manufacturers set. During major promotional events (holidays, big game, back-to-school), 94% to 95% is more realistic and still regarded as strong performance. If your chain-level OSA falls below 90% in a promoted period, the lost-sales impact is large enough to materially affect period revenue.


## Frequently asked questions


What is the difference between on-shelf availability and in-stock rate?They are often used interchangeably, but there is a subtle distinction. In-stock rate typically refers to whether inventory exists somewhere in the store or supply chain (backroom, DC, or shelf). On-shelf availability specifically measures whether the product is on the selling shelf and purchasable by a shopper at that moment. A store can be "in stock" in the backroom while having zero on-shelf availability if no one has worked the replenishment.


What is phantom inventory and why does it cause out-of-stocks?Phantom inventory is when a retailer's system records units as available, but the physical shelf is empty. It causes out-of-stocks because the automated replenishment system sees adequate inventory and does not trigger a reorder. Common causes include receiving errors, theft not deducted from inventory, returns logged incorrectly, and infrequent cycle counts. Detecting phantom inventory requires comparing[POS Data](https://www.cpgscout.ai/blog/pos-data) zero-scan signals against the inventory record: if both show no movement but system inventory is positive, phantom inventory is likely.


How do you estimate lost sales from an out-of-stock event?The baseline velocity method is most common: calculate the store's average daily scan rate during a comparable in-stock period, then multiply by the number of OOS days. For promoted periods, multiply the baseline by the expected lift factor before applying it. For example, a SKU averaging 0.8 units per day per store, out of stock for 5 days at 80 stores, produces a lost sales estimate of 320 units at retail price. The actual revenue loss depends on whether shoppers switched to a competitor (roughly 30 to 40% do) versus deferring the purchase.


What is the best method for out-of-stock detection at the store level?Combining POS zero-scan analysis with inventory level data gives the most accurate detection. A store showing zero POS scans for a normally active SKU, combined with low or suspicious inventory records, is a strong OOS signal. Shelf image recognition from field visits or retail scanning services (Trax, Wiser, etc.) can detect the gap earlier but requires physical coverage. The fastest practical approach for brands without a large field team is automated POS alerts tied to a velocity threshold, routed to broker reps or merchandisers within 24 to 48 hours.


How does on-shelf availability connect to planogram compliance?Planogram compliance directly affects OSA. A planogram that authorizes one facing for a high-velocity SKU creates a structural OOS risk because the shelf depletes faster than replenishment cycles can refill it. When OOS events cluster consistently at the same SKU across a chain rather than at individual stores, inadequate facing count on the[planogram](https://www.cpgscout.ai/blog/what-is-a-planogram) is often the root cause. Brands can make a data-driven case for additional facings by presenting lost-sales estimates and comparing their velocity-per-facing against category benchmarks.
