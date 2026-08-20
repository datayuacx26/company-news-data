---
schema_version: "1.0.0"
document_id: "c055e322eb87b7b1126219d48f6a3716dcea446d6f61716c2218a67df8296261"
company_key: "yc-serif-health"
company: "Serif Health"
source_id: "yc-serif-health-news-import-98e88b19e297"
canonical_url: "https://www.serifhealth.com/blog/baselining-price-transparency-data-to-medicare"
published_at: "2025-04-23T23:00:00+00:00"
first_seen_at: "2026-07-22T13:10:17.126339+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:83b43b17805a8f404655fec37f43919f3ca4fe76cda8c232bad1b85232c6bb0e"
---

# Baselining price transparency data to Medicare

Some of the more common requests we see from customers utilizing price transparency data involve transforming a posted dollar ($) amount into some kind of percentage or conversion factor commonly used for contracting reimbursement – e.g.,


> What is ‘X’ hospital’s inpatient base rate?


> What is ‘Y’’s specialty physician group’s % of CMS on E&M?


> What is ‘Z’s anesthesia group’s conversion factor?


At Serif Health, we’ve built capabilities to derive answers to these questions seamlessly using our platform, Signal, and other custom applied analytics we offer on top of our gold-tier data-set.


## Background


Those familiar with price transparency data know the values shown at a row-level appear by procedure code. For instance, if you wanted to see what Medical City Dallas receives for an inpatient hip replacement, you would search Medical City Dallas’ EIN (621682198) and inpatient MS-DRG code (DRG 470) and see an allowed with BCBS TX PPO of ~$72K:


Now, this kind of data is useful if you were really interested in understanding healthcare costs for different procedures but let’s say you fit one of these other personas instead:


**Director, Network Analytics @ BCBS** : You’d want to know what % of Medicare or what DRG-conversion factor Medical City Dallas uses for inpatient and if United, Cigna, or Aetna have a better deal.


**Managed Care Contracting @ HCA** : You’d also want to know what % of Medicare or what DRG-conversion other health systems in DFW-Texas benchmark at (e.g., Baylor, UTSW) to see if BCBS gave you a fair price.


Most stakeholders involved in setting contracts that determine the prices in healthcare do not think in terms of $ rates by code. They understand healthcare cost as a percentage of what Medicare would pay (i.e., % of CMS) or some other kind of conversion factor.


This begs the question: what inputs lead to the $72K for DRG-470 we see posted?


One contracting methodology is percent of Medicare. Simply put, BCBS-TX and HCA could agree that Medical City Dallas should receive some multiplier on what Medicare would pay on inpatient care. On the inpatient side, the Medicare baseline (e.g., ‘what Medicare would pay’) could be more general like the national average CMS payment amount or be more specific to what Medicare pays Medical City Dallas based on their unique pricer.


Another contracting methodology is multiplying a conversion factor by various diagnosis related group weights. This would mean HCA would agree on a conversion factor (e.g., ~$40K) and what gets paid per inpatient procedure would be the $40K conversion factor multiplied by that specific inpatient DRG weight (e.g., ~1.75). These weights can be specific to the payer, come from Medicare, or be derived some other way.


While the discussion above focuses primarily on inpatient contracting, similar heuristics (e.g., backwards-deriving conversion factors or % of CMS) can apply to other service lines as well.


## Finding the right ‘baselines’


**Example 1)** Inpatient conversion factors - let’s say you wanted to know Baylor’s inpatient conversion factor with BCBS ->


Step 1: Grab the posted price transparency dollar amounts for some inpatient DRGs:


Step 2: Divide the $ amounts by the appropriate DRG weights for each code and you’ll get a consistent inpatient conversion factor that is 15,586:


**Example 2)** Anesthesia conversion factors - taking similar steps as finding the inpatient conversion factors but dividing by the number of anesthesia base units instead:


[Watch a demonstration of re-scaling Anesthesia Data](https://www.loom.com/share/50e6b3b13528432d97ba72c59881b7d2?t=11&sid=b60cc233-9063-4d4c-8f39-96a3b93777ef)
**Example 3)** Percent of Medicare - to represent a contract effectively as a % of CMS requires several key pieces ->


Step 1: Check if the $ amounts shown in price transparency data were actually generated using a multiplier on Medicare.


As we have discussed in[another blog](https://www.serifhealth.com/blog/seeing-value-based-contracts-in-the-payer-price-transparency-data) , sometimes price transparency data can show case-rates (e.g., a flat payment amount for ED / urgent care visits), percentage of charges, per-diems, or the conversion factors directly.


In these scenarios, base-lining to Medicare does not make sense because none of these rates rely on multiplying out Medicare as part of their contracting methodology. Flagging values that are perfect integers is typically the best way to exclude these kinds of arrangements from any Medicare comparison.


For instance, BCBS-TX posted some kind of a flat $45K placeholder value for Memorial Hermann across several DRGs -


Dividing these by a Medicare value would be incorrect since you are comparing apples-to-oranges. For reference, including these values would lead to an inpatient % of Medicare that under-states the true cost by ~100% of CMS!


Step 2: Bring in the right Medicare payment values.


Medicare releases many different fee schedules for hospital inpatient, hospital outpatient, ASCs, drugs, labs, SNFs, physician groups, etc. These update at different times and sometimes reflect differences in payments by specific facility, region, site of service (facility vs non-facility), and various billing modifiers.


Making sure you bring in the right baseline for a given row of data is key.


For example, baselining ASC payment rates to hospital outpatient Medicare payments would make it seem the median facility fee for a colonoscopy is 50% of Medicare:


Even after switching from hospital outpatient to ASC Medicare payments, baselining to the ASC national payment amount will likely continue to under-state true cost given ~92% of Medicare is also likely below what a commercial entity would accept:


Finally, bringing in the right ASC fee schedule for Georgia’s Medicare Administrative Contracting (MAC) entities from 2024 leads to a ~105% of CMS standard fee schedule that aligns with market feedback:


At Serif Health, we have coverage into all of these various fee schedules CMS publishes and have derived algorithms based on customer feedback, claims, and market knowledge to generate accurate Medicare benchmarks to account for the nuances and data discrepancies mentioned above.


If you are interested in learning more about how we support managing care analytics on price transparency data, please reach out tohello@serifhealth.com or schedule some time with us to chat[here](http://calendly.com/serif-health/30min) .
