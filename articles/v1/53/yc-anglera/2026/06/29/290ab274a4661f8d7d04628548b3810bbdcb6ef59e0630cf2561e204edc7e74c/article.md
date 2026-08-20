---
schema_version: "1.0.0"
document_id: "290ab274a4661f8d7d04628548b3810bbdcb6ef59e0630cf2561e204edc7e74c"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/appliances-attributes"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:b8fe3419cb390bd2eea9c0bdcacc760490de43b964e927a3717b14478ce1643f"
---

# Why appliances products go invisible: the attribute gaps that filter you out

A shopper filters by counter-depth, 20-23 cubic feet, and ENERGY STAR, and your French-door refrigerator never shows up, not because it's a bad match, but because three fields in the feed are blank. Appliances are one of the most attribute-dependent categories in retail: shoppers filter hard, AI shopping agents filter harder, and a product without the right spec fields simply isn't in the running. This is a structural problem, not a copywriting one, and it has a structural fix.


## The attributes that actually gate appliance discovery


Appliance shoppers don't browse. They filter by fit and function, because a refrigerator that doesn't fit the cabinet cutout or the outlet isn't a refrigerator they can buy. That makes a specific set of attributes load-bearing across the category:


- **Physical dimensions** : width, height, depth, and door-swing clearance, often down to the eighth of an inch
- **Capacity** : total cubic feet, and often the fridge/freezer split
- **Depth class** : counter-depth vs. standard-depth, which is its own filter on nearly every major retailer
- **Configuration** : French door, side-by-side, top-freezer, bottom-freezer, four-door
- **Finish** : fingerprint-resistant stainless, black stainless, matte, panel-ready
- **Energy certification** : ENERGY STAR status, and increasingly the estimated annual kWh
- **Connectivity** : smart/Wi-Fi enabled, compatible app
- **Install type** : freestanding, built-in, or fits-existing-cutout
- **Ice and water** : through-door dispenser, in-door ice maker, filtered vs. unfiltered
- **Voltage/plug type and venting requirements** , for anything beyond a basic refrigerator


None of these are nice-to-haves. Each one is a facet on a real retail filter, and in appliances the[GS1 Global Data Model](https://www.gs1.org/standards/gs1-global-data-model-attribute-implementation-guide/13-1) treats physical dimensions as core enough that a dimension change of more than 20 percent requires an entirely new[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) . That's how load-bearing depth and width are considered to be in this category: they're identity attributes, not descriptive flourishes.


## What happens when one of these is missing


Faceted search on a category page is really a chain of AND filters. A shopper picks "counter-depth," then "20-23 cu. ft.," then "French door," then "ENERGY STAR." Every one of those is a separate attribute field on the backend. If your feed has capacity but no depth class, you drop out at the second click, invisibly, with no error and no signal that anything went wrong. You still rank fine in a plain text search for "refrigerator." You just never reach the shopper who already knows what they want, which in appliances is most of them.


The AI layer makes this worse, not better. A shopping agent parsing a query like "counter-depth French-door fridge under 24 cu ft with an ice maker" is doing exactly what the faceted filter does, matching structured fields, not reading marketing copy.[OpenAI's Agentic Commerce product feed spec](https://developers.openai.com/commerce/specs/file-upload/products) asks for dimensions, weight, material, and a full taxonomy path as structured fields precisely because the model needs values it can filter and compare, not a paragraph it has to interpret. A missing depth field isn't invisible to a human filter and visible to an AI agent. It's invisible to both, and the AI agent has less patience for guessing.


## A French-door refrigerator, before and after


Here's a real-shape example: a mid-range French-door refrigerator as it typically lands in a raw supplier feed, versus what it needs to clear filters and answer AI queries.


Attribute Raw feed Enriched


Title "Refrigerator, stainless, large" "36-in. Counter-Depth French-Door Refrigerator, 22.1 cu. ft., Fingerprint-Resistant Stainless"


Capacity Not provided 22.1 cu. ft. total (14.9 fridge / 7.2 freezer)


Depth class Not provided Counter-depth


Configuration "French door" (free text) French door, 4-door, bottom freezer


Width Not provided 35.75 in.


Finish "Stainless" Fingerprint-resistant stainless steel


Energy Not provided ENERGY STAR certified


Ice/water Not provided In-door ice and water, filtered


Connectivity Not provided Wi-Fi enabled, companion app


Install type Not provided Freestanding, fits standard 36-in. opening


The raw version isn't wrong, it's just thin. Thin is enough for a browse page. It is not enough for a filter or a chat query, and appliances are a category where most traffic arrives already filtering.


## Ask an AI to recommend one


Try this yourself: ask an AI shopping assistant to "recommend a counter-depth French-door refrigerator around 22 cubic feet that's ENERGY STAR certified with an ice maker." Watch which retailers' products get named. It's almost never the retailer with the best price. It's the one whose product data actually contains all four of those values in a structured, matchable form. Everyone else's inventory, including plenty of equally good products, gets silently passed over.


[ENERGY STAR notes that certified refrigerators run about 9 percent more efficient](https://www.energystar.gov/products/refrigerators) than the federal minimum, a real differentiator shoppers actively filter for, but only if that certification is a field on the product record and not just a badge in a hero image.


## Anglera's role


Anglera continuously audits your appliance catalog against the attributes that actually gate filtered and AI search, capacity, depth class, configuration, finish, energy certification, connectivity, install type, flags the gaps, and fills them from spec sheets and manufacturer data so nothing sits half-tagged. It plugs into whatever PIM or feed you already run, without a rip-and-replace migration. Your PIM stores the data; Anglera makes sure every appliance in it is actually filterable.
