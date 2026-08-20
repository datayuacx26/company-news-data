---
schema_version: "1.0.0"
document_id: "7fec386e32832e18d69b4c99d521d1b3b518b846d9355f0d829bdc51b50d036a"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-calculate-net-revenue-retention-nrr-with-sql/"
published_at: "2026-08-16T00:00:00+00:00"
first_seen_at: "2026-08-16T13:57:34.054400+00:00"
fetched_at: "2026-08-16T13:57:37.537863+00:00"
content_hash: "sha256:1979d2db62db1366912939bb60904c082da2cb75bdf586aa8008f5f6799f0a7a"
---

# How to calculate net revenue retention (NRR)

Net revenue retention (NRR) measures how much recurring revenue you keep and grow from the customers you already had at the start of a period, ignoring any new customers you signed during it. The formula is` NRR = (Starting MRR + Expansion - Contraction - Churn) / Starting MRR` , expressed as a percentage. An NRR above 100% means your existing base grew on its own, before you closed a single new deal. Below 100% means the base is shrinking and new sales have to run uphill just to keep total revenue flat.


This guide is for founders, finance and RevOps operators, and analysts who want to compute NRR directly from their billing or subscription data instead of trusting a number that shows up in a board deck with no query behind it. It covers the exact formula, a worked numeric example, how to calculate NRR and gross revenue retention (GRR) in SQL off a monthly MRR table, whether to measure it monthly or annually, realistic benchmarks, and the mistakes that quietly make the number wrong.


## What net revenue retention actually measures


NRR answers one question: if you had stopped acquiring customers at the start of the period, what would have happened to the recurring revenue from the customers you already had? It rolls four movements into a single percentage:


- **Starting MRR** (or ARR): recurring revenue from a fixed cohort of customers at the start of the period.
- **Expansion** : upsells, cross-sells, seat increases, and usage growth within that same cohort.
- **Contraction** : downgrades and reduced usage within the cohort.
- **Churn** : revenue lost from customers in the cohort who cancelled entirely.


The critical rule is that revenue from brand-new customers is excluded. New logos are counted in new-business metrics, not retention. NRR is a health check on the base you already own, which is why investors weight it heavily: it isolates whether the product delivers compounding value or leaks it.


A closely related metric, gross revenue retention (GRR), uses the same cohort but ignores expansion. It caps each customer at their starting revenue, so it can never exceed 100%. GRR tells you how much you keep before any upsell; NRR tells you what happens after expansion is layered on.


Net revenue retention (NRR) Gross revenue retention (GRR)


Includes expansion Yes No


Can exceed 100% Yes No (capped at 100%)


Best read as Compounding of the existing base Leakage floor before upsell


Typical use Growth efficiency, investor reporting Churn severity, product stickiness


NRR is also called net dollar retention (NDR) or net MRR retention. The terms are interchangeable; pick one and use it consistently.


## How to calculate NRR: a worked example


Take a cohort of customers generating $100,000 in MRR on January 1. Over the period:


- Some upgrade, adding $18,000 in expansion.
- Some downgrade, removing $5,000 in contraction.
- Some cancel, removing $12,000 in churn.


Apply the formula:


```text
NRR = (100,000 + 18,000 - 5,000 - 12,000) / 100,000
= 101,000 / 100,000
= 101%
```


GRR for the same cohort strips out the $18,000 of expansion:


```text
GRR = (100,000 - 5,000 - 12,000) / 100,000
= 83,000 / 100,000
= 83%
```


The gap between 101% NRR and 83% GRR is the story. This business is holding total revenue flat, but only because a handful of expanding accounts are papering over real churn. Reading the two numbers together is the point. A strong NRR built on a weak GRR floor is fragile: it unravels the moment one of the expanding accounts leaves.


## How to calculate NRR in SQL


The cleanest way to compute retention is off a table with one row per customer per month holding their MRR, something like` mrr_by_customer(customer_id, month, mrr)` . Most billing systems or a modeled table in your warehouse can produce this. The elegant part: you never have to classify expansion and churn by hand. If you take the starting cohort and sum their MRR at the end of the period, that end number already nets expansion, contraction, and churn, because churned customers simply have zero end MRR.


For annual NRR measured from January 2025 to January 2026:


```text
with   start_mrr   as   (
select   customer_id, mrr   as   start_mrr
from   mrr_by_customer
where   month   =   date   '2025-01-01'
and   mrr   >   0
),
end_mrr   as   (
select   customer_id, mrr   as   end_mrr
from   mrr_by_customer
where   month   =   date   '2026-01-01'
)
select
round  (  100  .  0   *   sum  (  coalesce  (  e  .  end_mrr  ,   0  ))   /   sum  (  s  .  start_mrr  ),   1  )   as   nrr_pct
from   start_mrr s
left join   end_mrr e   on   e  .  customer_id   =   s  .  customer_id  ;
```


The` left join` from the starting cohort is what enforces the rule that new customers are excluded: anyone who was not paying on the start date never enters the calculation.


To turn that single number into something you can act on, break the same cohort into its expansion, contraction, and churn buckets. This is the revenue waterfall behind the NRR figure:


```text
with   start_mrr   as   (
select   customer_id, mrr   as   start_mrr
from   mrr_by_customer
where   month   =   date   '2025-01-01'   and   mrr   >   0
),
end_mrr   as   (
select   customer_id, mrr   as   end_mrr
from   mrr_by_customer
where   month   =   date   '2026-01-01'
),
change   as   (
select   s  .  start_mrr  ,   coalesce  (  e  .  end_mrr  ,   0  )   as   end_mrr
from   start_mrr s
left join   end_mrr e   on   e  .  customer_id   =   s  .  customer_id
)
select
sum  (start_mrr)                                                                as   starting_mrr,
sum  (  case   when   end_mrr   >   start_mrr   then   end_mrr   -   start_mrr   else   0   end  )        as   expansion,
sum  (  case   when   end_mrr   <   start_mrr   and   end_mrr   >   0   then   start_mrr   -   end_mrr   else   0   end  )   as   contraction,
sum  (  case   when   end_mrr   =   0   then   start_mrr   else   0   end  )                          as   churned,
round  (  100  .  0   *   sum  (end_mrr)   /   sum  (start_mrr),   1  )                               as   nrr_pct,
round  (  100  .  0   *   sum  (  least  (end_mrr, start_mrr))   /   sum  (start_mrr),   1  )             as   grr_pct
from   change;
```


The` least(end_mrr, start_mrr)` term is the trick for GRR: it caps every customer at their starting revenue so expansion cannot count, which is exactly what gross retention requires. One query now returns the starting base, the three movement buckets, NRR, and GRR together.


## Should you measure NRR monthly or annually?


Both are valid, and they answer different questions.


- **Monthly NRR** compares each month to the prior month. It reacts fast, so it is useful for spotting a sudden spike in downgrades. It is also noisy, because annual contracts, mid-term upgrades, and billing timing create lumpiness that a single month exaggerates.
- **Annual NRR** compares a cohort to itself twelve months later. It smooths out contract timing and is the version investors and boards expect. Most published benchmarks are annual.


A common compromise is a trailing twelve-month NRR calculated every month, which gives you a smooth annual figure that still updates monthly. If you report a monthly number internally and an annual number externally, label each clearly so no one compares a 101% annual figure against a 99.2% monthly one and panics.


## What is a good NRR benchmark?


Benchmarks only mean something within your segment, because retention correlates strongly with average contract value (ACV): a $20 per month product and a $250,000 per year product retain very differently. According to[SaaS Capital’s 2025 retention research](https://www.saas-capital.com/blog-posts/what-is-a-good-retention-rate-for-a-private-saas-company/) , the median NRR across private B2B SaaS companies was about 101%, with median GRR around 91%, and both climb as ACV rises. SaaS Capital also frames GRR as table stakes: to have a shot at parity with peers, gross retention generally needs to clear roughly 90%.


For a directional target, Bessemer Venture Partners’ widely cited framing puts 100% NRR at “good,” 110% at “better,” and 120% or higher at “best,” though that scale was calibrated for growth-stage enterprise SaaS. At SMB price points, holding 100% is genuinely strong; at enterprise scale, top performers push well past 120%. Treat any single median as a starting line, not a goal, and benchmark against companies at your ACV.


## Common mistakes that make NRR wrong


Most incorrect NRR numbers come from a handful of avoidable errors. Run through this before you trust the figure:


- **Counting new customers in the numerator.** NRR is about the starting cohort only. If a customer who signed up mid-period shows up in your end-of-period sum, the number is inflated. The` left join` from the start cohort above prevents this.
- **Netting at the aggregate level instead of per customer.** You cannot subtract total churn from total expansion across the whole book and call it right. Classify each customer individually as expansion, contraction, or churn, then sum the buckets.
- **Mixing monthly and annual windows.** A monthly figure and an annual figure are not comparable. Pick one per report and label it.
- **Letting non-recurring revenue leak into MRR.** One-time setup fees, professional services, and usage overages that are not truly recurring distort the base. Normalize to recurring revenue first.
- **Mishandling reactivations.** A customer who churned and later resubscribed should not be quietly folded back into the original cohort as expansion. Decide the rule and apply it consistently.
- **Reading NRR without GRR.** High NRR can hide severe churn masked by a few large upsells. Always report the pair.
- **Currency and discount drift.** Foreign-currency conversions and promotional discounts that change over the period can move MRR without any real customer behavior. Hold them constant or account for them explicitly.


## How to track NRR in a dashboard


A single NRR number is a checkpoint; the trend and its components are where decisions live. A useful retention view shows:


- NRR and GRR plotted together as trailing twelve-month lines, so the gap between them is visible at a glance.
- The monthly revenue waterfall (starting MRR, expansion, contraction, churn) as a stacked bar, so you can see whether a dip came from more churn or less expansion.
- NRR segmented by plan, ACV band, or acquisition cohort, since a blended number hides which segments are actually leaking.


The practical challenge is keeping this live rather than rebuilding it in a spreadsheet every month. Because the calculation runs entirely in SQL against your subscription data, you can point a tool like[Basedash](https://www.basedash.com/) directly at your billing database or warehouse, save the waterfall query, and let it refresh on its own. That also lets a non-technical teammate open the NRR chart, filter to a segment, and ask a follow-up question without waiting on the analyst who wrote the query. If you are building a broader view, this metric slots naturally into a[SaaS revenue dashboard](https://www.basedash.com/blog/how-to-build-a-saas-revenue-dashboard-metrics-data-sources-and-structure) alongside MRR and churn, and it belongs in most[board reporting](https://www.basedash.com/blog/how-to-build-a-board-reporting-dashboard-metrics-structure-and-cadence) packs. For the segment view, pairing NRR with[cohort analysis](https://www.basedash.com/blog/how-to-do-cohort-analysis-retention-revenue-and-behavioral-cohorts) shows which signup cohorts expand and which decay.


## FAQ


### What is a good net revenue retention rate?


It depends on your segment. Median private B2B SaaS NRR sits around 101%, but that blends very different businesses. As a directional scale, 100% is good, 110% is better, and 120% or higher is best, though those targets were framed for enterprise SaaS. At SMB price points, holding 100% is strong; at enterprise scale, top performers exceed 120%. Benchmark against companies at your average contract value, not the blended median.


### What is the difference between NRR and GRR?


Both track the same starting cohort of customers, but gross revenue retention ignores expansion and caps each customer at their starting revenue, so it can never exceed 100%. Net revenue retention includes expansion, so it can go above 100%. GRR shows your leakage floor before any upsell; NRR shows what happens after expansion. Read them together, because strong NRR can mask weak GRR.


### Do you include new customers in NRR?


No. NRR measures only the customers who existed at the start of the period. Revenue from customers acquired during the period is counted in new-business metrics, not retention. Including new logos inflates the number and defeats the purpose, which is to isolate how the existing base behaves on its own.


### Is net revenue retention the same as net dollar retention?


Yes. Net revenue retention (NRR), net dollar retention (NDR), and net MRR retention refer to the same calculation. Different companies and reports use different names, but the formula is identical: starting recurring revenue plus expansion minus contraction and churn, divided by starting recurring revenue.


### Should I calculate NRR monthly or annually?


Use annual NRR for board and investor reporting, since it smooths out contract timing and matches most published benchmarks. Use monthly NRR internally to catch sudden changes early, accepting that it is noisier. A trailing twelve-month calculation updated each month gives you a smooth annual figure that still moves monthly.
