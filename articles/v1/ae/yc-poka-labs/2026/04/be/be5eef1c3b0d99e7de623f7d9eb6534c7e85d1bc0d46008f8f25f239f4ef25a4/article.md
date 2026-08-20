---
schema_version: "1.0.0"
document_id: "be5eef1c3b0d99e7de623f7d9eb6534c7e85d1bc0d46008f8f25f239f4ef25a4"
company_key: "yc-poka-labs"
company: "Poka Labs"
source_id: "yc-poka-labs-news-import-f74330f571e0"
canonical_url: "https://pokalabs.com/blog/lead-times-are-pricing-decisions/"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-22T09:41:02.267326+00:00"
fetched_at: "2026-07-28T22:15:36.111958+00:00"
content_hash: "sha256:e3acc9d4515b86829b0b0dd4e00b83f5a60825b9c4197595957f36e15f301c97"
---

# Lead Times Are Pricing Decisions

Quoting a four-week lead time at $X versus a two-week lead time at $X+15% is one of the most consequential pricing decisions your business makes. So why does almost every quoting workflow treat lead time as a logistics constant the rep just copies in?


In most distributors and manufacturers we've worked with, the rep opens the ERP, finds the SKU, copies the standard list price, copies the standard lead time from the inventory record, and pastes both into the quote. The same SKU should not have one price.


## Lead Time Is a Price Lever, Not a Fact


A customer asking for material in 5 days has a fundamentally different problem than a customer asking for material in 5 weeks. Picture a coatings plant whose primary surfactant supplier just declared force majeure—their reactor is sitting idle, every shift down costs six figures, and they need a drop-in substitute in their tank by Friday. Now picture a CDMO planning a Q3 production campaign, working off a forecast their procurement team built six weeks ago. Both customers order the same SKU. Their willingness to pay is not the same. Their substitution options are not the same. Their definition of "winning the deal" is not the same.


When you quote both at the same price, you are doing one of two things, and probably both:


#### Leaving margin on the urgent deal


The customer who needed it yesterday would have paid a real premium for guaranteed delivery. You quoted list. They said yes immediately—because you were cheap, not because they shopped around. The "fast yes" is the most expensive yes you get.


#### Losing the planned deal


The customer with a 6-week planning horizon could have been won with a 4% discount and a longer lead time. You quoted standard. They went with the cheaper competitor who treated lead time as negotiable.


> "Lead time is not a logistics output. It's a pricing input. Treating it as a copy-paste from your ERP is the same mistake as treating list price as the only price."


## The Same SKU, Three Legitimate Prices


Take a single grade of a specialty solvent—say a high-purity acetone used in coatings and pharma intermediates. Three customers ask for the same product, same volume, in the same week. Here is what a lead-time-aware quote looks like:


#### Customer A — Line down


Pull from regional warehouse, ship next day


Lead time


3 days


Price vs. list


+18%


#### Customer B — Standard reorder


Ship from primary warehouse on next milk run


Lead time


2 weeks


Price vs. list


List


#### Customer C — Planning a campaign


Ride next production batch, no expedite cost


Lead time


6 weeks


Price vs. list


−4%


Three customers, one SKU, three correct prices. The 18% premium funds the optionality of regional inventory. The 4% concession wins planned volume that would otherwise go to a competitor. The list price in the middle anchors everything. None of these prices is "the" price.


## Why Regional Inventory Makes This Real


The pricing-versus-lead-time tradeoff is not theoretical. It maps directly to where your inventory sits. We've spent a lot of time inside regional inventory and lead-time models, and the same pattern shows up every time:


-


**Lead time is local, not global** A 3-day lead time is trivial when stock is already in the customer's region. The same 3-day lead time from across the country requires an expedite, an interbranch transfer, or both. The cost to serve is completely different even though the SKU is identical.


-


**Stock position is a perishable asset** Slow-moving inventory in a regional DC is dead capital. Quoting it at a discount with a short lead time turns it into revenue. Quoting it at list with a generic lead time leaves it sitting until next quarter's writedown.


-


**Urgency surfaces in the request, not the SKU** Customers tell you they're urgent—in email phrasing, requested dock dates, willingness to accept partial shipments. None of that ever reaches the ERP. So none of it ever reaches the quote.


-


**Production slack is a discount nobody offers** A future production batch with open capacity costs almost nothing incremental to add a customer to. That's the cheapest order you can fulfill all year. The customer doesn't know that. Most reps don't either.


### The COO Math


Take a distributor doing $200M in annual revenue with average 22% gross margin. Assume even a conservative slice of orders has lead-time flexibility on either end:


25% of revenue is urgent


$50M × 8% recoverable premium ≈ $4M of pure margin


25% of revenue is planned


3% incremental win rate on $50M × 22% margin ≈ $330K of margin


That's roughly $4.3M of margin sitting on the table because lead time is treated as a copy-paste field. The capital is already deployed. The inventory already exists. The only thing missing is the pricing decision.


## Why Reps Can't Solve This Manually


It's tempting to push this onto sales—"just price the urgency in." It doesn't work, and the reason isn't laziness. The information a rep would need to make this call lives in five different systems:


Inventory by location


Which warehouse has it? How much? Reserved or available?


In-transit & inbound


What's on the water? What clears customs this week?


Production schedule


When is the next batch? Is there open capacity to add to?


Customer history


Have they paid premiums before? Pushed back on lead times?


Asking a rep to triangulate all of that under a 24-hour quote SLA, on every single line, is asking them to do a job no human can do at scale. So they default to: copy the list price, copy the standard lead time, send the quote, hope. That default is not a bug in your sales team—it's a bug in your tooling.


## What Lead-Time-Aware Pricing Looks Like


The right system doesn't just spit out one number. It generates a small set of legitimate options for the same line, each with its own lead time and its own price, each backed by where the inventory actually sits:


#### Today's quote


One price per SKU, regardless of urgency


Lead time copied from a static field


No visibility into in-transit or scheduled production


Customer urgency vanishes in the inbox


#### Lead-time-aware quote


Multiple legitimate price/lead-time pairs per line


Sources stock by region, transit, and production


Reads urgency signals from the original RFQ


Sales rep approves; system handles the math


> "Where you stock determines what lead times you can quote. What lead times you quote determines what prices you can capture. Inventory strategy is pricing strategy."


## Where to Start


You don't need a forecasting overhaul or a perfect data warehouse to start treating lead time as a price lever. In the rollouts we've run with distributors, three concrete moves get the first wave of margin recovery in a single quarter:


First, audit a quarter of recent quotes for lead-time signals you ignored. Search inbound RFQs for words like *urgent* , *line down* , *ASAP* , *by Friday* . Compare the prices you quoted to your list. The gap is your premium-recovery opportunity, sitting in plain text.


Second, define an explicit expedite premium and an explicit long-lead-time discount per major product family. Even a static rule ("+12% for sub-week delivery from regional stock; −3% for 6+ week delivery on planned production") beats the implicit rule you have today, which is "always quote list, always quote standard lead time, hope for the best."


Third, govern the floors, not every quote. Pricing managers should approve guardrails—floor and ceiling logic per region, per customer tier, per urgency bucket—and then let the system do the per-line math. The job stops being "what is the price of SKU X" and becomes "what is the price surface of SKU X across lead times, regions, and customer types." That's where the margin is.


From there, the leap to lead-time-aware quoting—where the system sources stock across regions, reads urgency from the RFQ, and proposes multiple price/lead-time pairs per line—is a tooling problem, not a strategy problem.
