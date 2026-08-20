---
schema_version: "1.0.0"
document_id: "2bae2de166c68826f1ebe3946b6dfb8560fec2139767499088594cce90ec5961"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/beauty-aeo"
published_at: "2026-06-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:a53b691f0fe2f939ca41c3782270264a4d60e3def31be3bff7f6ca8d9938e7a9"
---

# How beauty shoppers search now — and why your catalog isn't the answer

Beauty shoppers used to start with a search box. Now they start with a question: "recommend a fragrance-free retinol serum for sensitive skin under $40." ChatGPT, Google's AI Mode, and Gemini answer that question by reading product data directly, not by sending someone to browse your PDP. If your catalog can't answer the question in structured form, you don't get considered at all.


## The path to purchase already moved


Google's AI Overviews now show up on roughly[14% of all shopping queries](https://almcorp.com/blog/google-ai-overviews-shopping-queries/) , a jump the report describes as a 5.6x increase in four months. That's not a niche feature anymore, it's a default layer sitting between a shopper's question and your product listing.


Beauty is moving even faster than the category average. Ulta Beauty didn't wait for this to become optional: it partnered with Google to put its catalog inside Gemini's AI Mode, letting shoppers get[recommendations, comparisons, and checkout directly inside Google's conversational interface](https://www.retaildive.com/news/ulta-ai-agent-gemini-shopping-experience/818308/) . Sephora has done something similar with an app inside ChatGPT, and Fenty Beauty built an advisor on WhatsApp. None of these are experiments anymore; they're distribution.


The mechanism matters more than any single retailer's rollout. An AI answer engine doesn't "browse" your site the way a shopper does. It queries structured data sources, parses` schema.org/Product` markup, and matches the product whose attributes fit the shopper's constraints. Text on a page can help, but agents read structured fields with far more confidence and far less latency than they read prose. If a field is empty, it's a query your product simply cannot win.


## Ask an AI to recommend one


Try this exact prompt in ChatGPT or Google's AI Mode: "recommend a vitamin C serum for oily, acne-prone skin, fragrance-free, under $35, that ships to a US zip code by Thursday."


That single sentence encodes six filters: ingredient, skin type concern, allergen exclusion, price ceiling, and delivery speed. An AI shopping agent can only apply a filter if your catalog exposes the matching attribute in a structured field. Most beauty PIMs don't. A typical raw feed carries a title, a price, and a marketing description that buries the actual formulation in adjectives.


Here's what that gap looks like for one SKU:


Field Raw feed (as exported from PIM) Enriched (agent-readable)


Title "Glow Boost Brightening Serum" Vitamin C Brightening Serum, 15% L-Ascorbic Acid


Key ingredient not populated` l-ascorbic acid` , 15% concentration


Skin type not populated oily, combination, acne-prone


Fragrance not populated fragrance-free


Allergen flags not populated no essential oils, no alcohol denat.


Price $32.00 $32.00,` priceCurrency: USD`


Return policy not in schema 60-day,` hasMerchantReturnPolicy` declared


Shipping not in schema 2-day delivery,` deliveryTime` declared


Reviews 4.6 stars (image only, no markup)` aggregateRating: 4.6` , 1,240 reviews, machine-readable


The raw version and the enriched version describe the identical bottle. Only one of them is answerable.


## Why "good enough" copy still fails


This is the part retailers get wrong most often: a beauty PDP can read beautifully to a human and still be invisible to an agent. Marketing copy like "luminous, radiance-boosting formula" contains zero of the filterable facts a shopper's AI prompt is actually asking for. The concentration, the skin type fit, the allergen exclusions, the shipping promise, these have to live in structured fields, not adjectives.


Independent audits back this up. Roughly[65% of pages cited by Google's AI Mode and 71% of pages cited by ChatGPT carry structured data](https://alhena.ai/blog/schema-markup-ai-search-ecommerce/) of some kind, but a meaningful share of that markup is incomplete or invalid, meaning the page still fails to answer the query even though a schema tag is technically present. Having schema on the page isn't the bar. Having the *right fields populated correctly* is.


For beauty specifically, the fields that decide whether a product surfaces in an AI answer are narrower and more specific than general retail:


Attribute category Why AI agents query it


Active ingredient + concentration Matches "retinol," "vitamin C," "niacinamide" style prompts


Skin/hair type and concern Matches "for oily skin," "for curly hair," "sensitive"


Fragrance and allergen flags Filters out disqualifying products before ranking begins


Size/format (trial vs full) Matches budget and "travel size" prompts


Certifications (cruelty-free, clean, vegan) Trust signals AI models weight when multiple products tie


Return policy and delivery window Answers "can I get this by Thursday" style constraints


Review volume and rating, structured Social proof an agent can cite without visiting the page


Trust signals carry real weight here too: products carrying verified certifications and badges get surfaced by AI models noticeably more often than otherwise-identical products without them, according to reporting on how[ChatGPT selects which beauty products to recommend](https://www.gcimagazine.com/retail/digital-e-commerce/news/22968759/the-top-sites-chatgpt-is-using-for-beauty-recommendations) . The models are weighing SKU-level completeness alongside brand reputation, not just brand reputation alone.


## The fix isn't a new platform, it's finished data


None of this requires ripping out your PIM or standing up a new commerce stack. Your PIM stores the SKU; the gap is that most beauty catalogs were built for a human scanning a grid of thumbnails, not an agent resolving six filters in one query. Getting from "raw feed" to "agent-readable" means filling in the ingredient, skin-type, allergen, certification, and policy fields your PIM already has room for but nobody populated at scale.


Anglera plugs into your existing PIM or commerce platform, additive rather than rip-and-replace, and continuously scores, gap-fills, and enriches exactly these attributes across a full beauty catalog. It turns marketing copy into the structured, agent-readable fields that ChatGPT, AI Mode, and Gemini actually query, so the next shopper who asks an AI to recommend a serum gets your product back as an answer, not a link they never click.
