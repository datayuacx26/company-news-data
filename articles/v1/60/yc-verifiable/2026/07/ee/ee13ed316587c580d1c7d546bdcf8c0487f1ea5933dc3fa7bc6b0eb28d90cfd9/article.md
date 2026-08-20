---
schema_version: "1.0.0"
document_id: "ee13ed316587c580d1c7d546bdcf8c0487f1ea5933dc3fa7bc6b0eb28d90cfd9"
company_key: "yc-verifiable"
company: "Verifiable"
source_id: "yc-verifiable-news-import-4a6227c38e0f"
canonical_url: "https://verifiable.com/blog/the-credentialing-compliance-environment-changed-most-health-plans-are-still-catching-up"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-22T18:41:41.803031+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:1224aa09295747ea4a4171320146c0a0c28e299ceeea545fbe5a00646946f176"
---

# The Credentialing Compliance Environment Changed. Most Health Plans Are Still Catching Up.

Four overlapping regulatory developments have reshaped the credentialing compliance environment for health plans since 2024, and they are not changes that can be absorbed by working harder or adding headcount. NCQA's 2025 guideline updates, the Mental Health Parity and Addiction Equity Act final rule, CMS directory accuracy enforcement, and Medicare Advantage Star Ratings scrutiny are converging on the same infrastructure gap: the systems and processes most health plans use to manage provider data were not built to do what these changes now require.


The headline requirements are not complicated. Most compliance teams have read the guidance documents. What is harder to answer is the operational version of each question: not what the standard says, but what it demands of your team on a given Tuesday afternoon when a provider's status changes and your system needs to catch it, record it, and propagate it correctly. That is the gap this post is designed to address.


**Related reading:** For a full regulatory reference table mapping each change to its specific infrastructure requirements, urgency level, and diagnostic questions for your team, check out[The Hidden Economics of Credentialing: A Framework for Operational Redesign](https://verifiable.com/resources/the-hidden-economics-of-credentialing-a-framework-for-operational-redesign) , a practitioner's guide co-authored with Joey Costa of[Provider Peak](https://providerpeak.com/) .


## **NCQA 2025: What the shortened windows actually require**


**The four changes at a glance**


- **PSV window shortened —** 180 to 120 days for accredited plans; 120 to 90 days for certified credentialing organizations. Organizations relying on extended timelines or slower CVO turnaround are no longer compliant.
- **SAM and Medicare/Medicaid monitoring required monthly —** Action required within 30 days of any finding. This is a current requirement, not a transition one. Organizations running checks less frequently than monthly are already out of compliance.
- **License expiration re-verification now proactive —** Expiring licenses must be tracked and re-verified before the next re-credentialing cycle. Particularly high operational impact for networks with significant behavioral health coverage.
- **Audit trail maintenance now explicit —** Verification histories must be maintained as ongoing system outputs, not reconstructed from email chains or spreadsheets. An auditor requesting history for a specific provider as of 18 months ago should receive it in minutes.


‍


[NCQA's updated standards for health plan accreditation](https://wpcdn.ncqa.org/www-prod/wp-content/uploads/HPA-2025_Proposed-Standards-Updates.pdf) took effect July 1, 2025, and four changes are likely to have the most direct operational impact on credentialing teams. The most widely discussed is the shortened PSV window: the primary source verification timeline for accredited health plans has moved from 180 days to 120 days, and for certified credentialing organizations from 120 days to 90 days. For organizations that relied on extended timelines or slower CVO turnaround to stay compliant, the math no longer works.


Less discussed but equally consequential is the SAM and Medicare/Medicaid monitoring requirement. Plans must now monitor System for Award Management and Medicare/Medicaid exclusion lists and take action within 30 days of any finding. This is not a transition requirement but a current one, which means organizations running these checks less frequently than monthly are already out of compliance by design. The third change, license expiration re-verification, is new territory for health plans specifically: the updated standard requires that expiring licenses be tracked and re-verified proactively, not just at the next scheduled re-credentialing cycle. For networks with significant behavioral health coverage, where provider attrition and license changes are more frequent than in medical/surgical populations, this adds meaningful operational volume.


The fourth change is the one that most often surfaces during audits: the requirement for explicit, continuous audit trail maintenance. NCQA's prior language on documentation was relatively flexible. The 2025 update is specific. Verification histories must be maintained as ongoing system outputs, not reconstructed after the fact from email chains and spreadsheets, and an auditor asking for the full verification history for a specific provider as of 18 months ago should be able to get it in minutes, not days.


At a[recent webinar co-hosted by NCQA](https://verifiable.com/blog/credentialing-as-a-network-performance-lever-key-takeaways-from-the-ncqa-verifiable-webinar) and Verifiable, Danny Zajac, principal strategic adviser at Blue Cross Blue Shield of North Carolina, offered an account of what preparing for these changes looked like internally that is worth holding onto: nothing was catastrophically broken, he said, but the data architecture underlying their credentialing function could not scale to where the organization needed to go. Fixing it required rethinking not just the technology but the team structure and data model underneath it. That framing is more useful than the compliance checklist version of these changes, because it points at the real question: not whether you're meeting the current standard, but whether your infrastructure is capable of meeting the next one. **Related reading:**[Get Ahead of the 2025 Changes to NCQA Credentialing Guidelines](https://verifiable.com/blog/get-ahead-of-the-2025-changes-to-ncqa-credentialing-guidelines) |[Credentialing as a Network Performance Lever: NCQA and Verifiable Webinar Takeaways](https://verifiable.com/blog/credentialing-as-a-network-performance-lever-key-takeaways-from-the-ncqa-verifiable-webinar) |[NCQA Audit Readiness](https://verifiable.com/blog/ncqa-audit-readiness) **Diagnostic question for your team:** Can you demonstrate monthly SAM and Medicaid monitoring to an auditor today? If a regulator asked for the full verification history for a specific provider as of 18 months ago, how quickly could your team produce it?


## **MHPAEA: Why behavioral health networks are the highest-risk population right now**


[The Mental Health Parity and Addiction Equity Act final rule](https://www.dol.gov/agencies/ebsa/about-ebsa/our-activities/resource-center/fact-sheets/final-rules-under-the-mental-health-parity-and-addiction-equity-act-mhpaea) , published in 2024, moved behavioral health network adequacy from guidance to active enforcement. The new nonquantitative treatment limitation documentation requirements mean plans must demonstrate that their behavioral health network composition is genuinely comparable to medical and surgical networks, not just in policy language but in practice. And the enforcement trajectory makes clear that regulators intend to verify the difference.


[The New York Attorney General's settlement with EmblemHealth](https://ag.ny.gov/press-release/2026/attorney-general-james-secures-sweeping-reforms-improving-access-mental-health) in early 2026 is the clearest signal of where this is heading. The settlement required a $2.5 million penalty and member reimbursement tied directly to behavioral health ghost network rates: providers listed as in-network who were unreachable, no longer practicing, or not accepting new patients. The action came under state-level enforcement rather than federal MHPAEA review specifically, but it illustrates the dollar value of directory inaccuracy in behavioral health networks and the seriousness with which regulators are now treating the gap between what plans list and what members can actually access.


The operational reason behavioral health networks are particularly vulnerable to this exposure is provider attrition. BH providers change their practice settings, insurance participation, and licensing status at higher rates than most medical and surgical specialties, which means point-in-time credentialing checks, even when they are NCQA-compliant at the moment of the re-credentialing cycle, can miss status changes that occur in the two to three years between cycles. For large BH networks, continuous monitoring is not a premium feature but the only compliance posture that is operationally defensible.


What a best-practice BH credentialing program can look like in practice is illustrated by[Grow Therapy's experience](https://verifiable.com/resources/case-studies/grow-therapy) . Before implementing automated credentialing with continuous monitoring, their turnaround times ran from 60 to 120 days and the inconsistency made planning and forecasting difficult. After, the time from credentialing to a patient being matched with a therapist dropped to three to four days, and their NCQA certification became a competitive differentiator in payer contracting rather than a compliance cost. That trajectory, from compliance burden to market advantage, is the argument for treating BH credentialing infrastructure as a strategic investment rather than a line item to minimize.


**Related reading:**[Grow Therapy on Verifiable](https://verifiable.com/resources/case-studies/grow-therapy) |[How Faster Healthcare Credentialing Impacts Care Outcomes](https://verifiable.com/blog/how-faster-healthcare-credentialing-impacts-care-outcomes)


**Diagnostic question for your team:** If a behavioral health provider's license lapsed this week, how quickly would your directory reflect that change? Are you monitoring your BH network with the same cadence as your medical and surgical populations?


The full regulatory reference table, including infrastructure requirements and urgency ratings for all four changes covered in this post, is available in[The Hidden Economics of Credentialing: A Framework for Operational Redesign](https://verifiable.com/resources/the-hidden-economics-of-credentialing-a-framework-for-operational-redesign) . Co-authored with Joey Costa of Provider Peak. Designed to be useful regardless of what technology or operational model you use.


## **CMS directory accuracy: From internal compliance check to visible market signal**


CMS has been moving toward greater scrutiny of provider directory accuracy for several years, and the direction of that movement matters for how health plans should be thinking about directory infrastructure right now. The agency has indicated interest in public disclosure of directory error rates, which would transform accuracy from something plans track internally into something members, employers, and regulators can compare across plans in the market.


The underlying state of provider directories across the industry provides important context for why this scrutiny exists.


A[2023 review of physician directories from five large health insurers](https://pmc.ncbi.nlm.nih.gov/articles/PMC10015301/) found that **81%** of entries contained at least one inconsistency, including address errors, incorrect specialty listings, or providers who were no longer at the listed location.


A[secret shopper study conducted by the New York Attorney General's office](https://ag.ny.gov/sites/default/files/reports/mental-health-report_0.pdf) called roughly 400 mental health providers listed as in-network in a state health plan and found that **86%** were effectively ghost entries.


A U.S. Senate Finance Committee review of Medicare Advantage directories that same year found that **one-third** of listed mental health providers had wrong phone numbers, were unreachable, or did not return calls.


These are not anomalies from a few poorly managed plans; they describe a systemic gap that exists because directory accuracy depends on status changes propagating from the credentialing record in near real time, and most plans' infrastructure was not built to do that.


The No Surprises Act adds a separate liability dimension. Inaccurate out-of-network listings create direct claims exposure: a provider listed as in-network who is actually out-of-network when a member receives care can trigger a billing dispute with financial consequences for the plan. As CMS enforcement intensifies and public disclosure of error rates becomes more likely, directory accuracy shifts from a compliance exercise to a competitive and financial variable that leadership should be tracking with the same rigor as other network metrics.


**Related reading:**[4 Reasons Why Ongoing Provider Network Monitoring Matters](https://verifiable.com/blog/4-reasons-why-ongoing-provider-network-monitoring-matters) |[ROI of Provider Data Transparency in Healthcare Organizations](https://verifiable.com/blog/roi-of-provider-data-transparency-in-healthcare-organizations)


**Diagnostic question for your team:** What is your current lag between a provider leaving the network and that change propagating to your out-of-network listings and public directory? And how do you measure your directory accuracy rate today?


## **MA Star Ratings: The revenue line credentialing teams don't own but directly affect**


For Medicare Advantage plans, there is a line of causality running from credentialing infrastructure to plan revenue that almost never gets drawn explicitly, because the data lives in different departments and no one has mapped the connection. Drawing it is worth doing, because it changes the financial calculus of a credentialing investment in ways that moving it from one column to another in the operations budget cannot.


**Credentialing lag → directory inaccuracy → CAHPS access-to-care scores → Star Ratings → quality bonus revenue**


CAHPS access-to-care scores are a primary input to Star Ratings. Research on Medicare Advantage plan performance indicates that each rating point is worth an estimated $200 to $500 per member per year in quality bonus revenue. Directory inaccuracy is a documented driver of poor access-to-care scores: when a member contacts a provider listed as in-network and finds that provider unavailable, not accepting new patients, or no longer at the listed location, the CAHPS survey captures that failure. And directory accuracy depends on provider status changes propagating from the credentialing record in something close to real time, which means plans running two- to three-year re-credentialing cycles with manual reconciliation between systems are carrying a Star Rating exposure in their current infrastructure, whether or not they have calculated what it costs.


***The credentialing decisions a plan makes today are likely to appear in its Star Rating revenue two or three years from now. Most plans have not drawn that line.***


Verifiable addressed this downstream revenue framing in a Becker's Healthcare conversation: when you solve the fluidity of provider data and centralize it in an accurate, continuously maintained system, you can tie member sentiment around provider directory accuracy directly to the improved efficiency of how that data is collected and maintained. The ROI is measurable, but only if the connection between credentialing infrastructure and member experience is made explicit, and that connection starts with how frequently the credentialing record is updated and how quickly those updates reach the directory.


**Related reading:**[Verifiable and Becker's Podcast: Navigating Network Compliance](https://verifiable.com/blog/verifiable-beckers-podcast-navigating-network-compliance) |[3 Ways Health Plans Can Optimize Networks With Verifiable](https://verifiable.com/blog/3-ways-health-plans-can-optimize-networks-with-verifiable)


**Diagnostic question for your team:** If your directory accuracy rate is off by 5%, what is the estimated CAHPS access-to-care impact at your current membership scale, and how does that translate to Star Rating revenue exposure?


## **Pre-audit self-assessment: Five questions to ask your team**


Before any vendor evaluation or technology decision, the most useful first step is a documented baseline. The five questions below map to each of the regulatory requirements covered in this post and can be completed with your team in about 15 minutes. They are also the diagnostic questions that tend to reveal, faster than any vendor demo, where your current infrastructure is structurally limited rather than just improperly configured.
‍


- Can you demonstrate monthly SAM and Medicaid exclusion monitoring to an auditor today?
- If a behavioral health provider's license lapsed this week, how quickly would your directory reflect that change?
- What is your current lag between a provider leaving the network and that change reaching your out-of-network listings?
- If a regulator asked for the full verification history for a specific provider as of 18 months ago, how long would it take your team to produce it?
- At your current membership scale, what is the estimated Star Rating revenue impact of a 5% directory accuracy gap?


The practitioner's guide includes a full maturity assessment across five operational dimensions, along with the complete regulatory reference table and a 90-day pilot structure designed for organizations that want to validate accuracy before committing to a full transition.


## **FAQs: What compliance teams are asking about 2025 regulatory changes**


**What did the NCQA 2025 credentialing updates actually change?**


Four things, and three of them get less attention than the PSV window shortening. The primary source verification timeline dropped from 180 to 120 days for accredited plans, but SAM and Medicare/Medicaid exclusion monitoring is now required monthly with action within 30 days of any finding, meaning plans running quarterly checks are already non-compliant. License expiration tracking must now be proactive rather than waiting for the next re-credentialing cycle. And audit trail maintenance moved from flexible documentation to a specific requirement: full verification histories must be producible as system outputs on demand, not reconstructed after the fact.


‍ **Why are behavioral health networks at higher regulatory risk?**


Primarily because of provider attrition rates. BH providers change practice settings, insurance participation, and licensing status more frequently than most medical and surgical specialties **—** which means a directory that was accurate at the last re-credentialing cycle can become materially wrong well before the next one. The EmblemHealth settlement in early 2026 (a $2.5M penalty tied directly to ghost network rates in behavioral health) illustrates the concrete cost of that gap. MHPAEA enforcement is now active rather than advisory, and regulators are comparing what plans list with what members can actually access.


‍ **What is the financial risk of provider directory inaccuracy for a health plan?**


It runs across at least three dimensions simultaneously. Inaccurate out-of-network listings create direct claims exposure under the No Surprises Act. Ghost network entries in behavioral health now carry settlement risk under MHPAEA enforcement. And for Medicare Advantage plans, directory errors depress CAHPS access-to-care scores, which feed into Star Ratings. This means that inaccuracy in the directory today has a measurable revenue consequence two or three rating cycles from now. CMS has signaled interest in public disclosure of directory error rates, which would add a competitive dimension to a risk that’s currently invisible to members and employers.
‍


Download[The Hidden Economics of Credentialing: A Framework for Operational Redesign](https://verifiable.com/resources/the-hidden-economics-of-credentialing-a-framework-for-operational-redesign) for the complete regulatory reference, maturity assessment, and decision framework. Co-authored with Joey Costa of Provider Peak and designed to be useful regardless of vendor or operational model.


If you are evaluating whether your current CVO relationship can meet the updated NCQA requirements,[five diagnostic questions to benchmark your CVO](https://verifiable.com/blog/5-questions-to-determine-whether-your-cvo-can-meet-new-ncqa-guidelines-in-2025) is a useful starting point.
