---
schema_version: "1.0.0"
document_id: "e73ee23643f3b5d64a5b1d7da1c116fec29041ff60779d00cbfd5b571e52ea24"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/product-content-stack-six-layers"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T17:36:10.661019+00:00"
fetched_at: "2026-07-31T17:36:17.979173+00:00"
content_hash: "sha256:9190aa358985d19476d8f8b99fb787d38412eddab5f5b74ffed0427ed4c3e091"
---

# The product content stack has six layers. Most teams shop the wrong one.

There is a specific conversation I have had enough times to recognise it from the first sentence.


A distributor or a retailer has decided their product data is a problem. They have a shortlist. On it: a PIM, an AI copywriting tool, and a feed management platform. They want to know which one to pick.


The honest answer is that those three products solve three unrelated problems, and the shortlist was assembled from search results rather than from a diagnosis. It is the equivalent of shortlisting a plumber, an electrician and a roofer because the house feels wrong.


## Six layers, one vocabulary


The product content market looks like one category because every vendor in it uses the same forty words. It is really six, and they stack:


Layer What it does Representative vendors


**Storage** Models, governs and versions product data Akeneo, Salsify, inriver, Stibo, Pimberly


**Production** Creates attribute values that don't exist yet Anglera, Trustana, Pumice.ai, BPO providers


**Distribution** Moves finished content to trading partners Syndigo, 1WorldSync, Icecat, content pools


**Channel shaping** Reformats per destination and its rules Feedonomics, Productsup, Channable, Rithum


**Expression** Writes prose from structured data Hypotenuse, Jasper, Writer, PIM-native tools


**Measurement** Grades the result and ranks what's broken Profitero, DataWeave, NIQ, Salsify Insights


Every one of these is a real category with real leaders, and each of them will describe itself as solving "product content." They aren't lying. They're describing their own layer, and the buyer is the only person in the conversation who has to work out which layer they're standing in.


We wrote up the full vendor set for each of them in a set of[market maps](https://www.anglera.com/best) — including the categories where the answer is somebody other than us, because a map that only leads to one place isn't a map.


## The diagnosis takes an afternoon


You can identify your layer from the shape of the failure, not from the shape of the vendor's demo.


**Two teams have different values for the same SKU and nobody knows which is right.** That's storage. You need a system of record, and you need it before anything else, because the other five layers all assume one exists.


**The fields are simply empty.** Thirty thousand SKUs with no material, no dimensions, no certification, because nobody ever typed them in and the supplier's PDF is a scan. That's production, and it is the layer most often misdiagnosed as storage, because it is the one that looks solved in every demo — demo catalogs are complete.


**A retailer keeps rejecting your submissions.** That's distribution. Their schema, their validation rules, their network.


**Google disapproves 8% of your items every week.** Export the reasons and sort them. If they're mostly missing[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) , missing size, missing colour — that's production surfacing at the channel-shaping layer. A feed rule can rename` colour` to` color` . It cannot invent a colour that no field records.


**The specs are complete and the copy reads like a parts list.** That's expression, and it's the cheapest problem on this list to fix.


**You suspect content is costing you money and can't prove it.** That's measurement, and it's often the right first purchase, because it converts a vague complaint into a ranked queue with a number attached.


## The mistake that costs the most


By some distance, it is buying storage to solve production.


It's an easy mistake to make. PIM vendors demo beautifully. The data model is elegant, the workflow is clean, the governance is genuinely good, and the catalog on screen is complete. Nobody in the room notices that the completeness was a property of the demo data rather than of the software.


Eighteen months and a seven-figure programme later, the catalog is in a much better system and the[attribute fill rate](https://www.anglera.com/glossary/attribute-fill-rate) has moved by two points. Not because the PIM failed — it did exactly what it says on the tin. Because filling in 400,000 empty fields was scoped as a phase inside the migration, and when the timeline slipped, the phase without a dedicated owner is the one that got deferred to next year.


A PIM stores product data. It does not go and find it. Those are different verbs and they need separate budgets.


## The other mistake: generating over nothing


The second most expensive error runs the stack in the wrong order — buying expression before production.


Point any description generator at a SKU that reads` BR120 · Eaton · circuit breaker` and it will return a confident, fluent paragraph about amperage, mounting style and typical application. Some of it will be right. The model cannot mark which parts it read and which it inferred, because from inside the generation there is no difference between the two.


An empty description field is a visible gap that someone can be assigned to fix. A confident, wrong description is a silent liability that syndicates to every channel you're connected to. In B2B, a wrong thread pitch doesn't read as a typo. It reads as a returned pallet and a call to your rep.


Fill the attributes, then write. It is a slower first month and a materially different year.


## What a coherent stack looks like


For a mid-size distributor, the shape that works is usually:


1. **A PIM** as the system of record, sized to the catalog rather than to the sales deck
2. **An enrichment practice** — not a project — producing sourced attribute values into it continuously
3. **A feed or syndication layer** appropriate to how you actually sell
4. **Generation** derived from the enriched record, canonical first and per-channel second
5. **Measurement** with enough remediation capacity behind it that the queue can actually be worked


The word doing the work in that list is *practice* . Catalogs don't hold still. Suppliers revise specs, new SKUs arrive weekly, certifications lapse, channels change required fields. A one-time backfill produces a completeness number that peaks the day it lands and decays from there — which is why the same organisation runs the same cleanup project every few years, usually with a different vendor and the same outcome.


## Where we sit, and where we don't


Anglera is layer two. We build the attribute schema a category actually needs, fill it SKU by SKU against supplier documents and how buyers in that category search, cite where each value came from, and write the result back into whatever system you already run. Implementation lands around 30 days because there's no front end to replace.


We are not a PIM and have no ambition to become one — we work alongside Akeneo, Salsify, Syndigo, inriver, Pimberly and the rest. We're not a content network, not a feed manager, and not a digital shelf platform. If your problem is one of those, the[market maps](https://www.anglera.com/best) name the vendors who lead each one, and we'll say the same thing on a call.


The problem we do own is the one nobody's software solves on its own: the values were never captured, the team that would capture them doesn't exist, and every system downstream is only as good as the completeness underneath it.


If you're not sure which layer you're in, bring one category you're losing in. That diagnosis is free and it takes about an hour — and if the answer is that you need a feed tool, we'll tell you which one.
