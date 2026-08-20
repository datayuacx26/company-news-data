---
schema_version: "1.0.0"
document_id: "d04498b1b4b553bb7a5821007430e73116d108ae9514250162dd87f1d8024c76"
company_key: "verisk-analytics-inc-common-stock"
company: "Verisk Analytics Inc."
source_id: "verisk-analytics-inc-common-stock-news-import-737963da0dcd"
canonical_url: "https://www.verisk.com/blog/closing-the-section-111-wcmsa-reporting-compliance-gap/"
published_at: null
first_seen_at: "2026-08-07T10:09:49.193734+00:00"
fetched_at: "2026-08-07T10:09:50.889152+00:00"
content_hash: "sha256:da4ec2bb2b96448381ba9d2a9b13c747c756726a162c016b456d7a82ef129c9d"
---

# Closing the Section 111 WCMSA Reporting Compliance Gap

##


## How the new Section 111 WCMSA reporting requirements changed the math


[A year into CMS’s new Section 111 WCMSA reporting requirements](https://www.verisk.com/blog/tpoc-wcmsa-reporting-is-underway-unexpected-issues-and-considerations/) , the compliance equation has shifted on every Medicare beneficiary settlement.


For years, workers’ compensation carriers operated against a familiar line: settle a Medicare beneficiary’s claim below CMS review thresholds, and the often onerous Workers’ Compensation Medicare Set Aside (WCMSA) review process stayed out of the picture. That approach resulted in the Centers for Medicare & Medicaid Services (CMS) having limited visibility into future medical allocations in settlements below the $25,000 review threshold, leaving a blind spot for a significant number of claims.


That gap closed on April 4, 2025, when CMS began requiring additional information at the point of Section 111 settlement reporting through five new WCMSA fields. The agency indicated it wasn’t getting the settlement information needed to close out the claims it was reviewing, and the historical practice of following up with documentation after the fact had broken down across the industry. The fix was to require that data be provided at the point of settlement reporting.


A year in, the downstream effect is clear. CMS now sees workers’ compensation settlements that used to sit comfortably below its MSA review threshold. Because the agency rolled out[civil monetary penalties (CMPs)](https://www.verisk.com/blog/section-111-penalties-notice-appeal-and-payment/) in the same timeframe, the risk of inaccurate reporting moved from theoretical to material.


## The compliance gap hiding in your Section 111 data


A consistent pattern is showing up across casualty insurers since go-live: adjusters are defaulting to $0 in the new WCMSA fields, especially on settlements under the historical $25,000 threshold. Some carriers believe their teams have this covered. The reporting data often says otherwise.


The reasons are practical:


- The old rule of thumb was “$25,000 or above, consider an MSA.” The new posture is closer to “if you’re settling with a Medicare beneficiary at all, consider some form of MSA or future allocation.” That shift hasn’t fully propagated through every adjuster's desk.
- Adjusters carry 100–200 cases at a time. Medicare beneficiaries make up a fraction of that book, but carry an outsized share of the compliance exposure. There is no realistic path to training every adjuster up to specialist-level Medicare fluency on a sliver of their caseload.
- Experienced adjusters are retiring, and the institutional knowledge that flagged Medicare status as something special is leaving with them. New adjusters inherit the caseload but not always the playbook.
- When in doubt, $0 feels safe. Under the new rules, a consistent pattern of zeros is the opposite of safe; it’s a flag.


A wrong zero on a Medicare beneficiary settlement has two consequences. First, CMS notices the pattern. The full penalty ramifications are still catching up to the policy, but the exposure is real and growing. Second, the beneficiary is potentially hurt: They settle, no future allocation is recorded, and when they go in for an x-ray related to the work comp injury, Medicare may deny payment. They pay out of pocket.


## Closing the gap across the claims lifecycle


Verisk’s automated suite of Medicare compliance solutions addresses three specific considerations across the claim lifecycle:


1) identifying when an MSA is needed, through[MSA Link](https://www.verisk.com/493de0/siteassets/iso-claims-partners/downloads/msa-link.pdf)


2) producing a defensible allocation for small settlements, through the[Data-Driven MSA](https://www.verisk.com/493cfb/siteassets/iso-claims-partners/downloads/data-driven-msa.pdf)


3) validating what gets reported to CMS, through[Section 111 Data Sync](https://www.verisk.com/resources/campaigns/section-111-demo/) .


Each maps to a distinct point in the claim, and each requires functionality that general claims platforms weren’t designed to provide.


What makes a workflow genuinely connected *end-to-end* isn’t any single point solution. It's that Section 111 data, MSA data, and lien information coincide in one integrated environment. When an adjuster or compliance lead needs to address “I got an MSA,” “I want CMS submission,” “I have a lien I forgot about,” or “My Section 111 reporting says this, what's going on?”—those answers should come from one place, not four. That cross-system visibility enables your data to work harder across claim touchpoints.


That cross-system visibility is what separates a connected solution from a collection of point tools, enabling your data to work harder for your teams across departments.


***MSA Link: a Medicare-aware alert layer your system doesn’t have***


Claims systems do a lot of things well. Medicare compliance is not always one of them. MSA Link supplies the missing layer, working from the Section 111 claim data you already report today through MSP Navigator, with the architecture built to apply to any Section 111 solution set.


What that looks like in practice: When a claim involving a Medicare beneficiary reaches the point where settlement may be likely, MSA Link prompts the adjuster directly. It recognizes potential Medicare compliance needs and lays out the available paths against the relevant thresholds, whether that means a standard WCMSA or a data-driven allocation for below-thresholds settlements, or deferring the decision if the claim isn’t currently positioned for settlement. To help keep claims on track, MSA Link can automatically initiate an MSA if no action is taken within a reasonable window.


Two things make an alert layer useful in day-to-day claims handling. First, it focuses attention on the smaller population of Medicare beneficiary claims within an adjuster’s caseload but carry great compliance risk. Second, it surfaces at a moment when action is needed, rather than buried in a queue. Just as important, MSA Link can be configured to fit your workflow and protocols, so timing, default actions, and trigger criteria align with how your teams operate.


***Data-Driven MSA: a defensible allocation for the sub-$25K claims that used to get $0***


For settlements with a Medicare beneficiary under $25,000, a traditional MSA rarely makes economic sense. These claims typically have limited future medical needs, and the cost of a traditional MSA is prohibitive. Historically, that is why the future medical component of these claims was bypassed. As of April 4, 2025, ignoring this isn’t an option.


The Verisk Data-Driven MSA fills that gap. The approach uses Section 111 data already on file to produce an allocation that reflects the specific injury profile and the claimant's life expectancy, drawn from CDC tables. The models behind it draw on one of the largest workers’ compensation data sets in the industry, driving a credible number quickly and at a cost that fits the claim. The output is unique to the individual’s claim.


Three things the Data-Driven MSA delivers:


- **Fast turnaround.** A 48-hour delivery window keeps a settlement moving rather than stalling it.
- **Cost that fits the claim.** The whole point of a streamlined methodology is that the economics work for sub-$25K settlements. There are two costs to watch:


- The allocation itself can come back higher than the settlement, which stalls closing the claim.
- And obtaining a traditional allocation can be cost-prohibitive in its own right, eating up more in fees than the settlement.


A streamlined approach keeps both in check.


- **Defensibility.** The number delivered is a defensible allocation amount, supported by accepted ICD-10 codes, life expectancy, and the applicable state workers’ compensation fee schedule.


- A note on where this kind of allocation isn't the right fit: if the claim was denied, the carrier isn't responsible for past or future medicals, and the correct number is $0.
- If significant treatment is anticipated—a surgery on the horizon, for example—the eventual cost will almost certainly exceed $25,000, and the claim belongs on a different path. The Data-Driven MSA is designed for a specific profile: Medicare beneficiary, limited or no current treatment—accepted claim, and closing future medical needs.


***Section 111 Data Sync: a validation layer between your adjuster and CMS***


Section 111 reporting is where the WCMSA amount becomes part of CMS’s record, making accuracy before submission critical. As CMS continues to operationalize the new requirements, the pre-submission validation step in Section 111 Data Sync can help carriers reduce avoidable errors and improve reporting consistency.


Section 111 Data Sync catches two common issues.


1. First, it flags mismatches between what’s about to be reported and what’s already on file. For example, when an approved MSA exists but a different amount is entered in the Section 111 fields.
2. Second, it helps confirm that the amount in the settlement documentation matches what will be reported, especially when multiple figures appear throughout the documents and it’s not immediately clear which one should be reported.


This gives adjusters a clearer review point before submission and helps carriers strengthen reporting accuracy as the new process continues to take shape.


## Section 111 WCMSA reporting compliance in practice: How it comes together on a single claim


A composite walk-through:


Picture a Medicare beneficiary settling a work comp claim under the new rules. MSA Link surfaces the claim when settlement starts to look realistic and points the adjuster toward the right path—standard MSA, Data-Driven MSA, or a deferred decision. If a Data-Driven MSA allocation is the right fit, it comes back within 48 hours, and settlement moves forward. When the adjuster goes to report through Section 111, Section111 Data Sync checks the numbers against what’s already on file and against the settlement document itself, catching any mismatches before it becomes CMS’s official record.


Trigger at the front end. Allocation in the middle. Validation at the back end. Each closes a gap the others can’t. Together, they keep the claim from defaulting to the wrong number at any point along the way.


## If you do one thing


Of the three, the most direct line to CMP exposure runs through Section 111 reporting. If you can only put one piece in place first, start with **Section111 Data Sync.** What arrives at CMS via Section 111 is what CMS treats as final, and a pre-submission check is the difference between catching an error and explaining one.


All three matter. But validation is where the penalty teeth live, and it’s the most concrete step you can take this quarter to make sure the right number is the one CMS keeps on file.


## What “good” looks like in 2026


Two years ago, “good” Section 111 compliance meant accurate reporting on settlements with a Medicare beneficiary. In 2026, “good” means substantial, holistic compliance addressing future medical needs for all workers’ compensation claims settling with a Medicare beneficiary with the right allocation prompted at the right point in the claim lifecycle, and validation at the moment of submission, so the wrong number never becomes CMS’s authoritative record.


Verisk’s automation suite makes that the default, not the exception.
