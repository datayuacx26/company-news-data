---
schema_version: "1.0.0"
document_id: "4b5d2c572559681666596cfc6e4e02baf7caa1ee410cf93729fb7c543010b580"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/retail-void-analysis"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-06T16:30:12.013653+00:00"
fetched_at: "2026-08-06T16:30:13.622759+00:00"
content_hash: "sha256:b8f07034b53f22a0381810440f08f9add9d18d3758e4e66b74a5a7c3e92c5f8b"
---

# Retail Void Analysis: Find the Stores Not Selling

A void is a store that is authorized to carry your item and is not selling it. Retail void analysis is the work of finding those stores, deciding which ones are genuinely broken, and putting a dollar figure on each so somebody can act in priority order rather than alphabetically.


It is the highest-return analysis available to most brands, for an unglamorous reason: the distribution is already paid for. You won the listing, you funded the free-fill, and some percentage of the doors never sold a unit. That percentage is usually larger than anyone expects.


## Key takeaways


- Authorized, shipped, set, and selling are four different states. Void analysis is the gap between the first and the last.
- Zero units does not mean void. Separate never-sold, stopped-selling, and genuinely slow before anyone drives anywhere.
- Size each void at the median velocity of comparable stores, not at chain average, or the list ranks by store size instead of by opportunity.
- Run it weekly. A void found in week 3 is fixable; the same void found at the line review is a cut.


## The four states between a listing and a sale


Most brands track the first state and assume the fourth. The middle two are where the losses live.


State What it means Where you see it


Authorized The chain approved the item for these stores Buyer's authorization list


Shipped The distributor or warehouse sent it to the store Distributor order and shipment data


Set The item is physically on the shelf with a tag Store visit, or inferred from first sale


Selling Units are ringing at the register Retailer POS or syndicated store-level data


A brand authorized in 412 doors that ships to 384 and rings a first sale in only 371 has 41 stores that have never sold a unit. Nothing in a standard sales report says so, because a standard sales report shows the stores that sold.


## Separating a void from a slow seller


Zero units in a store this week can mean four different things, and they call for four different responses. Sorting them is the analytical core of void analysis.


- Never sold. Authorized, but no unit has ever rung. Almost always never set, or set in the wrong section. This is the highest-value bucket and the easiest to fix.
- Stopped selling. Sold consistently, then went to zero and stayed there for three or more weeks. Usually an out-of-stock that became a delist, or the item lost its tag during a reset.
- Intermittent. Sells in some weeks and not others, at a rate below one unit per week. Frequently a genuine slow seller in a small store, and sometimes a chronic out-of-stock. Check whether the zero weeks cluster after a promotion.
- Genuinely slow. Sells every week at a low rate. Not a void. Leave it alone or address it as an assortment question.


The three-week rule matters. A single zero week in a store selling two units a week is noise, and treating it as a void produces a list nobody can work. Requiring three consecutive zeros against a store with prior sales cuts the list by more than half and raises its hit rate sharply.


## Sizing each void so the list ranks itself


A void list sorted by store name is a list nobody works. Sorted by dollars, it becomes a route.


The right benchmark is the median velocity of comparable stores, not the chain average. Comparable means similar total store volume and similar banner, because a 3.1 units per store per week average across a chain is meaningless when the top quartile does 6.8 and the bottom does 1.2. Sizing a small-format void at the chain average overstates it by 3x and pushes it above genuinely larger opportunities.


Worked example: a 16 oz fermented salsa, 412 authorized Sprouts doors, four-week window. 63 stores with no movement. Of those, 41 never sold a unit, 14 stopped after selling, and 8 are intermittent. The 41 never-sold stores sit in banners whose comparable median is 2.6 units per store per week. At a $4.21 wholesale, four weeks of 41 stores at 2.6 units is roughly $1,795 in lost wholesale revenue for the period, and about $23,300 annualized if nothing changes.


That annualized figure is what gets a field visit funded. The weekly number rarely does, which is why void analysis reported only as a weekly count gets ignored.


## Working the retail void analysis list


Never-sold stores go first, and they go to whoever can physically check the shelf, because the answer is nearly always that the item is in the back room or was never cut in. Stopped-selling stores go to the buyer or the distributor rep, because the cause is usually an order or authorization problem rather than a shelf problem. Intermittent stores get watched for another cycle before anyone spends money on them.


One caution worth stating: a void list is only as good as the authorization list it is built against. Authorization lists go stale, stores close, and a chain may deauthorize a subset without telling you. Reconcile the list quarterly or you will spend a season chasing stores that are not supposed to carry you.


## How Scout runs this


Scout reads your retailer POS and distributor data, holds the authorization list, and classifies every non-selling store into the four buckets weekly. Each void is sized at the comparable-store median rather than the chain average, and the output is a ranked list with the annualized dollar figure attached, which is the form that actually gets acted on. The related[distribution gap calculator](https://www.cpgscout.ai/tools/distribution-gap-calculator) does the top-line version of the same arithmetic if you want to size the opportunity before building anything.


For the broader measurement frame this sits inside, see[retail execution software](https://www.cpgscout.ai/retail-execution-software) and[on-shelf availability](https://www.cpgscout.ai/blog/on-shelf-availability) .


## Frequently asked questions


What is a retail void?A store that is authorized to carry your item but is not selling it. The most common causes are that the item was never physically set on the shelf, was set in the wrong section, or went out of stock and was never reordered.


How is void analysis different from out-of-stock tracking?Out-of-stock tracking usually looks at items that were selling and stopped. Void analysis includes stores that never started, which is typically the larger and more fixable bucket after a new listing.


How many voids are normal?It varies by channel and by how recently the item launched, but a newly listed item with 10 to 20 percent of authorized doors never ringing a sale in the first eight weeks is common. Established items with a stable authorization list run much lower, and a sudden rise usually means a reset went badly.


Can you do void analysis without store-level data?Not properly. You need store-level POS or syndicated data with an authorization list to compare against. Chain-total sales cannot tell you which doors are missing, and a market-level report averages the problem away.
