---
schema_version: "1.0.0"
document_id: "39fcba8a82a8baed36241ea7cdb6f1bde38528bd19bbae0b6291195eea3507b1"
company_key: "yc-extend"
company: "Extend"
source_id: "yc-extend-news-import-054f4f06cd55"
canonical_url: "https://www.extend.ai/resources/dont-pay-the-zdr-tax"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-18T21:11:52.319392+00:00"
fetched_at: "2026-08-18T21:11:55.133657+00:00"
content_hash: "sha256:f65cf1350bfb4e04657148f2bb3ca888f99e2cfa6acd83561576e8d3a535fe02"
---

# Don’t pay the ZDR tax

**TLDR:**


1. Extend does not charge additional fees for zero data retention (ZDR) on any of our plans (including pay-as-you-go, monthly, or annual).
2. If you’re currently paying for ZDR with another provider, please reach out — we’re offering a migration discount and dedicated migration support.


There’s a new tax showing up in AI infrastructure — the ZDR tax.


More AI providers are putting zero data retention behind annual contracts, enterprise plans, or additional platform fees. In some cases, we've watched companies get quoted $25k+ by other providers just to ensure that their data is deleted.


Reducto ZDR terms, reviewed August 2026.Pulse ZDR terms, reviewed August 2026.LandingAI ZDR terms, reviewed August 2026.Firecrawl ZDR terms, reviewed August 2026.Brave ZDR terms, reviewed August 2026.


Why does this happen? ZDR costs a provider almost nothing to offer — it's the *absence* of retention (not a feature that's expensive to maintain). But because it's a hard requirement for anyone deploying AI in a regulated industry, it makes a perfect toll booth. Vendors know you can't ship without it, so you'll pay whatever it costs.


We think this is backwards.


If you're deploying AI into production in financial services, healthcare, real estate, or basically any regulated industry, ZDR isn't a premium add-on. It's table stakes.


Your customers expect it. Your compliance team needs it. No company should have to choose between paying a five-figure tax and keeping their customers' data safe.


## Extend’s commitments


**Every customer can enable ZDR.** ZDR is available on all of our paid plans, including pay-as-you-go (PAYG) and monthly plans. You don't need to sign an annual contract, there's no "platform fee", and no lengthy negotiation needed to unlock it. No ZDR tax.


**We will never train on customer data.** Unlike some of the other document processing providers out there, Extend never trains on customer data. There are plenty of legitimate ways to build training datasets. Reaching into our customers' documents is not one of them.


We made these decisions early because document processing infrastructure sits in one of the most sensitive parts of the AI stack.


Our customers send us the documents their business runs on. Mortgage applications. Healthcare records. Bank statements. Insurance forms. Legal agreements. Often these documents contain information that companies would never consider putting into a public dataset. We take this responsibility seriously (and it doesn't matter if you're an early stage company or an enterprise).


## Paying the ZDR tax today?


If you're currently paying another provider for zero data retention, reach out and we'll work with you on:


- **Discounted rates** for teams currently paying another provider for ZDR
- **Dedicated migration support** to move existing document processing workloads over to Extend


If you’re subjected to ZDR fees,[let us know how we can help](https://www.extend.ai/get-started) .


## Security doesn't come at the expense of performance


Extend is built to win on both trust and performance.


We publish open-source, independently reproducible benchmarks across the document types our customers actually process in production. You can compare Extend against other providers on accuracy, latency, and cost (or run the eval yourself on your own documents).


[RealDoc-Bench](https://www.extend.ai/resources/realdocbench) Parsing


Extend achieved state-of-the-art accuracy across real-world documents and was 1.5× faster than the next-most accurate system.


[LongArray-Extract](https://www.extend.ai/resources/long-array-extraction-benchmark) Extraction


Extend delivered 99.2% accuracy with 100% completion and was 2.8× faster than the closest competitor.


[PoliTax Split](https://www.extend.ai/resources/document-splitting-benchmark) Splitting


Extend harness catches 2/3 or more boundaries for every underlying model, adding 8.3-28.4 points over direct model use.


Don't switch to Extend just because we have a better ZDR policy. Switch because we performed better on your documents, at better cost and latency, **and** our data guarantees and philosophy align with yours.


## More on how Extend handles your data


- Our[data-handling documentation](https://docs.extend.ai/security/data-handling) explains exactly how Extend stores, encrypts, retains, and deletes customer data.
- Customers with data-residency or infrastructure-isolation requirements can choose US regions, an EU region, hybrid deployment, or bring your own cloud. Details in our[deployment documentation](https://docs.extend.ai/security/deployment-options) .
- Our[compliance documentation](https://docs.extend.ai/security/compliance) covers our certifications and regulatory programs, and supporting reports are available through the[Extend Trust Center](https://trust.extend.ai/) .


AI infrastructure companies ask customers to trust them with sensitive parts of their business. That trust shouldn’t come with an add-on fee. Your data is yours, and we're committed to keeping it that way.
