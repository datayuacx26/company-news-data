---
schema_version: "1.0.0"
document_id: "0dad8c12497a99617e7b6462846a7a3e72d99d856cf1cca83e4b0c0978069a33"
company_key: "yc-sphinx"
company: "Sphinx"
source_id: "yc-sphinx-news-import-f18a1b608f6d"
canonical_url: "https://sphinxhq.com/blog-posts/ofac-50-percent-rule-compliance-guide"
published_at: "2026-08-05T10:00:01.795+00:00"
first_seen_at: "2026-08-05T10:42:46.075844+00:00"
fetched_at: "2026-08-05T10:42:47.386402+00:00"
content_hash: "sha256:01100dd092282680c0844b65aa76d31921f5e0d7125d001054a79ee538f32332"
---

# The OFAC 50 Percent Rule: A Practical Guide for Compliance Teams

**TL;DR:** The OFAC 50 Percent Rule treats any entity owned 50 percent or more, directly or indirectly, in the aggregate by one or more blocked persons as itself blocked. Compliance teams must trace ownership through multi-tier corporate structures and aggregate stakes across unrelated sanctioned parties. OFAC collected over $265 million in enforcement penalties in 2025 alone, with several cases hinging on failures to look beyond formal ownership records.


## What the OFAC 50 Percent Rule Actually Requires


The OFAC 50 Percent Rule establishes that any entity owned 50 percent or more by one or more blocked persons is itself treated as a blocked person, even if that entity does not appear on the SDN List. The rule was formalized in OFAC's August 13, 2014 revised guidance and applies across all sanctions programs administered by the U.S. Department of the Treasury's Office of Foreign Assets Control.


Four elements define how the rule operates. First, ownership interests of multiple blocked persons are aggregated. If SDN A owns 30 percent and SDN B owns 25 percent of the same entity, that entity is blocked at 55 percent aggregate ownership. Second, ownership flows through corporate tiers. If a blocked person owns 50 percent of Entity A, and Entity A owns 50 percent of Entity B, Entity B is blocked because the sanctioned person indirectly owns 50 percent. Third, blocking is automatic. No separate OFAC designation is required for the downstream entity. Fourth,[sanctions screening](https://sphinxhq.com/blog-posts/best-sanctions-screening-software-2026) must account for these unlisted entities, because enforcement follows a strict liability standard where intent or knowledge is not required for a civil violation.


The rule applies broadly. It covers financial institutions, private equity sponsors, asset managers, exporters, multinational corporations, and any U.S. person entering transactions that touch entities in which sanctioned parties hold ownership stakes. Property blocked under the rule remains frozen and must be reported to OFAC, regardless of subsequent changes in ownership percentages.


## How Indirect Ownership and Aggregation Work in Practice


How indirect ownership flows through multi-tier corporate structures under the OFAC 50 Percent Rule.


Indirect ownership is the area where most compliance teams encounter difficulty. OFAC defines indirect ownership as a blocked person's stake in an entity held through one or more intermediary entities that are themselves 50 percent or more owned by blocked persons. The calculation requires tracing ownership through each corporate tier and determining whether the chain of majority ownership remains unbroken.


Consider OFAC's own published examples. A blocked person owns 50 percent of Entity A and 50 percent of Entity B. Entities A and B each own 25 percent of Entity C. Entity C is blocked because the sanctioned person indirectly owns 25 percent through Entity A and 25 percent through Entity B, totaling 50 percent. But change the numbers slightly, where the blocked person owns only 25 percent of Entity B, and Entity C is no longer blocked. Entity B is not majority-owned by the sanctioned person, so the ownership does not flow through.


Aggregation adds another layer of complexity. Ownership interests of blocked persons designated under different OFAC sanctions programs are combined. A Russian SDN's 30 percent stake and an Iranian SDN's 20 percent stake in the same entity would aggregate to 50 percent, triggering the rule. Compliance teams must screen against the full SDN List rather than treating each sanctions program in isolation.


For organizations dealing with complex corporate structures,[UBO identification](https://sphinxhq.com/blog-posts/what-is-ubo-identification) becomes essential to map these ownership chains accurately. Without clear visibility into beneficial ownership, the risk of transacting with an unlisted blocked entity increases substantially.


## Control vs. Ownership: Where the Bright Line Gets Blurry


The 50 Percent Rule covers ownership only. Control-based blocking requires a separate OFAC designation.


The 50 Percent Rule speaks only to ownership, not to control. OFAC has stated this explicitly: an entity controlled by a blocked person but not owned 50 percent or more is not automatically blocked under the rule. OFAC retains the authority to separately designate such entities and add them to the SDN List, but the 50 Percent Rule itself does not reach them.


That distinction, however, is narrowing. On March 31, 2026, OFAC issued a[sanctions advisory on sham transactions](https://ofac.treasury.gov/media/935441/download) that signals a material shift. The advisory warns that sanctioned persons increasingly use opaque legal structures, trusts, and proxies to conceal continuing property interests while appearing to have divested below the 50 percent threshold. OFAC now applies what it calls "functional definitions" of interest and property interest that look beyond legal formalities to underlying practical and economic realities.


The enforcement record backs this up. In 2025, OFAC took action against GVA Capital Ltd. with a $215.9 million penalty for managing assets tied to a blocked Russian oligarch, according to[OFAC's 2025 enforcement data](https://ofac.treasury.gov/civil-penalties-and-enforcement-information/2025-enforcement-information) . OFAC rejected the firm's reliance on formalistic ownership arrangements and stated that gatekeepers, including investment advisers, accountants, and attorneys, are "often better positioned than others to monitor for and identify ways" in which a sanctioned person retains a property interest. The IPI Partners LLC settlement, at $11.5 million, reinforced the same point: OFAC expects firms to scrutinize whether blocked parties retain influence or decision-making authority through proxies.


According to analysis by WilmerHale, nearly 65 percent of OFAC's 2025 enforcement actions were classified as "egregious," compared to 42 percent in 2024 and between 0 and 35 percent in prior years. This trend suggests that OFAC's tolerance for compliance gaps around ownership analysis is declining, and that reliance on the 50 Percent Rule as a safe harbor requires increasingly rigorous due diligence behind it.


## Documenting Screening Decisions and Building an Audit Trail


The 50 Percent Rule creates documentation obligations that extend well beyond standard SDN list screening. Compliance teams must record not only that screening was performed but the methodology used to calculate ownership percentages, the data sources consulted, and the rationale for concluding that an entity is or is not blocked.


Effective documentation includes several components. Ownership charts that map each tier of a corporate structure, with percentage stakes attributed to identified beneficial owners. Source verification records showing where ownership data was obtained, whether from corporate registries, KYC questionnaires, or third-party data providers. Aggregation calculations that demonstrate how stakes held by multiple blocked persons were combined. And decision memos that explain why a particular entity was cleared for a transaction or flagged for blocking.


This documentation matters during regulatory examinations and in enforcement proceedings. OFAC's sham transaction advisory explicitly recommends that firms review available information to evaluate whether any red flags are present when a blocked person previously held an interest in property. Red flags include commercially unreasonable transaction terms, transfers to family members or close associates, unclear purpose of transfer, and unduly complex corporate structures involving higher-risk jurisdictions.


For teams managing high volumes of screening alerts, maintaining this level of documentation manually is unsustainable.[Enhanced due diligence](https://sphinxhq.com/blog-posts/what-is-enhanced-due-diligence) workflows that systematically capture ownership data and screening rationale reduce both the risk of enforcement action and the operational burden on analysts. Organizations that invest in structured audit trails position themselves to demonstrate good-faith compliance even when edge cases arise.


## What Changed in 2025 and 2026


Two developments have reshaped how the 50 Percent Rule operates in practice. The first is OFAC's March 2026 sham transaction advisory, which supplements the 50 Percent Rule with guidance on evaluating whether ownership divestitures are genuine or merely paper transactions designed to evade sanctions. The advisory does not replace the 50 Percent Rule but layers additional diligence expectations on top of it.


The second is the Department of Commerce's Affiliates Rule, issued by the Bureau of Industry and Security in September 2025. This rule imposes export control restrictions on entities with 50 percent or greater ownership by parties on the BIS Entity List, the MEU List, or OFAC's SDN List. Although implementation is paused until November 10, 2026, companies must prepare to comply with both ownership-tracing regimes simultaneously. Together, these developments mean compliance teams now face parallel ownership analysis requirements from both OFAC and BIS.


Enforcement volume tells the story quantitatively. OFAC's 2025 enforcement actions totaled $265.7 million across 14 cases, according to OFAC's published penalty data. Through the first half of 2026, OFAC had already reached $282.7 million across five cases, driven by a $275 million settlement with Adani Enterprises Limited. The trajectory indicates that enforcement intensity is not declining, and the financial stakes for compliance failures continue to grow.


## Where Sphinx Fits


Sphinx automates the ownership-tracing and screening workflows that the 50 Percent Rule demands. The platform surfaces beneficial ownership chains across multi-tier corporate structures, aggregates stakes held by multiple sanctioned parties, and flags entities that meet or exceed the 50 percent threshold without manual calculation. Every screening decision is documented with a full audit trail that captures the data sources, ownership percentages, and rationale behind each disposition.


For compliance teams dealing with the expanded diligence expectations from OFAC's sham transaction guidance, Sphinx detects patterns consistent with the red flags OFAC has identified: nominee arrangements, transfers to close associates, and complex structures in higher-risk jurisdictions. The platform integrates with existing[AML screening workflows](https://sphinxhq.com/blog-posts/aml-solutions-that-reduce-false-positives-what-actually-works) and resolves alerts that involve ownership ambiguity, cutting the time analysts spend tracing corporate hierarchies and documenting their findings.


## Frequently Asked Questions


### Does OFAC publish a list of entities blocked under the 50 Percent Rule?


No. OFAC does not maintain a public registry of entities blocked solely under the 50 Percent Rule. These entities are blocked automatically by operation of the rule, even though they do not appear on the SDN List. Compliance teams are responsible for independently determining whether an entity meets the 50 percent ownership threshold through their own due diligence and beneficial ownership analysis.


### How does OFAC calculate ownership when multiple blocked persons hold stakes?


OFAC aggregates ownership interests held by all blocked persons, regardless of which sanctions program they are designated under. If SDN A owns 30 percent and SDN B owns 20 percent of the same entity, the aggregate is 50 percent and the entity is blocked. Individual control by any single blocked person is irrelevant to the aggregation calculation.


### What happens if a blocked person divests below 50 percent?


If the divestment occurs entirely outside U.S. jurisdiction and does not involve U.S. persons, the entity may no longer be automatically blocked. However, property that was already blocked while the entity was majority-owned remains blocked until OFAC authorizes its release or removes the relevant SDN from the list. OFAC does not recognize unlicensed transfers of blocked property, and the 2026 sham transaction guidance adds scrutiny to whether any divestment was genuine.


### Does the 50 Percent Rule apply to control without ownership?


No. The 50 Percent Rule addresses ownership only. An entity controlled by a blocked person but not owned 50 percent or more is not automatically blocked under the rule. OFAC may separately designate such entities, and the March 2026 sham transaction advisory signals that OFAC increasingly looks beyond formal ownership to assess whether a sanctioned person retains a practical or economic interest in property.


### What due diligence does OFAC expect for 50 Percent Rule compliance?


OFAC expects firms to conduct appropriate due diligence on entities that are party to or involved with transactions, including tracing ownership through multi-tier corporate structures and verifying that purported divestitures are not sham transactions. The 2026 advisory identifies specific red flags to evaluate, including commercially unreasonable terms, transfers to family members, and the use of complex structures in jurisdictions with weak transparency controls.
