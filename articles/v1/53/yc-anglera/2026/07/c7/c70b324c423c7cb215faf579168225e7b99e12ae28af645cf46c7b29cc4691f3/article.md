---
schema_version: "1.0.0"
document_id: "c70b324c423c7cb215faf579168225e7b99e12ae28af645cf46c7b29cc4691f3"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/content-pool-match-rate"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:ed932845a0ca4e48417ddf8b3669b6799031109719dd228a2dbfc263c1f9fa9c"
---

# Match rate: the number your content subscription doesn't advertise

Every content pool demo runs on the same implicit promise: *we have the manufacturers you sell.* The logo wall is real. The SKU counts are real. And none of it answers the only question that matters to your P&L: of the items in **your** item file, how many will actually light up?


Distributors who run the crosswalk find out that "coverage" and "match rate" are different numbers, and the gap between them has a shape.


## Why the overlap disappoints


Think about how each side of the match was built. The pool signs manufacturers in rough order of national demand: the big electrical, plumbing, HVAC, and industrial brands every subscriber sells. Your item file was built by decades of local decisions: the regional lamp brand your contractors ask for by name, the private-label fittings program your buying group set up, the legacy lines from two acquisitions ago, the kits and cut-to-length items that exist only as SKUs in your ERP because you invented them.


The intersection is the national brands — which is precisely the part of your catalog every competitor also carries. The misses cluster in:


- **Private label and house brands.** No manufacturer record exists upstream; nobody but you will ever author this content.
- **Regional and niche lines.** Below the pool's signing threshold, indefinitely.
- **Kits, assemblies, cut goods, repackaged items.** Your invention, your SKU, no upstream source at all.
- **Superseded and legacy part numbers.** Customers still order them; pools carry the replacement, not the number in your customer's reorder file.


Notice what this means strategically: the unmatched tail is not the junk drawer. It is disproportionately the assortment only you carry — the exact SKUs where a buyer *couldn't* comparison-shop you, if only they could find and evaluate the product on your site.


## The crosswalk is a project, then a treadmill


Even inside the overlap, records don't match themselves. The join keys are dirty on both sides:


- **MPN normalization.**` TB-100-1/2` ,` TB100050` , and` TB100 1/2"` are one part. String-equality matching says otherwise. Hyphens, leading zeros, and embedded fractions defeat naive joins in exactly the categories (fittings, fasteners, conduit) with the most SKUs.
- **Identifier gaps.** UPCs missing on the distributor side, or assigned at the case level upstream and the each level in your ERP.
- **Unit-of-measure and pack collisions.** The pool's record describes the manufacturer's sell unit; your item is the box of 50, or the pair, or the 250-foot reel.
- **Supersessions and duplicates.** The pool has the current generation; your file has three generations, all with open order history.


Then it repeats. New line picked up: new crosswalk. Supplier reissues a catalog with renumbered parts: re-match. Pool adds a manufacturer: someone has to notice, map, and reconcile conflicts with the content you already had. Match rate is not a milestone you pass; it's a level you maintain.


## Measure it before you renew


A useful exercise before any renewal, done with a spreadsheet and an afternoon:


1. Export your active, sellable item file — not the full ERP dump, the SKUs that earn revenue.
2. Have the vendor run it against the library and return per-SKU results, not a percentage.
3. Split the result three ways: matched with full attributes, matched but thin (an image and a one-line description is a match on paper and a blank shelf in practice), and unmatched.
4. Weight by your revenue and margin, not by SKU count. A 60% match by line count can be far less by gross-margin dollars once the private-label and specialty lines land in the unmatched bucket.


That margin-weighted number is the honest value of the subscription. It is usually still worth paying for. It is almost never the number from the sales deck.


## What to do with the misses


The matched middle is table stakes — every subscriber has it, which is[its own problem](https://www.anglera.com/blog/same-datasheet-problem) . The tail is where owned enrichment earns its keep: extracting specs from the manufacturer cut sheets and supplier portals the pool never processed, normalizing values into the same governed vocabulary as the rest of your catalog, and writing it all back to your PIM with a citation per value, so the SKUs only you sell finally have content only you have. That work is Anglera's core job, and it's also the piece you own outright regardless of which subscriptions you keep — the full argument is in[the shared content pool trap](https://www.anglera.com/blog/shared-content-pool-trap) .
