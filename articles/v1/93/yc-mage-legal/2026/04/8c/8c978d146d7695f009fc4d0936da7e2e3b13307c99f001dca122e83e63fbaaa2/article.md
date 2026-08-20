---
schema_version: "1.0.0"
document_id: "8c978d146d7695f009fc4d0936da7e2e3b13307c99f001dca122e83e63fbaaa2"
company_key: "yc-mage-legal"
company: "Mage Legal"
source_id: "yc-mage-legal-news-import-389f7de0f02c"
canonical_url: "https://magelegal.com/blog/tech-ma-diligence-software-targets"
published_at: "2026-04-26T00:00:00+00:00"
first_seen_at: "2026-07-22T03:05:30.029696+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:55b836a3d312cd34da112c76e3ece2c57f285a1e7a3a3ff2ee45c304255ce8f5"
---

# Tech M&A Diligence: What Software Targets Actually Need

Tech M&A is intangibles-first. The diligence questions that matter for a manufacturing target (plant condition, environmental compliance, working capital normalization) matter less. The questions that matter for a software target (IP integrity, customer concentration, open-source compliance, team retention) take center stage.


This is the practitioner's view of what actually goes wrong on software-target deals and where the diligence has to be sharpest.


## The IP assignment chain


This is the single most common deal-breaker on tech M&A. The pattern repeats across deals:


- Founder builds the prototype in a personal capacity, often pre-incorporation.
- Company gets formed; the founder begins working full time.
- The original prototype IP is never re-assigned formally.
- Years later, the target is being sold. Material IP may not legally belong to the company.


This is fixable, but it has to be addressed. The SPA can carve out the issue with a specific assignment plus a representation that the founder doesn't intend to assert rights. The deal pricing may reflect the carve-out. What kills deals is discovering this two weeks before signing.


The diligence work: review every founder's IP assignment agreement, check the dates, check the scope (does it cover pre-incorporation work?). Same for senior engineers and other key contributors. Pre-2016 employment agreements often lack DTSA whistleblower notices; post-2016 agreements that lack them have weak trade-secret protection. We covered the broader IP assignment patterns in[IP Assignment Clauses PE Buyers Miss](https://magelegal.com/blog/ip-assignment-clauses-pe-buyers-miss) .


## Open-source license compliance


The second most common dealbreaker. Targets routinely use open-source components in proprietary products without recognizing the obligations.


License risk gradient:


- **Permissive (MIT, BSD, Apache 2.0)** : mild obligations, attribution-only typically. Acceptable in proprietary products.
- **Weak copyleft (LGPL, MPL)** : moderate obligations, applies to modifications of the licensed code only. Acceptable with care.
- **Strong copyleft (GPL, AGPL)** : strong obligations, can require source-code disclosure of the proprietary product that uses the licensed code. Often incompatible with closed-source SaaS distribution.


A target with GPL contamination in core proprietary code has a re-architect-or-renegotiate problem. The buyer either prices in remediation cost (months of engineering work) or walks. We have seen real deals fail here. Run a software composition analysis (SCA) on the codebase early in diligence; do not wait until weeks before signing.


## Customer concentration


SaaS targets often have a top-5 customer set that's 40-60% of revenue. The headline ARR becomes contingent on:


- **Anti-assignment language.** Does the contract permit assignment to the buyer? Many enterprise customer contracts have anti-assignment clauses that survive change-of-control and require new consent at acquisition. We covered the standalone clause analysis in[Anti-Assignment Clauses in M&A](https://magelegal.com/blog/anti-assignment-clauses-in-manda-what-every-deal-attorney-should-know) .
- **Change-of-control triggers.** Some customer contracts have outright change-of-control termination rights. The buyer is buying contingent revenue.
- **MFN and price protection.** Targets often gave price protection ("won't increase more than CPI") or perpetual auto-renewal to win deals. These compound at scale and limit pricing power post-close.
- **Auto-renewal and notice windows.** Some contracts auto-renew; others auto-terminate without affirmative renewal. The contract review needs to surface both.


The diligence work: read the top-50 customer contracts (not just the top-5), check change-of-control, anti-assignment, MFN, auto-renewal, and pricing protection. Tabulate. The aggregate risk picture is what the buyer needs.


## AI-using targets: a new layer


A new dimension on tech M&A in 2026: targets that use AI in their products inherit new contract patterns.


- **Training data rights.** Customer contracts increasingly restrict whether the target can train AI models on customer data. The target's competitive moat may depend on training data; the contract may say no.
- **AI subprocessor disclosure.** GDPR and CCPA require subprocessor disclosures. AI vendors are subprocessors; many targets haven't disclosed them.
- **AI-specific reps and warranties.** NVCA's April 2024 model added AI-specific reps (AI Technology, AI Inputs, Training Data, AI Outputs, AI Compliance, Ethical Use, IP indemnity for AI-generated content). Whether the target's customer contracts and the SPA align with these is now a diligence question.
- **AI talent retention.** The ML engineers, data scientists, and applied researchers who built the AI capability are often more concentrated than other engineering disciplines. Loss of 1-3 key contributors can eliminate deal value. Retention plans matter more than on traditional tech targets.


This category is evolving fast. Treat it as a sharpening edge rather than a settled checklist.


## Other tech-specific items


A short list of items that recur on tech deals:


- **Privacy compliance.** GDPR, CCPA, state-by-state US privacy regimes. Subprocessor lists, DPAs in place, consent frameworks. A target with weak privacy posture inherits a remediation cost.
- **Data security audits.** SOC 2 Type II, penetration testing history, incident response. SaaS targets without this are operating below market norm.
- **Source-code escrow.** Customer contracts may require it. Whether escrows are funded and current matters.
- **Domain and trademark portfolio.** Often holes in international coverage; product-name trademarks may not be registered in jurisdictions the target sells in.
- **Stock issuances and 409A valuations.** Cap table reconciliation against underlying SPAs and option grants. Stale 409As are a common finding.
- **R&D credit history.** Federal and state R&D tax credits often have post-acquisition continuation requirements; check.


## Integration realities


Tech M&A integration is engineering-team-first. The synergies often live in tech-stack consolidation; the cost lives in platform-migration drag for 12-24 months post-close.


Plan for it. The integration timeline is a diligence input, not a post-close discovery.


## How AI-augmented diligence specifically helps


Tech data rooms are document-heavy. 2,000-5,000 documents is normal; some PE-platform tech deals run higher. Reading 1,500 customer contracts manually for change-of-control patterns is exactly the kind of work AI is built to compress.


Run the configured risk pass overnight, get a sortable findings view by Day 2, focus the team on the high-severity issues. The compression is real, and on tech deals it's larger because the document volume is larger.


For the workflow walkthrough see[AI Due Diligence: An Operational Playbook](https://magelegal.com/blog/topics/due-diligence) . For the broader industry comparison see[M&A Trends by Industry](https://magelegal.com/blog/ma-trends-by-industry) .


If you have a current tech-M&A deal:[request a demo](https://magelegal.com/?demo=1) .
