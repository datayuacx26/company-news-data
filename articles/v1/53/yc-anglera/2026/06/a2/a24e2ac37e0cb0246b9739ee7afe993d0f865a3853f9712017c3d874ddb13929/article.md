---
schema_version: "1.0.0"
document_id: "a24e2ac37e0cb0246b9739ee7afe993d0f865a3853f9712017c3d874ddb13929"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/free-text-attributes-break-bi"
published_at: "2026-06-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:3743d294b73ef8919b1bee3435abd8247e3f00c18886e35ff5a7fd1f3164ce99"
---

# Ten spellings of 'short sleeve': how free-text attributes quietly break BI

Pull up "short sleeve" performance across five seasons of a legacy ERP and you will not get one number. You'll get ten:` SS` ,` Short Slv` ,` short-sleeve` ,` ShortSleeve` ,` Short Sleeve` ,` SHORT SLEEVE` ,` Short Slv.` ,` S/S` ,` Sht Sleeve` , and the ever-present blank. Ask a planning analyst how short sleeve did last season and the honest answer is: nobody can say, because the system never treated those ten strings as the same thing.


This is not a hypothetical edge case. It's the default state of any attribute field that started as free text and lived through several years, a few ERP migrations, and more than one data-entry team.


## Why the same value ends up spelled ten ways


Free-text fields degrade for boring, structural reasons, not because anyone was careless.


**No pick list, no constraint.** If a field accepts any string, someone will eventually type a different string for the same concept. Sleeve length, closure type, fit, fabric content — anywhere a human types instead of selects, variance creeps in.


**Bulk imports carry the sins of the source.** A supplier's spec sheet says "Short Slv," last year's catalog says "SS," and a merchandiser fixing a typo months later types "Short-Sleeve." Every import adds another dialect instead of reconciling with the ones already there.


**Turnover erases tribal knowledge.** The analyst who knew "SS" and "Short Slv" meant the same thing moves on. Their replacement inherits a field with no documented standard and adds a plausible eleventh variant.


**Casing and whitespace are invisible to humans, fatal to machines.**` Short Sleeve` and` Short Sleeve` (trailing space) look identical on screen. To a` GROUP BY` clause, they are two different values — as are` SHORT SLEEVE` and` Short sleeve` in any case-sensitive system.


None of this shows up as an error. Nothing crashes. The catalog looks fine in a product listing page, where a human reads "Short Slv" and understands it instantly. The damage only surfaces in aggregation — the exact place planning teams live.


## Where it breaks: every rollup, every join, every model feature


A forecast is not a raw number. It's an aggregation: total sell-through by attribute, average sell-through rate by sleeve length, week-over-week trend by category and fit. Every one of those depends on the underlying values being the *same string* so the database can group them together.


When "short sleeve" exists as ten values instead of one:


- A rollup query returns ten small, statistically noisy lines instead of one clean trend line — and whichever spelling happens to have the most SKUs looks like the whole story.
- A like-item comparison for a new style silently excludes past styles tagged with a different spelling, so the forecast baseline is built on a fraction of the real history.
- A machine learning feature (say, sleeve length as an input to an assortment or replenishment model) either gets dropped for being too sparse per category, or gets treated as ten separate low-signal categories instead of one meaningful one.
- Buyers manually reconciling a season-end report in a spreadsheet start hand-merging categories — a workaround that has to be repeated, from scratch, every single reporting cycle.


Retail data-quality writeups increasingly flag this pattern: forecasting failures usually trace back to master data and attribute inconsistency, not a bad algorithm ([RELEX Solutions](https://www.relexsolutions.com/solutions/demand-planning-software/) ;[OnePint](https://www.onepint.ai/insights/what-causes-forecast-errors-in-demand-planning) ). Gartner puts a number on the broader problem: organizations lose an average of[$12.9 million a year](https://www.gartner.com/smarterwithgartner/how-to-improve-your-data-quality) to poor data quality, mostly through silent, compounding erosion like this rather than any single dramatic failure.


## Why manual cleanup doesn't survive contact with catalog scale


The instinct is to assign someone to "clean up the sleeve length field." That works for a demo. It does not work for a real catalog.


A retailer or distributor carrying tens of thousands of active SKUs is not cleaning one field once. They're cleaning it every time a supplier feed lands, every time a category is added, every time someone opens a spreadsheet and "helpfully" retypes a value by hand. Manual enrichment of a single attribute runs somewhere in the neighborhood of 30-45 minutes per SKU once research, judgment calls, and QA are included — a pace a governance backlog outruns within a quarter.


And manual cleanup has no memory. Fix "Short Slv" to "Short Sleeve" today and there's nothing stopping tomorrow's bulk import from reintroducing "Sht Sleeve." Without a system that maps every variant to a governed value *before* it lands in the reporting layer, the fix is temporary and the fragmentation is permanent.


## What restores the rollup: governed values plus automated mapping


The fix is not "add a dropdown to the entry form," though that helps going forward. The fix is closing the loop on the years of history that already exist in free text.


That means two things working together:


1. A governed pick list — one canonical value per concept, owned and versioned, not re-typed by whoever is closest to the keyboard.
2. Automated mapping of every historical and incoming variant to that canonical value, so "SS," "Short Slv," "S/S," and the rest all resolve to` Short Sleeve` without a human retyping five years of records by hand.


The mapping step is the one that actually gets skipped, because it's the tedious part — and it's also the part that determines whether "how did short sleeve do last season" gets answered in one query or gets punted to a manual spreadsheet reconciliation.


Legacy variant found in source data Governed value


` SS` Short Sleeve


` Short Slv` Short Sleeve


` Short Slv.` Short Sleeve


` short-sleeve` Short Sleeve


` ShortSleeve` Short Sleeve


` Short Sleeve` (trailing space) Short Sleeve


` SHORT SLEEVE` Short Sleeve


` S/S` Short Sleeve


` Sht Sleeve` Short Sleeve


` (blank)` Short Sleeve *(when confirmed from image or spec sheet)*


Once every one of those rows resolves to a single governed value, the rollup query that used to return ten fragmented lines returns one. The like-item comparison for a new style pulls in the full relevant history instead of a tenth of it. The sleeve-length feature in a forecasting model stops being noise and starts being signal.


This is standard practice in mature product-information-management guidance: a[controlled vocabulary](https://www.anglera.com/glossary/controlled-vocabulary) — one standard term instead of "cot." or "poly fabric" for the same fiber content, one[unit of measure](https://www.anglera.com/glossary/uom-unit-of-measure) instead of a mix of pounds and grams — is what makes filters and feeds reliable in the first place ([WISEPIM](https://wisepim.com/ecommerce-dictionary/product-data-standardization) ). The same discipline that cleans up a storefront filter is what makes a season-over-season rollup trustworthy.


## Where this fits for planning teams


None of this requires ripping out a PIM or a planning system. Your PIM stores the pick list; your planning tool consumes the clean rollups. The gap is in the middle — turning years of inconsistent free text into governed values without a team retyping every SKU by hand. Anglera reads the legacy variants directly out of existing systems and flat exports, maps them to a governed attribute set validated against source documents and imagery, and flags genuine conflicts instead of silently picking a winner. The result is that "how did short sleeve do last season" becomes a query again, not a research project.
