---
schema_version: "1.0.0"
document_id: "b0e848a5b352639005d3b45e21deadc12fa2f83ed850d9d5c38527865b14d324"
company_key: "yc-dot"
company: "Dot"
source_id: "yc-dot-news-import-8b97b36288bf"
canonical_url: "https://www.getdot.ai/blog/same-customer-spelled-four-ways"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-21T16:52:13.925276+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:f7f6de2cd5abd9c6e8099fc965886ab0d61715bb550fa6d74547b26e52edc7af"
---

# The Same Customer, Spelled Four Ways

**Ask a simple question: "How much revenue did we do with Acme this year?"**


Simple, until you open the warehouse. There's "ACME Corp" in Salesforce, "Acme Corporation GmbH" in billing, "acme-corp" in product events. And since last year's acquisition, half of Acme's spend sits under "Brightline Ltd", the company they bought, which finance merged and product never did.


The question was easy. Knowing that four strings are one customer was the hard part.


## The chore nobody budgeted


Humans absorbed this problem so quietly that most companies never noticed it was work. The analyst just knew: which IDs map, that the GmbH entity bills separately but rolls up, that Brightline is Acme now. The match happened in their head, got applied in a WHERE clause, and never landed anywhere.


An AI analyst exposes the gap immediately, in one of two bad ways. Strict matching misses revenue: Acme looks 40% smaller than it is, and someone makes a call on a wrong number. Fuzzy matching guesses: sometimes right, sometimes merging two companies that share nothing but a name. Either way, the day someone audits the answer, trust takes the hit. And the agent didn't fail at SQL. It failed at knowing two strings are the same company, which is the one thing nobody ever wrote down.


## Why this is finally fixable


Entity resolution used to be a grim project. Rule cascades, regex, string-distance thresholds, and a maintenance treadmill where every new edge case got a new rule. Master data management initiatives went there to die.


Embeddings changed the floor. Matching on meaning rather than spelling, using name, domain, address, whatever you have, is now a vector operation your warehouse runs natively, and an LLM can adjudicate the ambiguous middle of the distribution cheaply. The match quality that used to need a six-month master data project is now a pipeline plus an evaluation set: a few hundred human-verified pairs and an accuracy number you actually track. Once you can measure the matching, you can improve it, instead of accumulating rules forever.


## Resolve once, not on every query


So where should the matching live? You could let the agent fuzzy-match at query time, on every question, with slightly different judgment each time. That's[paying the toll on every trip](https://www.getdot.ai/blog/preparation-time-vs-query-time) , and inconsistently.


The better answer is to resolve entities once, at preparation time, into a mapping table that everyone shares: the agent, the dashboards, the CRM cleanup project that was always about to start. And unlike an in-flight guess, a mapping table is reviewable. A human can look at "Brightline Ltd maps to Acme Corp, confidence 0.78" and confirm it or reject it. The judgment call stays a judgment call. But now it's made once, recorded, and owned, instead of being remade silently inside every query.


Your AI analyst will meet your messy CRM on day one. The only question is whether it meets a mapping table, or makes one up.


*If this excites you, we'd love to hear from you.*[Get in touch](https://www.getdot.ai/) *.*
