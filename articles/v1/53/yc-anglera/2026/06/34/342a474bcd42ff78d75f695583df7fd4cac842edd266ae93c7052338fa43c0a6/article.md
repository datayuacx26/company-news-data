---
schema_version: "1.0.0"
document_id: "342a474bcd42ff78d75f695583df7fd4cac842edd266ae93c7052338fa43c0a6"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/health-supplements-attributes"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:38.455846+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:a3f0102085a67060550e2c5a6a8ec60df56bb47841c61634a88856e7090f8403"
---

# Building an attribute schema for Health & Supplements that shoppers and AI can actually use

A supplement facts panel tells a shopper what's in the bottle. It does not tell a filtered-search facet or an AI shopping agent whether that bottle is vegan, third-party tested, or dosed for someone over 50. Those are two different jobs, and most Health & Supplements catalogs only do the first one.


## The attributes a supplement facts panel doesn't cover


Every supplement label legally has to disclose serving size, servings per container, and the amount of each dietary ingredient, under[21 CFR 101.36](https://www.ecfr.gov/current/title-21/chapter-I/subchapter-B/part-101/subpart-C/section-101.36) . That's the compliance layer. It's necessary and it's usually the only structured data a brand feed actually carries.


The commerce layer sits on top of it, and it's where most catalogs go blank:


- **Form** — capsule, softgel, gummy, powder, liquid, tablet
- **Potency per serving, in a standardized unit** — milligrams, micrograms, or IU, normalized so a 1,000 IU vitamin D3 and a 25 mcg vitamin D3 read as the same strength
- **Servings per container and days-supply** , so a shopper can compare cost-per-month, not just price-per-bottle
- **Active ingredient / formulation type** — e.g., "vitamin D3 (cholecalciferol)" vs. "vitamin D2 (ergocalciferol)," which are not interchangeable to a filter or an AI agent
- **Diet and lifestyle claims** — vegan, non-GMO, gluten-free, kosher, halal
- **Allergen declarations** — the nine major allergens (milk, egg, fish, crustacean shellfish, tree nuts, peanuts, wheat, soybeans, and sesame, added by the[FASTER Act](https://www.fda.gov/food/food-allergies/faster-act-sesame-ninth-major-food-allergen) effective January 2023)
- **Third-party certification** — NSF Certified, USP Verified, Informed Sport/Choice, or none, which is itself a meaningful filter for anyone buying for an athlete on a testing protocol
- **Life stage / age group** — prenatal, kids, 50+, general adult
- **Flavor and delivery format** for gummies and powders


None of this is exotic. It's the stuff a shopper actually types into a search box or asks an AI assistant. And almost none of it lives in a supplement facts image or a free-text description field, which is exactly where most PIMs still park it.


## Why a missing attribute deletes the product, not just the detail


A faceted search filter and an AI shopping agent both work the same way: they read structured fields, not photos, and they treat a blank field as "unknown," not "yes" or "no." A vegan protein powder with no` diet_claims` field doesn't get excluded from a vegan-only search view — it never appears in it. The product isn't found and rejected; it's invisible.


The same failure shows up on the AI side. Structured product data — schema.org` Product` markup, feed attributes, standardized units — is what an AI shopping agent parses to compare options; without it, the agent either skips the listing or falls back to whatever text it can scrape, per[reporting on agentic commerce feed requirements](https://www.lengow.com/get-to-know-more/chatgpt-product-feed/) . If your feed says "1000" in a potency field with no unit, or "gluten free" only inside a paragraph of marketing copy, an AI agent building a comparison table across brands has nothing to hook the claim to. It won't guess. It'll just leave your product out of the answer.


## A vitamin D3 bottle, before and after


Here's a fairly typical raw feed for a store-brand vitamin D3 bottle, and what it looks like enriched to the attribute set above.


Field Raw feed Enriched


Title "Vitamin D3 Softgels 120ct" "Vitamin D3 (Cholecalciferol) 2000 IU Softgels, 120ct"


Form — (in title only) Softgel


Potency — 2000 IU (50 mcg) per serving


Servings / days supply — 120 servings / 120-day supply


Active ingredient — Cholecalciferol (D3)


Diet claims — Non-GMO


Allergens — Contains: none of the 9 major allergens


Certification "third-party tested" (body copy) NSF Certified


Life stage — Adult, general


Serving frequency "take as directed" 1 softgel daily


The raw version reads fine to a person scanning the bottle. It's unfilterable and unquotable to a machine — there's no field an AI agent or a facet can point to for "2000 IU," "non-GMO," or "NSF Certified." Ask an AI shopping assistant to "recommend a non-GMO vitamin D3 at 2000 IU that's third-party tested," and the enriched version is eligible to be surfaced with a specific, checkable reason; the raw version is guessed at, or skipped.


## Structuring it so it holds up


Group the schema into layers instead of one flat attribute list:


1. **Identity** — product name, brand, form, flavor,[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number)
2. **Potency** — active ingredient, standardized dose unit, servings per container, days-supply
3. **Claims and diet** — vegan, non-GMO, gluten-free, kosher/halal, allergen-free declarations mapped to the nine major allergens
4. **Certification** — named third-party seal (NSF, USP, Informed Sport) as a controlled value, not free text
5. **Audience** — life stage, age group, intended use occasion (sleep, immune, joint, prenatal)


The controlled-value part matters more than the list itself. "Non-GMO," "Non GMO," and "GMO-free" are the same claim to a shopper and three different values to a filter unless someone normalizes them. That normalization work — mapping messy supplier text to a consistent schema, flagging gaps by SKU, and keeping it current as formulations change — is what Anglera does continuously on top of whatever PIM or feed a retailer already runs. It doesn't replace the catalog system of record; it keeps the attributes in it complete and machine-readable, so the same bottle of vitamin D3 shows up whether a shopper clicks a filter or asks an AI to find one.
