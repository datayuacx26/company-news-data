---
schema_version: "1.0.0"
document_id: "bae8c289bee15c9ecd103d1428f9b271fd87dc038c7e89cc838c40ae3b4bd94c"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/cx-is-a-data-problem-2026"
published_at: "2026-06-28T00:00:00+00:00"
first_seen_at: "2026-08-06T19:14:01.668988+00:00"
fetched_at: "2026-08-06T19:14:09.920895+00:00"
content_hash: "sha256:4fa988182c9f6908c5eb94378e4b1f749d9ee7364ca57f2ed02e588cb0347e55"
---

# Most Bad Customer Experience in Distribution Is Just Bad Product Data

Distribution Strategy Group's answer to bad customer experience is more ownership: cross-functional teams, shared scoreboards, everybody accountable. Our answer is narrower and less comfortable: audit the catalog first, because most of what gets logged as a CX failure in wholesale distribution is a product-data failure wearing a CX costume. You cannot coach a rep out of a spec sheet that doesn't exist.


## The complaint and the cause are different things


Walk into any distributor's CX postmortem and the complaints sound like people problems. A buyer called three times before finding the right gasket. A rep quoted the wrong part. An order got returned because the customer thought they were buying something else. Leadership reads this as a training gap, maybe a handoff gap, and reaches for the fix[Distribution Strategy Group argues](https://distributionstrategy.com/2026/07/customer-experience-is-everybodys-job/) for: CX as "everybody's job," owned by cross-functional value-stream teams with a shared scoreboard.


Pull the thread on any of those three complaints and the root cause is usually upstream of any human being. The buyer called because the site's search returned nothing for the part number they typed, or returned forty near-matches with no way to tell them apart. The rep quoted wrong because the product page didn't carry the attribute that would have disqualified it — voltage, thread pitch, material, whatever the spec was. The wrong-item return happened because the listing had one stock photo and a two-line description standing in for what should have been a real spec table. None of that is a culture failure. It's a data failure that happens to surface at the point of human contact, which is why it gets misfiled as a people problem.


## The pre-sale audit we already run


Anglera measures this directly, which forces you to stop describing CX and start scoring it. Our[Digital Readiness Index](https://anglera.com/blog/top-distributors-2026/methodology) scores 200-plus distributors across four pillars, and three of the four are, functionally, a measured audit of the failure modes above. Product Data Depth checks whether a listing has a real name instead of a bare part number, a[manufacturer part number](https://www.anglera.com/glossary/mpn-manufacturer-part-number) and[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) a buyer's system can match against, and enough structured attributes to actually disqualify the wrong part before a human has to. Buyer Answerability checks whether the description answers intent rather than restating the spec table, whether there's more than one photo, and whether the catalog is internally consistent (the gap between a distributor's richest and thinnest product page). Commerce Transparency checks whether stock and price are stated at all, because unstated lead time is one of the most common reasons a B2B buyer abandons a search mid-session.


None of that requires talking to a customer. It's scored off the live page. And it maps almost exactly onto the three complaints above: the call because search failed is a Product Data Depth problem; the wrong quote is a Buyer Answerability problem; the ambiguous-description return is both. The pre-sale CX experience, in other words, is measurable without a survey. Where it's bad, it's bad for a specific, fixable reason that has nothing to do with whether the org chart has a Chief Experience Officer.


## Where culture investment hits a ceiling


The part of our data that should worry operators more than any single score is what doesn't move. Median Digital Readiness across the measured index sits at 58 out of 100, a coin flip on a pre-sale CX audit. Break it down by pillar and the gap lands exactly where you'd predict: distributors clear about two-thirds of available points on Commerce Transparency and Machine & Agent Readiness, the pillars closest to "did IT configure this correctly." They clear only about half on Product Data Depth and just under half on Buyer Answerability, the two pillars that are actually a description, an image set, and a spec table. Distributors are more likely to have a working sitemap than a product page a buyer can trust.


Split by archetype and the culture story falls apart from the other direction. Six operating models built on entirely different theories of advantage land within a nine-point band on the index: Program Supplier at 65, Technical Specialist and Scale Aggregator at 60, PE Roll-Up and Branch-Density at 58, Catalog-Native at 56. Catalog-Native is the archetype whose entire pitch is data as moat, and it sits at the bottom of that band. That's the tell: a strategy label doesn't predict execution any better than a service-culture program does. What predicts the score is whether someone actually did the SKU-level work, which is a narrower and less flattering thing to say in a strategy deck than "we're a data-native distributor." Seventeen of the 203 distributors in the index don't even have a public catalog we could measure. That's the most severe version of this failure there is, and it has nothing to do with anyone's attitude toward the customer.


## Where the trade press has a point


To be fair to Distribution Strategy Group, post-sale CX (the returns process, the account manager relationship, how a complaint gets escalated) genuinely is a culture and ownership problem, and their earlier argument that[better CX yields greater profits](https://distributionstrategy.com/2023/10/one-thing-leads-to-another-a-better-customer-experience-yields-greater-profits/) holds up fine downstream of the sale. Gartner's research backs the general direction, too:[67% of B2B buyers now prefer a rep-free buying experience](https://www.gartner.com/en/newsroom/press-releases/2026-03-09-gartner-sales-survey-finds-67-percent-of-b2b-buyers-prefer-a-rep-free-experience) , which means the moments where a human actually does intervene are increasingly the moments that matter most, and those are legitimately about people and process. But an earlier Gartner survey cuts against a pure-culture reading of pre-sale CX:[69% of B2B buyers report inconsistencies](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-sales-survey-finds-61-percent-of-b2b-buyers-prefer-a-rep-free-buying-experience) between what a vendor's website says and what a sales rep tells them. That's a data-integrity problem with a customer-facing symptom, not the reverse.


## The unit economics of fixing it


The fix has a real, boring unit cost, which is exactly why it keeps losing budget fights to "culture." Manually enriching a product record, writing the description, filling the attributes, sourcing a real image, runs roughly 30 to 45 minutes per SKU when a distributor does it by hand, and most catalogs run into the tens of thousands of SKUs. That's the actual reason branch-density distributors under-invest here: not indifference to the customer, but a task that scales linearly against a headcount that doesn't. It's not unique to distribution, either. The[Content Marketing Institute's 2025 B2B benchmark](https://contentmarketinginstitute.com/b2b-research/b2b-content-marketing-trends-research-2025) finds only 41% of B2B marketers even produce technical or data-sheet content, which is precisely the format a buyer needs to self-serve a spec question instead of picking up the phone. Every industry underfunds this. Distribution just feels it faster, because the SKU count is higher and the buyer has a part number in hand, not a vague need. "Everybody's job" framings tend to under-deliver here for the same reason: a catalog with 40,000 thin listings doesn't get fixed by a better scoreboard.


This is the specific gap Anglera works: sitting on top of whatever PIM or spreadsheet a distributor already runs, doing the enrichment work at the SKU level so the buyer-facing failure never reaches a human to escalate. Most operators can see the first honest read of where their own catalog stands, not a survey, a measurement, in the[Top Distributors 2026 index](https://anglera.com/blog/top-distributors-2026) .
