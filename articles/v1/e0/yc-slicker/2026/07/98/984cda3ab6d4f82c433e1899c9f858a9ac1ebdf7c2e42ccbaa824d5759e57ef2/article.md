---
schema_version: "1.0.0"
document_id: "984cda3ab6d4f82c433e1899c9f858a9ac1ebdf7c2e42ccbaa824d5759e57ef2"
company_key: "yc-slicker"
company: "Slicker"
source_id: "yc-slicker-news-import-4c2e7cff2c72"
canonical_url: "https://www.slickerhq.com/resources/lift-evaluation-protocol"
published_at: null
first_seen_at: "2026-07-22T13:50:21.965986+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:17ae278e729b560d10b59ca4c483ed666fc01f2b359acdd9c559b0b9e158ffda"
---

# Prove It, Don't Promise It

The problem


## A number is not evidence by default


A different customer mix, a favourable date window, a metric quietly redefined, or a rollout that favoured easy cases can all create apparent lift without the method doing anything. Before you accept a lift claim, name the ways it can be wrong.


> “How do I know the measured lift is real, and not an artefact of the setup?”
>
>
> The one question this paper answers.


Recovery rate, with 95% confidence intervals


Worked example from the paper: control versus Slicker on identical traffic.


A chart is only the output. The evidence is the setup behind it: comparable groups, mature outcomes, and success criteria fixed before the data is seen.


### Confounding


The two groups differ in composition, not just treatment. Hand one arm easier invoices and it outperforms for reasons that have nothing to do with the method.


### Selection bias


Non-random assignment changes the comparison set. Letting the system opt in the cases it is likely to recover is the classic example.


### Regression to the mean


Retry anything after a failure spike and it will appear to improve, because the spike was partly noise that was always going to subside.


### Cherry-picked windows


A favourable fortnight can overstate performance. A test that spans one convenient calendar window inherits that window's luck.


### Simpson's paradox


An aggregate lift can reverse inside every segment. The most counter-intuitive failure, and one that survives a casual review.


None of these require intentional bias. Only an uncontrolled test. Every one has the same antidote: a design that controls composition and checks itself.
