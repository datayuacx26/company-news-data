---
schema_version: "1.0.0"
document_id: "f42fe332cc8ab62947f68490ca5048301d6fc73b303a1feff47dfe41572720a6"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/catalog-readiness-audit-before-you-spend"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T22:51:30.785703+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:437e825e4cdc6323ee68dfc67fec312166d322f68e710b57c7612d8b78e281d9"
---

# Audit before you spend: scoring a catalog for AI readiness on a sample, not the whole thing

Someone has quoted you a number to enrich 400,000 SKUs. Maybe it came from a vendor, maybe from your own team's estimate of contractor hours. Either way you are being asked to approve spend against a problem nobody has measured. The catalog is "bad." How bad, where, and worth how much are all open questions.


This post is about closing those questions in about a week, on a sample, before the first invoice. It is the diagnostic step under[how to structure product data for AI agents](https://www.anglera.com/blog/structure-product-data-for-ai-agents) — you cannot decide what to restructure until you know what is actually broken.


## What are you auditing for, exactly?


You are auditing for one thing: whether a machine that has never seen your business can identify a product in your catalog, understand its constraints, and rank it against alternatives. Everything else — tidy titles, consistent casing, image counts — is downstream of that. Score the machine's ability to act, not the catalog's tidiness.


That reframing kills a lot of busywork. A description with three typos still retrieves. A description with no pressure rating does not answer the query it needs to answer. If your audit rubric can't tell those two apart, it will generate a long remediation list where the expensive items and the cosmetic items look the same.


## Why sample instead of scanning the whole catalog?


Because you are making a funding decision, not a remediation plan, and a funding decision needs a shape rather than a census. A stratified sample of a few hundred SKUs will expose the same structural failures a full scan does, in days instead of a quarter, and without standing up tooling you may end up not needing.


Full-catalog scoring is the right instrument later. It is the wrong instrument now, for a practical reason: a scan of 400,000 rows produces 400,000 rows of findings, and the first thing anyone does with that output is aggregate it back down to a summary. You can produce the summary directly.


The sampling audit also survives a hostile reading. When a CFO asks how you know the number, "we hand-checked 300 items across every tier and supplier and found the same six failures in all of them" is a stronger answer than a fill-rate percentage from a query nobody can reproduce.


## Which SKUs belong in the sample?


Not random ones. Stratify along three axes — revenue tier, data provenance, and product family — and pull a fixed count from each cell. A random draw from a long-tail catalog is dominated by items that haven't shipped in two years, and it will hand you a picture of a catalog you don't actually sell.


The strata that matter in distribution:


Axis Why it splits the catalog Typical cells


Revenue tier Gaps cost different amounts in different tiers Top 5% by revenue, next 20%, tail


Data provenance Failures cluster by where the data came from Manufacturer feed, scraped/keyed, legacy ERP conversion


Product family Attribute needs differ wildly by category Commodity (fittings, fasteners) vs. configured (pumps, gear)


Twenty SKUs per cell is usually enough to see the pattern. What you are watching for is not the average score but the clustering: if every failure in the sample traces to one supplier's feed or one 2019 ERP migration, you have found a fix that costs one project rather than 400,000 line items.


## Which gaps block retrieval, and which only weaken it?


A blocking gap keeps the product out of consideration entirely. A weakening gap lets it compete and lose. Blocking gaps are cheap to find, few in number, and non-negotiable. Weakening gaps are where most of the money goes, and they should be funded second, by revenue.


Google enforces close to this distinction in its own systems. In Merchant Center,[products that receive warnings "will continue to show across Google, however their performance may be limited," while disapproved products "stop showing across Google"](https://support.google.com/merchants/answer/12153802?hl=en) until the issue is fixed. That is a usable line for an audit rubric, because it comes from a system that has to make the call at scale.


Sorted the same way:


Gap Effect Why


No identifier beyond your internal SKU Blocks Nothing external can match the item


Price or availability absent from the page response Blocks Google requires` offers` with` price` and` priceCurrency` on merchant listings


Specs only in a rendered widget or a PDF Blocks Never reaches a crawler that doesn't execute JavaScript


Category assigned but no attributes populated Blocks filtering Item is unreachable by any constrained query


Missing rating, tolerance, or certification Weakens Item retrieves, then loses on the constraint


No return policy or shipping detail Weakens Google lists both as recommended, not required


Thin, supplier-identical description Weakens Nothing distinguishes the listing from ten others


Google's[merchant listing documentation](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing) is worth reading against your own feed here: only` name` ,` image` , and a nested` offers` with price and currency are required, while` sku` ,` mpn` ,` gtin` ,` brand.name` ,` availability` ,` itemCondition` ,` priceValidUntil` ,` shippingDetails` , and` hasMerchantReturnPolicy` sit in the recommended tier. Required gets you eligible. Recommended is where the ranking happens.


## How do you score one SKU without inventing a rubric from scratch?


Borrow the rubric from the channels that already publish theirs. Score each sampled SKU on identity, offer, retrievability, and constraint coverage, and record a binary per item rather than a subjective grade. Binary scores aggregate honestly. Five-point scales don't survive two reviewers.


Four checks per SKU, in order:


1. **Identity.** Is there a[manufacturer part number](https://www.anglera.com/glossary/mpn-manufacturer-part-number) and a manufacturer name, or a[GTIN](https://www.anglera.com/glossary/gtin-global-trade-item-number) ? Google's identifier set is[GTIN, MPN, and brand](https://support.google.com/merchants/answer/6324478?hl=en) , and brand plus MPN is an accepted pair where no manufacturer-assigned GTIN exists. Your internal SKU alone is not identity.
2. **Offer.** Price, currency, and availability present in the response the server sends, not assembled later.
3. **Retrievability.** Does the spec table exist as text in that same response?
4. **Constraint coverage.** Take the three attributes a buyer in this category actually filters on and check whether they are populated as fields. For a circuit breaker, that's frame size, interrupting rating, and pole count. For a centrifugal pump, flow, head, and port size. Pick them per family before you start, not per item while scoring.


Four booleans per SKU across 300 SKUs is a spreadsheet, not a project. That is the point.


## What does an AI crawler actually see?


Not what your browser sees. Request the page with a crawler's user agent, read the raw bytes, and search that response for the values a buyer would filter on. If a pressure rating is absent from the raw HTML, it is absent for every crawler that does not run JavaScript.


```text
curl -sA "Mozilla/5.0 AppleWebKit/537.36 (KHTML, like Gecko); compatible; GPTBot/1.4; +https://openai.com/gptbot" \
https://example.com/p/12345 | grep -i "pressure rating"


```


One correction worth making, because it changes what you conclude.[GPTBot](https://www.anglera.com/glossary/gptbot) is OpenAI's *training* crawler. The crawler behind ChatGPT's search features is` OAI-SearchBot` , and OpenAI states plainly that sites blocking it[won't appear in ChatGPT search results](https://developers.openai.com/api/docs/bots) . If you are auditing for discovery rather than for training exposure, test` OAI-SearchBot/1.4` as well — and check your robots.txt while you're there, because a blanket AI-bot block that someone added in 2024 will invalidate the whole audit.


Anthropic splits its fleet the same way:[ClaudeBot for training, Claude-User for user-initiated fetches, and Claude-SearchBot to "improve search result quality"](https://support.claude.com/en/articles/8896518-does-anthropic-crawl-data-from-the-web-and-how-can-site-owners-block-the-crawler) . Three agents, three robots.txt decisions, and only some of them affect whether you get recommended.


Then run a handful of the sampled URLs through the[Rich Results Test](https://search.google.com/test/rich-results) to confirm the structured data parses. Two failure modes look identical from a browser and completely different from a crawler: markup that isn't there, and markup that is there but invalid. The deeper mechanics of that check are covered in[validating that your product data is agent-readable](https://www.anglera.com/blog/validate-agent-readable-product-data) .


## What the audit hands you


Three numbers, on one page.


The first is the share of revenue sitting behind blocking gaps. Not the share of SKUs — the revenue. In most industrial catalogs those diverge sharply, because the tail holds the blanks and the top holds the money, and a SKU-count view will send you to fix the wrong end first.


The second is the number of distinct fixes the gaps collapse into. Audits routinely find that six root causes explain nearly everything: one supplier feed that never carried attributes, one migration that dropped a field, one template that renders specs client-side, and so on. Six projects is fundable. Four hundred thousand line items is not.


The third is the SKU volume each fix touches, which is what turns the first two into a price.


That page is the deliverable. Not a scorecard, not a dashboard, not a maturity model — a decision about whether to spend, on what, first. Once the spend is approved, the sampling audit has done its job and should be retired in favor of continuous scoring across the full catalog, because completeness decays: suppliers revise specs, new items get set up in a hurry, and the number you fought for slides back down.


Sources:


- [Google Merchant Center: Issues in Merchant Center (warnings vs. disapprovals)](https://support.google.com/merchants/answer/12153802?hl=en)
- [Google Search Central: How to add merchant listing structured data](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing)
- [Google Merchant Center: Identifier exists \[identifier_exists\]](https://support.google.com/merchants/answer/6324478?hl=en)
- [OpenAI: Bots and crawlers](https://developers.openai.com/api/docs/bots)
- [Anthropic: Does Anthropic crawl data from the web, and how can site owners block the crawler?](https://support.claude.com/en/articles/8896518-does-anthropic-crawl-data-from-the-web-and-how-can-site-owners-block-the-crawler)
