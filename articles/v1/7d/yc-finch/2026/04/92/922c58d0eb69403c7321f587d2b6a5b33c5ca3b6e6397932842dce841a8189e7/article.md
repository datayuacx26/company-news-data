---
schema_version: "1.0.0"
document_id: "922c58d0eb69403c7321f587d2b6a5b33c5ca3b6e6397932842dce841a8189e7"
company_key: "yc-finch"
company: "Finch"
source_id: "yc-finch-news-import-d853ae2d7ef6"
canonical_url: "https://www.tryfinch.com/blog/under-the-hood-opinionated-data-modeling-case-study"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-24T08:01:05.068281+00:00"
fetched_at: "2026-07-28T22:15:52.361502+00:00"
content_hash: "sha256:33d47613df6e20c6c72bce8b5470ee493eaff0831899ff4a67380aacec769474"
---

# Under the Hood: A Case Study in Opinionated Data Modeling

*This post is part of Under the Hood, a series from Finch's product team on the real engineering and product challenges behind unifying payroll data at scale.*


Unifying payroll data demands more than just passing raw data from different providers into a single endpoint. The data has to be consistent and standardized to be actionable. In the[vastly fragmented payroll landscape](https://www.tryfinch.com/blog/under-the-hood-challenge-and-value-of-unifying-payroll-data) , that means making deliberate choices about how to normalize values, what data to expose, and when and where to abstract provider-specific nuances. A powerful[unified API](https://tryfinch.com/blog/what-is-a-unified-api-everything-you-need-to-know) uses an opinionated data model to make these decisions, so the developers building on its foundation don’t have to.


That’s easy to say in the abstract. In practice, the rationale behind those decisions is based on deep research that is validated against real customer use cases. Every aspect of the model is designed to serve customers’ most common needs reliably.


To illustrate how Finch approaches these decisions, let’s take a look at how we designed our model for a specific field in our API: FLSA status.


## FLSA status: A case study


The Fair Labor Standards Act (FLSA) classification determines whether an employee is exempt or non-exempt from overtime protections. This distinction matters for benefits eligibility, leave management, income verification, and more.


It sounds like a straightforward field, and in most individual payroll systems, it is. The majority of providers use a simple toggle or two-option dropdown. But when you try to represent that field consistently across[dozens of providers](https://www.tryfinch.com/blog/under-the-hood-auth-fragmentation-and-connection-health) , the edge cases begin to stand out, with small variances in field names, value sets, and levels of granularity. Some providers expose it as a dedicated FLSA field with six constants; others bury it in an overtime exemption boolean; still others don’t surface it at all.


When I set out to add flsa_status to our employment API, I had to navigate all of that variance and make a call: how granular should this field be?


My first instinct, like most PMs, was to capture as much nuance as possible. FLSA classification has real complexity. There are white-collar exemptions, commission-based exemptions, highly compensated employee rules, and industry-specific carve-outs. Some of our competitors expose four or more distinct values. I wanted to be thorough.


Then I actually dug into how providers store this data, and how employers use it day-to-day. What I found changed my thinking entirely.


## The data that changed my mind


I went provider-by-provider through our top integrations to understand how FLSA status is actually modeled at the source.


**Most providers implement FLSA as a binary.** Across the providers I researched, the dominant pattern was a two-option field: Exempt or Non-Exempt. In several cases, the field wasn’t even labeled “FLSA Status” explicitly—it was surfaced as an overtime exemption toggle, a yes/no flag on position configuration, or a boolean in the API. The field that looks like it should be rich with classification data is, in practice, a simple binary for most of our provider network.


**Even where more granularity exists, employers often don’t use it.** This was the most surprising finding. Some providers technically support custom FLSA codes or additional classification types beyond the standard two. But when I researched how employers were actually configuring their accounts, most weren’t touching those options at all. They were using the simpler mechanism, often just an overtime exemption checkbox, and leaving the more detailed fields blank.


Our primary driver for this feature was customers in the absence and leave management space who need FLSA status to determine benefits eligibility and calculate claim payouts correctly. When I dug into what they actually need to know, it’s a binary: is this employee subject to FLSA overtime rules or not? That answer shapes which benefit plan they qualify for and how their claim is modeled.


The same pattern holds for every other use case in our pipeline, like group term life insurance providers and lenders doing income verification. None of them said “I need to know if someone is specifically a Salaried Non-Exempt vs. a straight hourly Non-Exempt.” They only need to know which side of the line an employee sits on.


## The problem with going granular anyway


I seriously considered adding more granularity to our model. A more detailed enum, separating out things like Salaried Nonexempt, Owner, or Commission Only Exempt does carry real signal.


But those distinctions only have value if you can reliably populate them across your provider network—and we can’t. Most providers in our network don’t store those sub-distinctions; they just store Exempt or Not. Shipping a six-value enum when four of those values would never be returned on the majority of our integrations doesn’t make the API richer, just noisier. And it creates a false promise about what we can deliver.


Other unified APIs have taken this approach, and their fallback for unmapped statuses ends up being a pass-through of the raw provider value. That’s fine for a pass-through API, but Finch’s value proposition is normalized, reliable data. We have to be honest about what we can actually normalize consistently.


That being said, our customers can still access the raw data: Finch supports passthrough via request forwarding for those edge cases.


## Finch’s FLSA model


Finch’s FLSA field ships as EXEMPT, NON_EXEMPT, UNKNOWN, and nullable. EXEMPT and NON_EXEMPT map cleanly from every provider that exposes the field. UNKNOWN exists for the edge case where a provider has a custom classification value that genuinely can’t be mapped to either side. Null means the employer hasn’t configured the field in their payroll system, which happens more than you’d expect.


In the end, I decided to optimize for reliability over theoretical data richness. The model maximizes useful, trustworthy signals given the actual state of how payroll providers store this field and how employers populate it. Sometimes the right API design is the one that accurately reflects the world rather than the one you wish existed.


The nuance will still be there when it’s needed. The exemption sub-type question is legitimate, and if customers come back asking for it with evidence that providers can support it reliably, we can revisit. But for now, the data says two values is the right answer.


## Why this matters beyond one field


FLSA status is one field in one API endpoint. But the process behind it—researching how providers actually store the data, testing assumptions against real employer configurations, choosing reliability over theoretical completeness—is the same process behind[every field in Finch’s unified data model](https://www.tryfinch.com/blog/under-the-hood-from-unifying-data-to-powering-workflows) .


This is what we mean when we talk about an **opinionated model** . Our opinions are deliberate, not arbitrary: every field represents a researched, validated decision about the most useful way to represent a piece of employment data across a fragmented ecosystem.


The alternative is to pass through raw provider values and leave the interpretation to each customer. It’s faster to ship but harder to build on, and it shifts the normalization burden downstream, where every customer has to solve the same mapping problem independently. Removing that friction is the benefit of a unified, opinionated model.


Visit our developer docs to learn more about[Finch’s field support](https://developer.tryfinch.com/integrations/field-support) , or[sign up for our sandbox](https://dashboard.tryfinch.com/signup?) to test it for yourself.
