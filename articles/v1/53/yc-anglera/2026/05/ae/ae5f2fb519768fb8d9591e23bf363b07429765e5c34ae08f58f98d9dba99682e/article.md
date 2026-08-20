---
schema_version: "1.0.0"
document_id: "ae5f2fb519768fb8d9591e23bf363b07429765e5c34ae08f58f98d9dba99682e"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/sporting-goods-aeo"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T22:13:02.720629+00:00"
content_hash: "sha256:2a78b838fe70df95faf55ecb3fe58c84391cd7b763045b0ee76dd44b033006a7"
---

# How sporting goods shoppers search now — and why your catalog isn't the answer

A shopper training for a fall marathon doesn't open ten tabs anymore. They ask ChatGPT or Google's AI Mode one question — "what's a good stability running shoe for overpronators under $150" — and expect a shortlist with reasons attached. If your product pages don't answer that question in a format machines can parse, your shoes never make the list, no matter how good they are on the shelf.


This is already showing up in the traffic numbers. ChatGPT hit[900 million weekly active users in February 2026](https://techcrunch.com/2026/02/27/chatgpt-reaches-900m-weekly-active-users/) , roughly double where it was a year earlier, and shopping-style queries are a growing share of that usage. Google, Shopify, and a coalition that includes Target, Walmart, Etsy, and Wayfair have already launched a shared[Universal Commerce Protocol](https://www.anglera.com/glossary/universal-commerce-protocol-ucp) so agents can browse and check out across retailers, while OpenAI and Stripe have their own competing[Agentic Commerce Protocol](https://www.anglera.com/glossary/agentic-commerce-protocol-acp) —[the race to build agentic commerce](https://www.fastcompany.com/91533534/shop-til-you-bot-google-openai-and-the-race-to-build-agentic-commerce) is real and it's moving fast. Sporting goods, specifically, is one of the categories where this shift is landing hardest, because gear buying is naturally spec-driven — cushioning stack height, drop, weight, waterproof rating, flex pattern — and that's exactly the kind of detail conversational search is built to filter on.


## Why "detail-heavy" categories are ground zero


Running shoes are a good proxy for the category's current momentum: running footwear grew[+8.9% year-over-year while lifestyle sneakers declined](https://www.yipitdata.com/resources/blog/corporate-footwear-market-trends-running-growth) , with Hoka, On, and New Balance taking share on the strength of performance specs, not just style. That's a category where shoppers ask pointed comparison questions — stack height, drop, plate or no plate, wide-toe-box or standard — and an AI answer engine either has that data or it doesn't.


Nearly a third of sporting goods purchases now begin or end in a digital moment — researched on a phone, compared against alternatives, then bought online or picked up in store. Increasingly, that research moment is a conversation, not a search-results page. If the AI can't cite your product's drop, weight, or width options, it recommends the competitor whose feed has them.


## The gap: your catalog was built for people, not agents


Most sporting goods PDPs read fine to a human. A shopper can look at a photo of a trail running shoe and infer it's for trails. An AI agent can't infer anything — it reads whatever text and structured fields exist, and nothing else.


Here's a realistic before/after for a trail running shoe listing:


Field Raw feed (typical) Enriched for AI + shoppers


Title Men's Trail Shoe Sz 10 Men's Trailrunner GTX Waterproof Trail Running Shoe


Description Great shoe for running Waterproof trail running shoe with Vibram outsole, 8mm heel-to-toe drop, rock plate, for technical terrain up to 20 miles


Terrain (missing) Trail, technical terrain


Drop (missing) 8mm


Waterproofing (missing) Gore-Tex membrane, waterproof


Weight (missing) 10.6 oz (men's size 9)


Width options (missing) Standard, Wide


GTIN/UPC (missing) 8-digit valid GTIN present


Return policy Linked in footer only Declared in product schema (30-day, free returns)


The right column isn't marketing copy — it's the set of discrete, machine-readable attributes an agent needs to decide whether this shoe answers a shopper's question at all.


One mechanism matters more than most retailers realize:[GTIN is one of the strongest matching signals Google uses](https://www.paz.ai/guides/google-merchant-center-for-ai-mode) to cluster a product across retailers and pull it into AI Mode's comparison view. A missing, malformed, or reused[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) doesn't just hurt SEO — it can drop a product out of the comparison set entirely, so an agent never even considers it against a competitor's cleats or hydration pack.


Return policy is a second, less obvious one. AI shopping answers increasingly favor listings where` hasMerchantReturnPolicy` is explicitly declared in schema, because an agent trying to answer "can I return these if they don't fit" needs a structured answer, not a link to a policy page buried three clicks deep.


## What "machine-readable" actually looks like


For a sporting goods catalog, agent-ready product content generally means:


- **Schema.org Product and Offer markup** on every PDP — price, currency, availability, and GTIN as structured fields, not just visible text
- **Attribute completeness by subcategory** — drop and stack height for running shoes, cleat pattern and turf compatibility for soccer boots, load capacity and frame material for packs, flex rating for skis
- **A valid, unique GTIN or an explicit "no GTIN" flag** rather than a blank field, since agents and Merchant Center both treat blanks as a red flag
- **Declared return and shipping terms** in structured data, not just footer copy
- **Consistent pricing** between the feed, the schema on the page, and checkout — mismatches are one of the fastest ways to get an agent to demote or drop a listing


Ask an AI right now: "recommend a waterproof trail running shoe with a rock plate for someone who overpronates, under $160." Watch which retailers show up. The ones that surface are, almost without exception, the ones whose product data already answers that exact question in a structured field — not the ones with the best shoe.


That gap between what a catalog contains and what it can actually answer is the whole problem. Most sporting goods catalogs weren't built with terrain type, drop, or fit-width as required fields, because no one needed them to be until agents started reading feeds instead of pages.


Anglera plugs into whatever PIM or commerce platform a retailer already runs — no rip-and-replace — and continuously scores, gap-fills, and enriches product content so attributes like drop, GTIN, and return terms are present, consistent, and structured the way AI shopping agents actually read them. Your PIM stores the data. Anglera does the work of making sure that data can answer the question a shopper — human or AI — is actually asking.
