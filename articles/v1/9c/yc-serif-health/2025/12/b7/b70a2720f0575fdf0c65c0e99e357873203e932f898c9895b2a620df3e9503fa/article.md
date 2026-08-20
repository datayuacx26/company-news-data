---
schema_version: "1.0.0"
document_id: "b70a2720f0575fdf0c65c0e99e357873203e932f898c9895b2a620df3e9503fa"
company_key: "yc-serif-health"
company: "Serif Health"
source_id: "yc-serif-health-news-import-98e88b19e297"
canonical_url: "https://www.serifhealth.com/blog/cms-tic-rule-update-2025"
published_at: "2025-12-23T02:22:00+00:00"
first_seen_at: "2026-07-24T01:12:39.675945+00:00"
fetched_at: "2026-07-28T22:24:50.551214+00:00"
content_hash: "sha256:43412168d47b478d613d6e2e7f35486b5dcd614179ecd67be0d207197e132ada"
---

# Transparency in Coverage Evolves: CMS Signals a More Usable, Sustainable Data Future

Last Friday, on **December 19, 2025** , CMS—together with the Departments of Labor and Treasury—announced a major **proposed rule** to update the[Transparency in Coverage (TiC)](https://www.cms.gov/newsroom/fact-sheets/transparency-coverage-proposed-rule-cms-9882-p) payer price transparency regulations ([CMS-9882-P](https://www.federalregister.gov/documents/2025/12/23/2025-23693/transparency-in-coverage) ). CMS frames the proposal as an effort to make public disclosures **more accessible, more standardized, and more reliable** .


To our team at Serif Health, this is long overdue! We’ve been blogging about the need to **reduce bloat, clarify context, and make the data navigable** since **** 2023 **,** and **** Serif Health already takes steps to enrich the data internally in many of the ways CMS proposes. Our[2025 State of the Data Report](https://www.serifhealth.com/l/2025-state-of-the-data-report) summarizes many of these enhancements.


Here’s what stood out to us with our viewpoint on each major change area:


## **1) A direct attack on “clinically implausible” provider-rate combinations (aka zombie rates)**


CMS is proposing that plans and issuers **exclude provider-rate combinations** for services a provider would be **unlikely to furnish or be reimbursed for** (e.g., podiatrists for heart surgery), based on scope of practice or specialty. The proposed mechanism is not a new national taxonomy standard but rather **the plan’s internal provider taxonomy mapping used in claims adjudication** .


To support the exclusion of unlikely combinations, CMS proposes requiring payers to publish the internal provider taxonomy mapping used to prepare the In-network Rate File. That is a meaningful step toward transparency about the *rules used to construct* the disclosure in addition to the rates themselves.


In our view, the key benefit here isn’t actually file size. It’s[usability and clarity](https://www.serifhealth.com/blog/zombie-hunting-filtering-approaches-for-price-transparency-data) of the data, as it should massively clean up ambiguity and remove the need for a user of the data to obtain claims datasets and do extensive data engineering work to[filter out all the junk](https://www.serifhealth.com/blog/impact-of-zombie-rate-filtering-for-tic-data) .


However, the argument that this is going to improve file sizes is over-simplified—CMS will need to give careful guidance to implementers and follow through on validation rules and tools for this to be a file size win. The reason is that a provider reference object (a TIN and NPI list pair) that is printed once in the file and connected to rates for “everything” actually requires less disk space than a file where every provider reference gets permuted potentially thousands of times so the NPI list is an exactly accurate reference for which providers are eligible to perform each billable code.


No matter how this gets implemented, we believe the benefits to usability should exceed any costs.


## **2) A new “Utilization File” to identify who actually billed and got paid**


CMS also proposes a **Utilization File** paired to each In-network Rate File. The concept: **list providers who submitted and received reimbursement for at least one claim** for a covered item/service over a defined window (the proposal describes a 12-month period ending 6 months prior to posting).


This is especially interesting because it allows consumers of the files to separate:


- *Contracted and credentialed* providers with theoretical rates, from
- Providers who are *demonstrably active* in claims.


It also provides a new source for utilization weighting, and additional analysis of volume-pricing dynamics. Finally, it eliminates the need to source expensive outside claims to cross-verify data.


## **3) Network-level reporting to reduce duplication (aligns “how payers organize” with how users analyze)**


CMS proposes requiring issuers to publish **one In-network Rate File per provider network** rather than per plan/policy—explicitly to reduce duplication and file counts, and to align more closely with how hospital price transparency data is typically structured.


In the Federal Register text, CMS points to widespread use of file organization constructs like tables of contents—and cites external work observing these patterns.


This would be a massive win—instead of enormous TOC’s full of entirely duplicated and redundant many-gigabyte files only changing by their filename or header, the[core and common providers](https://www.serifhealth.com/blog/provider-directory-use-cases) and rates in the network would be published in one file shared by most plans, and the rare set of plans that actually have custom rates or contracts can be published in small per-plan files. Any plan is then a simple composition of these networks—the structure becomes clear.


The critical implementation detail to make this work well is a precedence indicator - if any plan MRF becomes a composition of different networks and structures, precedence will be important to know how to converge those together when (not if) conflicts happen.


## **4) A Change-log file to track what’s new**


CMS proposes a new **Change-log machine-readable file** that would reflect what changed between one in-network posting and the next, so users can identify which files contain new information rather than re-downloading everything each cycle.


We believe this item needs a lot of clarification (and probably stands a reduced chance of making it to the final rule). Here’s why: It sounds like a good idea in principle, but in practice is incredibly hard to implement well and in a way that adds value relative to the burden it creates.


The proposed rule leaves key implementation details open, and those details matter. If the change-log is too high-level (e.g., “this network file changed”), it may quickly become noise because most large networks will have *some* contract or provider roster change each quarter; if it’s too granular, it runs into a real technical constraint of the current JSON structure: many elements don’t have stable, persistent identifiers, which makes “true deltas” (especially deletions) hard to represent without effectively reprinting large portions of the file. CMS is explicitly asking for comment on what level of change information would be most useful, and we’ll be watching closely to see how they balance usability with feasibility.


We’ll see how CMS threads the technical needle on this part of the rule.


## **5) Out-of-network (OON) Allowed Amount upgrades: more data, better comparability**


CMS is blunt that many Allowed Amount Files have **limited or no data** , and proposes three changes to increase usable OON volume:


1. **Aggregate Allowed Amount Files by insurance market type** (large group, small group, individual, self-insured)
2. **Lower the claims volume threshold from 20 to 11** over the reporting window
3. **Expand the reporting window** (proposal describes moving from 90 days to 6 months, and the lookback from 180 days to 9 months)


The “11 claim” threshold is also tied in the proposal to standard small-cell suppression logic used in other[CMS data](https://resdac.org/articles/cms-cell-size-suppression-policy) contexts.


This is a welcome change with no caveats. If payers have already implemented the systems to extract this data, it should be trivial to tweak the thresholds to produce more usable information in these files, and the market type breakout seems like readily available information to payers.


## **6) Quarterly posting (not monthly) as a burden reduction measure**


CMS proposes moving the in-network and allowed amount files to a **quarterly** update cadence rather than monthly, with some stakeholders explicitly arguing that networks and rates often don’t change meaningfully month-to-month and that users struggle to keep up with ingestion at the current pace.


While Serif Health’s[payer inventory](https://www.serifhealth.com/inventory/payers) of over 230 payers processed monthly makes clear we’re capable of those sprints, we’re supportive of the change given the additional complexity being added by the other parts of the rule.


After all, every health plan administrative burden is a cost we all pay for in our insurance premiums. If we consider the balance of complexity and clarity introduced with the other proposed changes, a tradeoff in frequency of posting seems fair…but only if enforcement holds payers accountable to posting on time and on schedule.


## **7) Aligning TiC cost estimates with the No Surprises Act**


Beyond the machine-readable files, the proposal would align TiC’s price comparison requirements with the No Surprises Act (NSA) by requiring plans/issuers to make the *same* cost-sharing information available through the online tool, on paper, and by phone upon request—and by updating related disclosures to reflect NSA balance-billing protections.


That alignment has a massive implication for MRF data quality: if a plan must operationalize one consistent cost-sharing estimate standard across channels and under both TiC and NSA, it reduces room for inconsistencies and forces tighter linkage between what’s disclosed and what is actually used for member-facing estimates.


We believe this will put a significant amount of positive pressure on the validity and usability of data in MRFs as it effectively makes the rates printed in MRFs binding for estimates.


## **8) Still no prescription drug transparency mandate**


The proposed rule clearly calls out the lack of a prescription drug transparency mandate and says that CMS is ‘separately taking into consideration’[RFI feedback](https://www.serifhealth.com/blog/prescription-drug-price-transparency-our-response-to-the-federal-rfi) on the prescription drug file for its implementation.


This is[disappointing](https://www.statnews.com/2025/12/22/trump-delays-drug-price-transparency-drug-companies-pbm/) , but given the clear call out that this will be addressed separately, we are hopeful CMS will move quickly in 2026 to address this gap in the price transparency requirements.


## **Humble Brag: Serif Health’s blog is explicitly cited in the proposed rule**


Two Serif Health blog posts from 2023 and 2024 are **directly referenced** in the[Federal Register](https://www.federalregister.gov/documents/2025/12/23/2025-23693/transparency-in-coverage) proposal:


- **“**[Learnings from MRF Land](https://www.serifhealth.com/blog/learnings-from-mrf-land) **”**
- **“**[Zombie Hunting: Filtering Approaches for Price Transparency Data](https://www.serifhealth.com/blog/zombie-hunting-filtering-approaches-for-price-transparency-data) **”**


We see this as real validation of what our team has learned the hard way: **the limiting factor isn’t whether rates exist; it’s whether the disclosures can be interpreted, compared, and trusted at scale.**


###
**Bottom line**


CMS-9882-P is not just a “more data” proposal; it’s an explicit acknowledgement that **data** **volume without usability is not transparent** . And it’s proposing mechanisms, including file restructuring, change tracking, utilization context, and clinically plausible filtering, that mirror what the most serious TiC users have already been forced to build themselves.


Overall, we welcome CMS’ proposed changes to the TiC MRF reporting; we advocated for some of these changes through the RFI process and valued our dialogue with the price transparency community, including CMS,[Patients Right Advocate](https://www.patientrightsadvocate.org/) ,[SIIA](https://www.siia.org/i4a/pages/index.cfm?pageid=1) , and other startups building in the space in doing so.


Still, this is a[proposed rule](https://www.cms.gov/newsroom/fact-sheets/transparency-coverage-proposed-rule-cms-9882-p) . MRF **amendments won’t apply until 12 months after publication of a final rule** , and dates are currently flagged as effective **January 1, 2027** . Those may change. The practical takeaway: **this is directionally important now** , even if many operational changes won’t be immediate. A few watch items that matter for anyone building on TiC as this rule is finalized:


- **Whether CMS finalizes network-level reporting and the new file trio** (taxonomy / utilization / change-log files) *as written* , or modifies scope based on comments.
- **Whether CMS further narrows file formats to a single standard** , with the proposed rule discussing the intent to restrict formats via technical implementation guidance.
- **Drug transparency rulemaking** and whether it meaningfully incorporates the lessons from TiC reporting to date as well as comments by Serif Health and others as part of the[Request for Information (RFI)](https://www.serifhealth.com/blog/prescription-drug-price-transparency-our-response-to-the-federal-rfi) we participated in last summer.


At Serif Health, we look forward to submitting comments on the proposed rule and engaging with CMS and the broader policy community next year. We’ll be tracking these developments and adapting our systems to match whatever the final rule turns out to be. Our focus is to **turn every transparency disclosure into reliable, comparable reimbursement intelligence** so teams can make decisions with confidence, not caveats. We’ll continue to publish insights on our blog and highlight how payer rates data shows up in[live MRFs](https://www.serifhealth.com/inventory/) via our payer inventory.


If you’re interested in applying price transparency data, we’d love to connect. Please reach out tohello@serifhealth.com or schedule a[demo](http://calendly.com/serif-health/30min) . Also feel free to check out our[sample data](https://app.serifhealth.com/sample/data) on our web platform[Signal](https://www.serifhealth.com/products/signal) .


‍


***Matt Robben contributed research, editing, and review for this article.***


‍
