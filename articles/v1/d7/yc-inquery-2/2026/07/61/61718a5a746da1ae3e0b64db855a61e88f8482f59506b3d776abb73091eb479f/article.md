---
schema_version: "1.0.0"
document_id: "61718a5a746da1ae3e0b64db855a61e88f8482f59506b3d776abb73091eb479f"
company_key: "yc-inquery-2"
company: "InQuery"
source_id: "yc-inquery-2-news-import-b28146ce019a"
canonical_url: "https://www.inquery.ai/post/adjuster-medical-summary-mistakes-claims-leakage/"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-25T09:40:05.993244+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:f65bf833c607f45b2bb3635f606a18c0fb7cbdc6b24d956b811bae4aa2a7b529"
---

# Medical Summary Mistakes That Drive Claims Leakage for Adjusters

Claims leakage is the money a carrier pays beyond what a claim was actually worth.


A surprising share of it starts in the medical record review.


That review is the summary an adjuster relies on to set reserves, test causation, and negotiate settlement.


When the summary misses a pre-existing condition, a treatment gap, or a recoverable lien, the dollars leak quietly and rarely come back.


This guide walks through eight medical summarization mistakes that drive leakage on bodily injury and workers’ comp files.


For each one, you get the mechanism that costs money and the check that stops it.


If you want the vendor-by-vendor view first, our[medical summary software comparison for adjusters and carriers](https://www.inquery.ai/post/medical-record-summary-software-adjusters-carriers-2026) lines up the major platforms against carrier requirements.


## What Claims Leakage Is and Why Summaries Drive It


Claim audits routinely put leakage at 20 to 30 percent of claim spend, and most of it is not fraud.


It is small, defensible-looking overpayments that stack across thousands of files.


That is exactly why it is hard to see.


The medical summary sits upstream of nearly every leakage point.


It feeds the reserve.


It frames causation.


It flags — or fails to flag — recovery rights.


Get the summary right and the downstream numbers get more accurate.


Get it wrong and the error compounds through the entire life of the claim.


### The Numbers Behind Leakage


Leakage hides inside averages.


A single file over-reserved by $4,000 looks like rounding.


Multiply it across a quarterly inventory and it becomes a budget line.


Auto bodily injury is the most-studied line.


The[Insurance Information Institute](https://www.iii.org/article/background-on-no-fault-auto-insurance) documents how disputed injury severity and causation drive litigated BI cost — the exact questions a medical summary is supposed to answer.


### Why Record Review Is the Control Point


No adjuster re-reads every page of a 600-page file.


The summary becomes the record of truth in practice.


If that summary is incomplete, the adjuster decides on a partial picture and does not know it.


That is what turns summarization quality into a financial control, not a clerical task.


The[NAIC auto insurance guidance](https://content.naic.org/insurance-topics/auto-insurance) treats causation accuracy as central to fair BI handling for the same reason.


## The Reserve-Setting Mistakes


The first reserve anchors the file, and a bad summary anchors it in the wrong place.


Reserves set within 14 days of first notice tend to develop with less volatility later.


That early number is only as good as the medical picture behind it.


### Setting Reserves on a Partial Summary


A partial summary is worse than no summary, because it looks complete.


When records from one provider are still outstanding, the summary should say so on the first page.


Miss that note, and an adjuster reserves as if the file is whole.


Our[missing records data management guide](https://www.inquery.ai/post/missing-records-data-management-2025) covers how to reserve responsibly while records are still in transit.


**The fix:** require every summary to open with a records-received inventory — provider, date range, page count, and any known gaps.


### Ignoring Outlier Bills and Provider Totals


Billed amounts drive reserves, and outliers drive leakage.


A single surgical bill or an inflated facility charge can swing exposure by five figures.


The summary needs every CPT code, billed amount, and provider total rolled up cleanly, with outliers flagged for follow-up.


A summary that reports “significant treatment” without the numbers forces the adjuster to guess high.


**The fix:** demand line-item billing extraction, not narrative descriptions of cost.


## The Causation Mistakes


Causation is where the largest defensible savings live, and where weak summaries give them away.


Every dollar of treatment tied to a prior condition is a dollar the carrier should not pay on this claim.


### Missed Pre-Existing Conditions


A prior lumbar injury from three years before the loss changes causation entirely.


Those signals are buried across intake forms, prior imaging references, and medication histories.


A summary that only reads the post-loss records will never surface them.


Missed pre-existing conditions are one of the most common and most expensive summarization failures on the carrier side.


Our post on[medical record summary mistakes in personal injury cases](https://www.inquery.ai/post/medical-record-summary-mistakes-personal-injury-cases) covers the same failure from the plaintiff angle.


### Undetected Treatment Gaps


A long gap in care undercuts claimed injury severity.


Gaps only mean something when the summary is date-ordered and complete enough to see them.


A 90-day gap between the ER visit and the first PT session is a negotiation lever — but only if it is on the page.


**The fix:** require a date-ordered treatment timeline that flags any gap longer than a defined threshold.


Our[medical records gap analysis guide](https://www.inquery.ai/post/ai-medical-records-gap-analysis-personal-injury) explains how automated gap detection works.


## The Recovery Mistakes


Recovery rights are money the carrier is owed, and a summary that misses them leaks on the back end.


Liens, subrogation, and Medicare obligations all appear in the medical records long before they appear anywhere else.


### Overlooked Liens and Subrogation


Health insurance liens, ERISA recovery rights, and hospital liens hide inside billing and correspondence.


Miss them in the summary and the carrier settles without accounting for recovery.


The money is gone by the time anyone notices.


Recovery after settlement is rare.


**The fix:** make lien and subrogation identification an explicit summary field, not a hope.


### Missed Medicare Set-Aside Triggers


Medicare Secondary Payer obligations attach to specific claim profiles.


Missing them creates compliance exposure on top of leakage.


The[CMS coordination of benefits and recovery program](https://www.cms.gov/medicare/coordination-benefits-recovery/overview) sets the rules for when Medicare’s interest must be considered.


A summary that surfaces Medicare status and set-aside triggers early saves both dollars and a downstream penalty.


## The Defensibility Mistakes


A finding you cannot verify is a finding you cannot defend, and undefendable findings collapse under dispute.


This is where triage-grade summaries and litigation-grade summaries part ways.


### Summaries Without Source Links


Every extracted fact — diagnosis, procedure date, billed amount — should link to the exact page in the source record.


Without that link, an AI finding is an assertion, not evidence.


Opposing counsel cannot challenge a fact without challenging its source page, which is why source-linking is the floor for any claim with litigation exposure.


[InQuery](https://www.inquery.ai/) treats page-level source-linking as the default.


Many AI-only tools produce summaries that read well but cannot be verified line by line.


The[medical record summary guide](https://www.inquery.ai/post/medical-record-summary-guide-ai) walks through what defensible output looks like.


### One Format for Every Line of Business


A BI summary and a workers’ comp summary answer different questions.


Workers’ comp adds compensability and return-to-work analysis.


SIU adds fraud-pattern review — the same triage InQuery’s[SIU review desk](https://www.inquery.ai/services/siu-review-desk) runs before a carrier commits to a full investigation.


Forcing one template across all three buries the findings each line actually needs.


Line of Business What the Summary Must Surface


Auto BI Causation, pre-existing conditions, billed specials, liens


Workers’ comp Compensability, MMI, work restrictions, return-to-work


General liability Mechanism of injury, treatment gaps, prior claims


SIU referral Provider patterns, billing anomalies, inconsistent history


## A Quick Scorecard: Triage vs. AI-Only vs. Source-Linked with QA


Not every summary is built to stop leakage, and the differences show up exactly where the money is.


The table below compares three tiers of medical summarization against the criteria that decide whether leakage gets caught.


Criterion Manual Triage AI-Only Summary Source-Linked + Human QA


Reserve-relevant billing Inconsistent Usually captured Captured and verified


Pre-existing condition flags Depends on reviewer Sometimes missed Flagged with source page


Lien / subrogation detection Rare Partial Explicit field


Defensible at deposition No No Yes


Cost per 200-page file High labor cost Low Moderate


### How to Read the Scorecard


The AI-only column is a real improvement over manual triage on speed and cost.


The gap is defensibility.


On a high-exposure file, an unverifiable finding is a liability, which is why the source-linked tier exists.


## How Adjusters Catch These Mistakes Before They Cost Money


The carriers that get real value from AI review treat evaluation as a test, not a procurement formality.


Three habits separate teams that stop leakage from teams that just move files faster.


### Build the Pilot on Your Hardest Files


Vendors quote 92 to 97 percent accuracy on clean digital records.


Performance falls on faxed records, handwritten notes, and scanned EHR printouts.


Those are the exact documents that dominate high-volume claims, so run the pilot on them — not the sample set the vendor hands you.


### Require Source Links and a QA Layer


At a 3 percent error rate, roughly one in 30 summaries carries a material miss.


A human QA step before delivery pushes error rates below 1 percent.


For high-exposure claims, that difference is the whole business case.


The platform should build human review into the standard delivery flow rather than charging for it as an add-on.


Our[platform features evaluation guide](https://www.inquery.ai/post/medical-summarization-platform-features-evaluation-guide) gives a structured way to score vendors on both.


### Measure Cycle Time, Not Just Turnaround


Turnaround time is how fast the vendor returns a summary.


Cycle time is how fast the file moves from first notice to a set reserve.


The second number is the one that touches leakage, so measure it directly and compare it against your current outsourced spend.


For the financial model behind that comparison, see our[medical summary software ROI analysis for carriers](https://www.inquery.ai/post/medical-summary-software-roi-insurance-carriers) .


## Where InQuery Fits


InQuery was built for both claims and legal document review, which is why its output holds up on the carrier side of a disputed file.


Every summary is source-linked to the original page.


Every output passes a human QA review before delivery.


The security posture meets carrier procurement out of the box.


It maps to enterprise claims systems like[Guidewire ClaimCenter](https://www.guidewire.com/products/core-products/insurancesuite/claimcenter-claims-management-software) and[Duck Creek Claims](https://www.duckcreek.com/product/claims-management-software/) through a REST API.


PHI is handled under a program aligned to the[NAIC Insurance Data Security Model Law](https://content.naic.org/insurance-topics/cybersecurity) .


### The Difference for Carriers


Compared with carrier-focused tools like[Wisedocs](https://www.wisedocs.ai/product/medical-chronologies) and[DigitalOwl](https://www.digitalowl.com/self-serve/pricing) , or plaintiff-first platforms like[Supio](https://www.supio.com/products/medical-chronologies) and[Casemark](https://casemark.com/features/medical-chronologies) , the differentiator is the pairing of source-linked output with a mandatory QA layer.


That combination is what converts a fast summary into a defensible one.


The[building for security guide](https://www.inquery.ai/post/building-security-2025) explains why that bar matters for carrier data, and the[value calculator](https://www.inquery.ai/value-calculator) models the savings against your current review spend.


## Frequently Asked Questions


### What is claims leakage in medical record review?


Claims leakage is any payment above the claim’s true value.


In medical review, it comes from summaries that miss pre-existing conditions, treatment gaps, billing outliers, or recovery rights — each of which shifts the reserve or settlement in the claimant’s favor.


Better summarization is one of the few controls that reduces leakage without slowing the file.


### How much of claims leakage comes from bad medical summaries?


There is no single published figure, but medical review touches reserves, causation, and recovery — three of the largest leakage categories on injury files.


Because the summary feeds all three, improving it has outsized effect relative to the effort.


Our[sorting, indexing, and data extraction guide](https://www.inquery.ai/post/ai-medical-records-sorting-indexing-data-extraction) shows where summarization sits in the wider claims workflow.


### Can AI medical summaries be trusted for setting reserves?


AI-only tools reach 92 to 97 percent accuracy on clean records and less on faxed or handwritten files.


That is enough for triage and initial reserves.


For high-exposure claims, a human QA layer that pushes accuracy above 99 percent is worth the difference.


Ask every vendor for accuracy on your hardest document types, not their samples.


### How does source-linked summarization reduce claims leakage?


[InQuery](https://www.inquery.ai/) produces source-linked summaries with a mandatory human QA layer, so reserve-relevant billing, pre-existing conditions, and liens are flagged and verifiable to the page.


Carriers using it typically see lower per-review cost and faster cycle time on routine BI claims.


[Get started](https://www.inquery.ai/get-started) to scope a pilot on your own files.


### What should a carrier require in a medical summary vendor?


Require a records-received inventory, line-item billing extraction, date-ordered treatment timelines with gap flags, explicit lien fields, page-level source links, and a human QA step.


Confirm the vendor returns structured data and integrates with your claims system.


The evaluation checklist above turns that list into a scorecard.


---


**About the Author**


Erick Enriquez is CEO and Co-Founder of[InQuery](https://www.inquery.ai/) , the AI medical record summarization and chronology platform built for personal injury firms, insurance carriers, and IME providers. He holds a Bachelor’s in Mathematical and Computational Sciences and a Master’s in Computer Science from Stanford University.


He has spent his career building production AI systems for high-stakes document workflows.
