---
schema_version: "1.0.0"
document_id: "2b0e56dd9774f8650a9b5a95be44f44a2fb6e137504658000fec624b336e81bc"
company_key: "yc-lumen-payments"
company: "Lumen Payments"
source_id: "yc-lumen-payments-news-import-87c7fb1c30f2"
canonical_url: "https://getlumen.dev/blog/we-were-wrong-about-the-future-of-ai"
published_at: "2025-09-25T00:00:00+00:00"
first_seen_at: "2026-07-22T02:53:16.718446+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:81e158947692d78d9ae4201cf62bb85a00716385accb995956f0b24b944a4fe0"
---

# We were wrong about the future of AI

When we were doing YC in the beginning of 2024, the common idea was that LLMs would keep getting better and cheaper. At the time, many pitches assumed that even if the current burn rate was high, it would drop soon. Some assumed we would also be able to self‑host open source models and reduce costs that way.


Only one of those assumptions turned out true: the models did get better, but they also got much more expensive, especially with agentic workflows. On top of that, they have become slower with the widespread of reasoning capabilities.


Switching to cheaper models is usually not an option, since the product ends up performing worse next to competitors who are paying for stronger models. Because of this, companies are trying to figure out pricing models that make sense. Most experiments fall into 4 buckets:


1.


**Flat fee**


This is popular because it’s easy to implement with Stripe or similar providers. Companies put a fixed usage limit (e.g. Claude and Cursor) and hope for the best. Powerusers are balanced by light users, and the math works out overall.


2.


**Credit system**


This is the most common approach today. Customers buy credits that map closely to tokens or dollar cost. Credits are often renewed automatically with the billing cycle or added via top up (i.e. prepaid credits).


3.


**Usage‑based billing**


This can be the only pricing model or in addition to the flat-fee or credit based system (aka overusage). This model is not new but it is fairly popular with AI since it allows to easily implement *cost + margin* model.


4.


**Outcome‑based pricing**


Common in AI customer support. Instead of paying per token, customers pay for a solved ticket or a completed outcome. This works because companies already pay by resolution when outsourcing support. It sets clear expectations but doesn’t always map well to open‑ended AI use cases.


#### Ramon Garate


Founder of Lumen
