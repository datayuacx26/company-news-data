---
schema_version: "1.0.0"
document_id: "7a1aa7c0437e1e73144f75c06999ecb6138b3f178cee414fbb31046fcc38a6e2"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/apparel-state"
published_at: "2026-06-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:959e35a427c143f9f6c54e8713febb7af74d6bb134d56ed41f2511c0729b6dd1"
---

# The state of product data in Apparel retail (2026)

Apparel has the messiest product data in retail, and it shows up in the numbers that matter: returns, conversion, and now, whether AI shopping agents can even parse what a retailer sells. Fixing a size chart or a color name feels like a rounding error until it multiplies across 40,000 SKUs and every channel that reads the feed.


## The catalog problem hasn't gone away


Apparel catalogs churn constantly. New colorways, new seasons, new supplier feeds, new marketplaces — every one of them is a chance for a size to get formatted differently or a material field to go blank. Multi-brand retailers compound this because every supplier ships its own naming conventions: one feed says "grey," another says "Grey," a third says "charcoal heather," and none of them match.


The result is a catalog that looks complete in a spreadsheet but fails the moment a shopper or an algorithm tries to filter on it. A missing` size` or` color` variant attribute isn't cosmetic in apparel —[Google requires both as mandatory attributes for Apparel & Accessories products](https://support.google.com/merchants/answer/7052112?hl=en) , and inconsistent formatting (` S, Medium, Lrg` instead of a single convention) is a documented cause of listing disapprovals. That's inventory that simply can't show up in Shopping results, not inventory that ranks poorly.


## What it costs: returns


Apparel already carries the highest return rate of any retail category. Depending on the source,[apparel returns run 20-40%, roughly double the ecommerce average](https://www.richpanel.com/learn/ecommerce-return-rates) , and fit or sizing issues account for as much as 70% of those returns. That's not a shipping or fraud problem. It's a data problem: shoppers can't tell, from the product page, whether an item will fit.


The mechanism is simple. A size chart that's missing, inconsistent across styles, or expressed in a unit the shopper doesn't use (EU sizing shown to a US shopper, for example) forces a guess. Guesses come back. So does "bracketing" — ordering multiple sizes with the plan to return what doesn't fit — which has become close to standard behavior in fashion ecommerce and inflates return volume regardless of how good the product itself is.


Here's what that looks like on an actual product page, before and after:


Attribute Raw feed Enriched


Size` M`` M (US 6-8, EU 38-40, fits 34-36in waist)`


Fit (blank)` Relaxed fit, true to size`


Material` cotton`` 98% cotton, 2% elastane, mid-weight (240gsm)`


Color` charcoal`` Charcoal Heather (Gray)`


Care (blank)` Machine wash cold, tumble dry low`


None of that requires new photography or a new supplier relationship. It requires someone — or something — to actually fill in the fields the feed already has room for.


## What it costs: AI visibility


The newer pressure is that apparel is now one of the categories where AI shopping agents are most active, because a query like "black wool coat under $300, ships by Friday" maps almost directly onto structured[product attributes](https://www.anglera.com/glossary/product-attributes) : category, material, color, price, availability. Agents don't read marketing copy to answer that query. They read the feed.


By early 2026, that channel is no longer theoretical.[ChatGPT's shopping surface was handling roughly 50 million shopping queries a day](https://elogic.co/blog/chatgpt-commerce-statistics/) with 900 million weekly active users, and apparel brands including Spanx, Vuori, and SKIMS were already live on agentic commerce integrations. Google's AI Mode and Gemini pull from the same Merchant Center feeds that apparel retailers already maintain — which means the same missing size fields, blank materials, and inconsistent color names that hurt search filtering also make a product invisible to an AI agent trying to match a shopper's request.


Ask an AI shopping assistant to "recommend a machine-washable, true-to-size midweight jacket under $150" and watch what happens to a product with a blank material field and a size labeled only` L` . It doesn't get excluded on merit. It gets excluded because the agent has nothing to match against.


## Why this is a 2026 problem, not a 2020 one


Three dynamics are converging:


1. **Marketplace and AI channels multiply the cost of bad data.** A gap that used to hurt one storefront's search now hurts every channel reading the same feed — Google Shopping, Amazon, TikTok Shop, and AI agents simultaneously.
2. **SKU proliferation keeps outpacing manual QA.** Fast seasonal turns and expanding size ranges mean more rows to check, not fewer, and most PIMs store the field without validating what's in it.
3. **Returns are now a margin line, not a customer-service line.** With apparel returns running double the retail average and driven mostly by fit, a retailer that can shave even a few points off sizing-driven returns is looking at a direct hit to gross margin, not just a support-cost reduction.


None of this requires ripping out a PIM or adopting a new commerce platform. Most of the raw data — size, material, fit, care — already exists somewhere in the supply chain; it's rarely standardized or complete at the SKU level where it's needed.


Anglera plugs into the PIM or commerce platform a retailer already runs and continuously scores, gap-fills, and normalizes exactly these attributes — size, fit, material, color — so a catalog reads consistently to shoppers, search engines, and AI shopping agents alike. Your PIM stores the data; Anglera does the work of keeping it complete.
