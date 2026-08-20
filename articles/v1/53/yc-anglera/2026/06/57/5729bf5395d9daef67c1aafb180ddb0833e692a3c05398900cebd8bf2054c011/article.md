---
schema_version: "1.0.0"
document_id: "5729bf5395d9daef67c1aafb180ddb0833e692a3c05398900cebd8bf2054c011"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/footwear-guide"
published_at: "2026-06-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:2e8de7183d2080e996f3d1226b0673c79222a42fdd5263a35bc29199c570942a"
---

# Cutting returns in footwear with better product data

Footwear has one of the highest return rates in ecommerce, and almost none of it is about defective product. It's about a shopper who couldn't tell, from the page, whether the shoe would fit. Here's what a footwear PDP actually needs to answer before checkout, using a running shoe to show the gap between a typical feed and one that works.


## Footwear returns are a data problem wearing a logistics costume


Retailers tend to treat returns as a shipping and refunds problem. For footwear, it's mostly upstream of that. Fit and sizing are consistently cited as the single largest driver of returns in footwear and apparel, and[footwear return rates commonly land between 17% and 30%](https://footwear.fittingbox.com/en/blog/footwear-return-rate-benchmarks-and-key-metrics-to-track) , well above the ecommerce average. Nationally, the scale is enormous: the[National Retail Federation projected close to $850 billion in returned merchandise for 2025](https://www.digitalcommerce360.com/2025/10/24/nrf-consumers-return-850-billion-merchandise-in-2025/) , and footwear is a repeat contributor every year.


The shopper-side symptom is "bracketing": ordering two or three sizes (or widths) of the same shoe, planning to keep one and send the rest back. It's a rational response to a page that won't tell them what they need to know, and[research on bracketing behavior shows it's now routine, not fringe](https://www.imrg.org/blog/bracketing-the-customer-trend-thats-costing-retailers-millions-and-how-to-manage-it/) , especially among younger shoppers who've learned that brand size charts can't be trusted on their own. Every bracketed order is two-thirds return by design, and the retailer eats the shipping, restocking, and often the markdown on a shoe that's now "used."


The fix isn't a better returns portal. It's a page that removes the reason to bracket in the first place.


## The questions a footwear shopper actually needs answered


Before checkout, a shopper making a footwear decision runs through a specific list, whether or not the page helps them:


- What size am I in this specific brand and model, not just my usual size
- Does this run small, large, or true to size
- What width options exist, and is this one narrow, standard, or wide
- What's it for (daily trainer, race day, trail, walking, standing all day)
- What does it feel like underfoot (firm, plush, springy, minimal)
- What's the heel-to-toe drop and stack height, if it's a running shoe
- What's the upper made of, and does it stretch or hold its shape
- Can I return it if I've worn it outside once


A generic size chart answers maybe one of these. The rest sit in a gap between "raw feed" and "the guide a runner needs before dropping ninety-plus dollars."


## A running shoe, before and after


Here's a real-world pattern: a neutral daily trainer, mid-stack cushioning, sold in standard and wide. This is roughly what comes in from a typical supplier feed versus what a shopper (or an AI agent) actually needs to make a confident call.


Attribute Raw feed Enriched


Size range "7-13" US 7-13 (half sizes 7.5-12.5), true to size for 87% of buyers per size-exchange data


Width Not listed Standard (B/D) and Wide (2E) available; wide adds ~10mm at forefoot


Fit note Missing Runs true to size; roomier toe box than prior model year


Drop Missing 8mm heel-to-toe drop


Stack height Missing 32mm heel / 24mm forefoot


Best for "Running shoe" Daily training, road, neutral gait (no stability posts)


Upper "Mesh" Engineered knit mesh, minimal stretch, breathable


Weight Missing 9.8 oz (men's size 9)


Return note Standard 30-day 30-day return, indoor test-wear accepted


The "raw feed" column isn't hypothetical. It's what most supplier and manufacturer feeds hand a retailer by default: a title, a size run, a material guess, and not much else. Drop and stack height are exactly the kind of spec that lives in a PDF spec sheet or the brand's own site, never in the feed a retailer actually imports.


## Where AI shopping agents make this worse, not better


This gap used to just cost conversions on-site. Now it costs visibility too. When a shopper asks an AI shopping assistant to recommend a stability running shoe with a wide toe box for a runner who overpronates, the assistant is matching against structured attributes: width, stability type, drop, use case. A[product with those fields empty simply doesn't surface as a candidate](https://www.digitalapplied.com/blog/product-data-ai-shopping-merchant-prep-guide) , no matter how good the shoe actually is. The listing with "runs small," "wide available," and a stated drop wins the recommendation before the shopper ever lands on a page to bracket-order from.


That's the double cost of thin footwear data now: more returns from the shoppers who do click through, and fewer AI-driven visits from the shoppers who never get shown the product at all.


## The checklist


For any footwear SKU, these fields should exist and be populated before it goes live, not patched in after the return-rate report flags it:


- Numeric size range plus half sizes, stated explicitly
- Width options, named (narrow/standard/wide/extra-wide) with a size-difference note
- A fit note: true to size, runs small, runs large, roomy toe box
- Drop and stack height for running and trail shoes
- Stated use case (daily trainer, race, trail, walking, work)
- Gait/support type for running shoes (neutral, stability, motion control)
- Upper material and stretch behavior
- Weight, especially for running and hiking shoes where ounces matter
- A clear, visible return policy for worn-once trial fit


Most catalogs are missing four or five of these on any given SKU, and the missing ones aren't random. They cluster on exactly the fields that drive fit confidence.


## Where Anglera fits


Your PIM stores the size run, the width, the material claim. It doesn't audit every footwear SKU for the fit-critical fields buried in a supplier PDF, catch when drop or stack height went missing on a new model year, or write the "runs small" note a merchandiser would only add by hand. Anglera scores each footwear listing against the fields that actually predict returns, gap-fills them from source specs, and keeps them current as new sizes and colorways roll in, so the PDP a shopper reads and the feed an AI agent parses are the same complete answer.
