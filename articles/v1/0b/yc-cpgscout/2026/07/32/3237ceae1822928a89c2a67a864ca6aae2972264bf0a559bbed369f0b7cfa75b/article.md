---
schema_version: "1.0.0"
document_id: "3237ceae1822928a89c2a67a864ca6aae2972264bf0a559bbed369f0b7cfa75b"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/trade-promotion-effectiveness"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-24T00:54:45.437387+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:3db4736ecf0fb3e5c058c64fda30f455456eee812631118e9d98f75f77579c7d"
---

# How to Measure Trade Promotion Effectiveness

Trade promotion effectiveness is a measure of whether a promotion returned more incremental margin than it cost. A display at Kroger may move a lot of units, yet still be a money-loser once you account for the trade spend, forward buying, and demand that would have occurred anyway. Measuring effectiveness correctly means separating what the promotion caused from what would have happened regardless.


This post covers every calculation a CPG brand analyst needs: how to set a baseline, how to compute promotion lift and incremental sales, and how to arrive at a defensible ROI or ROMI figure. It also flags the two traps that inflate promo results on paper: pantry loading and cannibalization. A complete worked example with dollar figures is included at the end.


## What "effectiveness" means in trade promotion


[What Is Trade Promotion?](https://www.cpgscout.ai/blog/what-is-trade-promotion) covers the mechanics of trade spend broadly, but effectiveness is a narrower question: did this specific event generate enough incremental margin to exceed its cost? That framing rules out several shortcuts that brand teams often use instead.


Volume sold on promotion is not the same as effectiveness. A brand that ships 10,000 cases during a promotional window may have shipped 8,000 of those cases anyway. The other 2,000 are the only units that the promotion actually caused, and only those units belong in an effectiveness calculation. Mixing promoted volume with base volume inflates apparent results and leads teams to over-invest in tactics that do not pay back.


Effectiveness also has to account for timing effects. Consumers who stock up during a promotional period buy less in the weeks after it ends. If the post-event dip is deep enough, it erodes the incremental units you thought you gained. A narrow view of the event window will therefore overstate true incremental sales.


## Baseline vs. incremental sales: the foundation of trade promotion effectiveness


Every effectiveness calculation starts with a baseline: what would sales have been without the promotion? The baseline is not the prior-period average and it is not the non-promoted store average. Both of those cut corners that compound into large errors.


### How to build a baseline


A sound baseline uses a control group of stores that carried the same item but did not participate in the event, matched on geography, banner, ACV, and historical velocity. You then apply any seasonal or trend adjustment before comparing the promoted stores to the control. The result is a week-by-week expected velocity that serves as your counterfactual.


Where a clean control group is not available, a pre/post design works if you extend the window. Take at least 8 weeks of pre-promotion history, fit a trend line, and project it through the event period. Subtract that projection from actual sales. The gap is your incremental estimate, though it carries more uncertainty than a matched-store approach.


The[Incremental vs. Base Volume in CPG Promos](https://www.cpgscout.ai/blog/incremental-vs-base-volume) post covers baseline modeling in more depth, including how to handle items with seasonal velocity curves.


### Incremental units


Incremental units = actual units sold during the promotion window minus baseline units over the same window. If your item sold 12,400 units across 300 promoted stores in a four-week event, and the baseline projection for those stores over the same four weeks was 9,600 units, you generated 2,800 incremental units. That number is your starting point for every downstream calculation.


## Lift calculation: putting a percentage on incremental sales


Promotion lift expresses the incremental units as a percentage of the baseline. The formula is straightforward:


Lift (%) = (Incremental Units / Baseline Units) x 100


Using the example above: 2,800 / 9,600 = 29.2% lift. Lift is useful for comparing events across different scale points (a test at 50 stores versus a national rollout) because it normalizes for store count and distribution. It is a weaker signal for comparing across categories, since high-velocity items structurally generate lower lift percentages than low-velocity ones.


A few benchmarks help contextualize lift. Temporary price reductions (TPRs) on mid-tier grocery items typically generate 20 to 40% lift. Feature-and-display events often hit 60 to 120%. Digital coupons without in-store support tend to land at 5 to 15%. If a number falls well outside its tactic category, re-examine the baseline before celebrating or panicking.


## ROI and ROMI: translating lift into a financial verdict


Lift tells you how much volume moved. ROI tells you whether it was worth it. The basic formula converts incremental units into dollars and compares that to the cost of the event.


### Promotion ROI


Promotion ROI = Incremental Margin / Trade Spend


Incremental margin is the gross margin you earned on the incremental units only, after accounting for any promotional price discount on those units. Trade spend is the total off-invoice allowance plus any co-op fees, displays, or slotting costs associated with the event.


An ROI of 1.0 means you broke even: every dollar of trade spend generated exactly one dollar of incremental margin. ROI above 1.0 is profitable. Most CPG brands set a hurdle rate between 1.2 and 1.5 for a promotion to be considered effective, though the threshold varies by category margin structure.


### ROMI (Return on Marketing Investment)


ROMI is a variation that expresses the return as a net gain rate rather than a ratio. The formula is:


ROMI = (Incremental Margin - Trade Spend) / Trade Spend


A ROMI of 0.30 means the promotion returned 30 cents in net incremental margin for every dollar spent. A ROMI of -0.10 means you lost 10 cents on the dollar. ROMI and ROI carry the same information; the choice between them is mostly convention. Some finance teams prefer ROMI because a negative number communicates a loss more intuitively than an ROI below 1.0.


## Cannibalization and pantry loading: the two results-killers


Even a correctly calculated lift and ROI can overstate the benefit of a promotion if you ignore demand that was merely shifted rather than genuinely created.


### Cannibalization


Cannibalization occurs when a promoted SKU pulls demand away from other items in your own portfolio. If your 12-pack cola sells 3,000 incremental units during a price event but your 24-pack loses 1,200 units over the same period, the true net incremental volume for your brand is 1,800 units. Running the ROI calculation on 3,000 units overstates results by 67%.


The same logic applies across adjacent categories if your brand has multiple sub-segments. A price reduction on a mainstream SKU can erode premium-tier velocity in the same store. When the promoted item and the cannibalized items carry different margins, the net impact on incremental margin can turn a headline-positive ROI negative.


### Pantry loading


Pantry loading happens when consumers buy more units than they intend to consume at the normal rate because the low price makes stocking up attractive. Those units show up as incremental in the event window, but they suppress post-event demand for several weeks. If you measure a four-week event without looking at the four weeks that follow, you will count pantry-loaded units as genuine new demand.


A simple check: plot weekly velocity for 8 to 12 weeks after the event ends. If weeks 1 through 4 post-event run 15 to 25% below the pre-event baseline, a meaningful share of the event units were pantry loaded. Adjust your incremental count downward by the post-event deficit to get a cleaner true-incremental figure.


## A worked example: full trade promotion ROI calculation


Here is a complete example with realistic CPG numbers. The brand is a mid-tier snack brand running a temporary price reduction plus display event at a regional grocery chain.


Input Value


Promoted stores 420


Event duration 4 weeks


Actual units sold (event window) 31,500


Baseline units (same stores, same 4 weeks) 21,000


Incremental units 10,500


Post-event deficit (weeks 5-8 vs. baseline) 1,800 units


Adjusted incremental units 8,700


Promoted retail price $3.49


Regular retail price $3.99


Brand net revenue per case (24 units) at promo price $28.20


Variable COGS per case $18.90


Incremental gross margin per case $9.30


Total incremental margin (8,700 units / 24 x $9.30) $3,371


Trade spend (off-invoice + display fee) $2,400


Promotion ROI (incremental margin / spend) 1.40


ROMI ((margin - spend) / spend) 0.40


The raw event lifted volume by 50% (10,500 / 21,000). After adjusting for the post-event dip, adjusted lift falls to 41.4% (8,700 / 21,000). The ROI of 1.40 clears a 1.2 hurdle rate, so the event is considered effective. Had the team used the unadjusted 10,500 units, ROI would have been 1.69, which overstates actual performance by 21%.


This example also shows why the volume sold on promotion is not the same as effectiveness. The brand moved 31,500 units during the event. Only 8,700 of them, or 27.6%, were genuinely incremental after adjusting for the post-event trough. The other 22,800 units were base sales the brand would have captured anyway, plus units that simply borrowed from future weeks.


For a deeper treatment of how to classify event outcomes, see[Trade Promotion Analysis: Did the Promo Pay Back?](https://www.cpgscout.ai/blog/trade-promotion-analysis) and[How to Tell If a CPG Promotion Actually Worked](https://www.cpgscout.ai/blog/cpg-promotion-performance) .


## Comparing promotions: a quick-reference tactic table


Tactic Typical lift range ROI risk factors Pantry loading risk


Temporary price reduction (TPR) 20-40% Deep discount compresses margin per unit Moderate


Feature + display 60-120% High display fees offset lift on low-margin items Low to moderate


Digital coupon only 5-15% Redemption below forecast deflates ROI Low


Feature only (no TPR) 15-30% Retailer execution risk; ads may not run as planned Low


Display only (no TPR) 25-50% Placement quality varies significantly by store Low


Buy-one-get-one 80-200% Extreme pantry loading; post-event dip can be severe High


These ranges are illustrative. Actual lift depends on the category, the brand's baseline velocity, the depth of the discount, and retailer execution quality. Teams running events across multiple banners with harmonized syndicated data can benchmark each banner's lift performance against the others to find where the same trade dollar produces the most incremental margin.


## How harmonized data improves trade promotion effectiveness measurement


The biggest obstacle to accurate effectiveness measurement is data fragmentation. A single event may span retailers where you have Circana data, others covered by SPINS, and a few where you rely on retailer POS portals. Baseline models built on one data source often cannot be applied cleanly to another because the velocity definitions, time periods, and store hierarchies differ.


Scout normalizes syndicated feeds, POS, and portal data into a single canonical schema, which means every store in a multi-banner event shares the same baseline methodology. That consistency closes the most common measurement gap: you no longer need separate spreadsheets for each data provider. The ROI calculation in the example above becomes a read-off rather than a weekend project.


## Frequently asked questions


What is a good trade promotion ROI?Most CPG brands set a hurdle rate of 1.2 to 1.5. An ROI of 1.0 means you broke even on incremental margin. Anything below 1.0 means the promotion destroyed margin relative to doing nothing. High-frequency commodity categories with thin margins often need ROI above 1.5 to justify the trade spend, while higher-margin specialty brands can accept lower thresholds.


How do you calculate incremental sales from a promotion?Incremental sales = actual sales during the promotion window minus baseline sales for the same stores and the same period. The baseline is what you would have sold without the promotion, estimated from a control group of non-promoted stores or from a pre-event trend projection. See[Incremental vs. Base Volume in CPG Promos](https://www.cpgscout.ai/blog/incremental-vs-base-volume) for a step-by-step baseline modeling approach.


How is promotion lift calculated?Promotion lift (%) = (Incremental Units / Baseline Units) x 100. If baseline was 9,600 units and actual was 12,400, incremental is 2,800 units and lift is 29.2%. Lift normalizes for scale, making it useful for comparing events of different sizes, but it should always be paired with a margin-based ROI calculation before declaring an event effective.


How do you measure trade promotion ROI?Trade promotion ROI = Incremental Margin / Trade Spend. Incremental margin is the gross margin earned on incremental units only, net of any promotional price discount. Trade spend includes off-invoice allowances, display fees, and co-op advertising costs. An ROI above 1.0 means the event was profitable on a gross-margin basis. For a net-return framing, use ROMI = (Incremental Margin - Trade Spend) / Trade Spend.


What is the difference between ROI and ROMI in trade promotions?Both metrics use the same inputs. ROI = Incremental Margin / Trade Spend and returns a multiplier (1.40 means $1.40 back per $1.00 spent). ROMI = (Incremental Margin - Trade Spend) / Trade Spend and returns a net rate (0.40 means 40 cents of net gain per dollar spent). An ROI of 1.0 equals a ROMI of 0.0; they are mathematically equivalent representations of the same result.
