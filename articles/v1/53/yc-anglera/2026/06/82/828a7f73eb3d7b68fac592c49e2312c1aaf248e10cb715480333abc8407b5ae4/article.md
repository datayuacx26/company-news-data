---
schema_version: "1.0.0"
document_id: "828a7f73eb3d7b68fac592c49e2312c1aaf248e10cb715480333abc8407b5ae4"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/fill-rate-vs-accuracy"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:9a5b74d3074ba9e1408aaa7298a5354bc818b496126ff937c7ecdde8f2555cec"
---

# 95% complete, still wrong: why fill rate isn't data quality

A merchandising director we'll call typical pulls up the quarterly data quality dashboard: sleeve length, 95% filled. Fabric weight, 91%. Fit, 98%. Every KPI is green. Six weeks later, a rebuy recommendation for "short-sleeve knit tops" undershoots by a third, because a meaningful chunk of the SKUs tagged short-sleeve are actually three-quarter or long-sleeve styles, mislabeled at intake and never checked again. The field was full. It just wasn't true.


This is the trap sitting inside almost every[data governance](https://www.anglera.com/glossary/data-governance) program in retail and distribution: completeness and accuracy are different properties, measured differently, owned by different people, and only one of them shows up on the scorecard.


## What fill rate actually measures


Fill rate answers one question: is there something in the cell. It is cheap to compute, easy to trend, and satisfying to report because it only ever moves in one direction as a catalog matures. A[field is complete](https://dqops.com/master-data-management-vs-data-quality/) when all required data is present in the record. Nowhere in that definition is a check against reality.


Accuracy answers a harder question: does the value match the real-world product. That check requires a source of truth outside the record itself, imagery, a spec sheet, a lab test, a customer who actually wore the thing, and a process to compare the field against it. Most PIM and MDM tooling was built to enforce the first property. Almost none of it was built to enforce the second, because the second requires domain judgment the system doesn't have.


Master data teams know this gap exists. It's one reason poor data quality still[costs organizations an average of $12.9 million a year](https://www.atrocore.com/en/blog/master-data-quality-management) , and why a recent IBM study found data problems near the top of operating leaders' concerns even at companies that consider their master data "governed." Governance disciplines who can edit a field and when. It rarely disciplines whether the edit was correct.


## Nobody owns correctness


Ask a data governance lead who owns the fabric composition field and you'll get an answer: merchandising, or product development, or whoever loaded the PIM at launch. Ask who verified that a specific SKU's fabric composition is right, today, and the answer gets vague fast. The team that owns the field structurally is rarely the team with product-level knowledge to judge a specific value, and the team that could judge it, buying, design, QA, has no workflow that routes values back to them for a second look.


That's the structural reason "95% complete" persists as a metric while "95% correct" almost never appears anywhere. Nobody is positioned to compute the second number, so nobody reports it, so leadership manages to the number that exists.


Three failure patterns show up constantly once you go looking:


- **Attribute contradicts the asset.** The spec field says short-sleeve; the product photo shows long-sleeve. The description was likely written from an early tech pack, and the garment changed in a late sample revision that never made it back into the PIM.
- **Copied from a sibling SKU.** A new colorway or a new season style gets seeded from the closest existing item to save time, and half the spec carries over unchanged, including attributes that should have changed with it, like weight, fabric blend, or care instructions.
- **Legacy values nobody revisits.** A field was populated correctly for a product line that no longer exists in that form, and every successor SKU inherited the value by default because updating it wasn't anyone's job.


None of these show up as a blank cell. All three pass a fill rate check with a perfect score.


## Why this corrupts planning specifically, not just the PDP


An e-commerce filter that shows the wrong sleeve length costs a handful of bad add-to-carts and some returns. A forecast built on the same wrong field is worse, because forecasting aggregates. A demand plan for "short-sleeve knit tops" sums every SKU tagged that way into one series, and a like-item forecast for a brand-new style leans on the sales history of items sharing its attributes, a technique demand planners increasingly rely on precisely because[new products lack their own history](https://www.toolsgroup.com/blog/how-ai-powered-demand-forecasting-transforms-new-product-introductions/) . If the attribute used to find those analogs is wrong, the new item gets grouped with the wrong comparison set, and the forecast inherits someone else's demand curve.


The output doesn't look broken. It looks like a normal plan with a normal-looking confidence interval, wrong for a reason that never surfaces in a forecast accuracy review, because the review checks whether the number was hit, not whether the inputs describing the product were true. Multiply that across an assortment and it compounds with global[inventory distortion already running around $1.73 trillion a year in lost sales and excess stock](https://www.ihlservices.com/news/analyst-corner/2025/09/retail-inventory-crisis-persists-despite-172-billion-in-improvements/) , a good share of which traces back to plans built on the wrong picture of what's actually in the warehouse.


## From filled to verified


State What it tells you What it misses


Blank field Nothing was captured Everything


Filled field Something was captured Whether it's correct


Verified field Captured value matches an independent source Little, if the sources are good


Turning "filled" into "verified" means cross-checking the text value against something other than the record itself. Does the sleeve-length field match what the product photo shows. Does the fabric weight in the PIM match the number on the actual spec document or lab test. Does the fit description match what reviewers repeatedly say about true-to-size or runs-small. Where two sources disagree, the disagreement should be flagged for a human to resolve, not silently overwritten by whichever source ran last.


That's a mechanical process, not a philosophical one: extract a candidate value from imagery, tech packs, and review text; compare it against the field on record; score the agreement; route mismatches to a queue instead of letting them sit. It's also the layer most PIMs and ERPs were never built to run, because they're built to store a value, not to interrogate whether it's still true.


That's the specific gap Anglera works: it plugs into whatever PIM, MDM, or flat file already holds the catalog and cross-validates attributes against imagery, spec documents, and reviews, flagging conflicts instead of hiding them behind a fill rate number. The point isn't a prettier dashboard. It's giving planning teams attributes they can aggregate on without silently baking someone else's mislabeled SKU into next quarter's buy.
