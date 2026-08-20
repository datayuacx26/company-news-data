---
schema_version: "1.0.0"
document_id: "67654b22e731056c2a98dd049875b20409bbfcb98ca5b79977737486d1ac1e3c"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/governance-controls-unified-feedback"
published_at: null
first_seen_at: "2026-08-07T21:50:28.388550+00:00"
fetched_at: "2026-08-07T21:50:30.457103+00:00"
content_hash: "sha256:8d9929cb06060673394ba1128cb3768756df587fca5188e55d0023ddce31a1f5"
---

# What governance controls do you need when you unify support tickets, reviews, and survey data?

Most enterprise customer experience (CX) teams[solve the connector problem](https://getthematic.com/insights/enterprise-multi-channel-feedback-integration) , then find out it was the easy half. Support tickets, app store reviews, and survey open-ends[land in one platform](https://getthematic.com/insights/single-source-of-truth-customer-feedback) . Themes get generated. A number goes on a board slide. Then someone asks where the number came from, and the program stalls.[Forrester's 2025 survey](https://www.forrester.com/blogs/feedback-and-cx-measurement-programs-must-boost-their-impact-according-to-forresters-2025-survey/) of 311 feedback management and CX measurement practitioners found that only half of teams can link CX metrics to business outcomes. Only half feel confident they can analyze what they collect.


Six governance controls close that gap: provenance from every record back to its source, a published method for how text becomes a metric, evidence you can open on demand, change control over the theme taxonomy, role-based access to who can see and edit what, and an explicit boundary around where the data goes. Thematic builds these controls into the product rather than into a policy document. That's what lets a CX leader defend a number months after first presenting it.


Below is what each control means, where unification projects lose the audit trail, and what to ask a vendor before signing anything.


## What "governed unification" actually means


Governed unification is combining feedback sources so that every resulting number traces back to the specific customer text that produced it, under a method and a taxonomy version you can name.


It's not the same thing as consolidation. Consolidation gets the records into one place. Governance is what happens after that, and it breaks into six dimensions:


- **Provenance.** Every record carries its source, timestamp, and metadata through the analysis, so you can always say which channel a finding came from.
- **Method transparency.** The arithmetic that turns tagged text into a score contribution is documented and reproducible by hand.
- **Evidence access.** Any aggregate number opens into the individual comments behind it.
- **Change control.** Edits to the theme taxonomy are staged, committed deliberately, and attributed to a person and a time.
- **Access control.** Viewing feedback, exporting it, and changing a theme are three separate permissions.
- **Data boundary.** Where the data is processed, who subprocesses it, and whether it trains anyone's model are all answerable in writing.


This matters because executive trust is the constraint, not analytical capability. The fastest way to lose an executive is to have no answer to "how do you know that?"


## What enterprise feedback governance typically requires


**Traceability as a requirement, not a feature.** The[EU AI Act](https://artificialintelligenceact.eu/article/12/) sets the expectation plainly for systems in its scope. High-risk AI systems "shall technically allow for the automatic recording of events (logs) over the lifetime of the system," to ensure "appropriate traceability." A feedback analytics platform is generally not a high-risk system under Annex III. But procurement teams have started applying the same bar to anything that produces a number an executive repeats.


**Explainability, which isn't the same as transparency.** The[NIST AI Risk Management Framework](https://nvlpubs.nist.gov/nistpubs/ai/nist.ai.100-1.pdf) draws the cleanest available line: "Transparency can answer the question of 'what happened' in the system. Explainability can answer the question of 'how' a decision was made in the system." Most vendors answer the first. Buyers pressure-testing a feedback platform are asking the second.


**Being able to show your work.** The General Data Protection Regulation (GDPR) requires under[Article 5(2)](https://gdpr-info.eu/art-5-gdpr/) that a controller "be able to demonstrate compliance with" the data protection principles. Being right isn't enough. The[UK Information Commissioner's Office](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/artificial-intelligence/explaining-decisions-made-with-artificial-intelligence/) and The Alan Turing Institute define six explanation types that give the showing some structure. Two land squarely on feedback analytics: the rationale explanation, "the reasons that led to a decision, delivered in an accessible and non-technical way," and the data explanation, "what data has been used in a particular decision and how."


**Privacy applied before processing, not after.** GDPR Article 5(1)(c) limits personal data to what is "adequate, relevant and limited to what is necessary." In feedback analytics that means redaction runs ahead of the model, not as a display filter afterward.


Meeting this bar pays off. In KPMG and the University of Melbourne's[2025 global study](https://assets.kpmg.com/content/dam/kpmgsites/xx/pdf/2025/05/trust-attitudes-and-use-of-ai-global-report.pdf) of 48,340 people across 47 countries, four in five said they'd be more willing to trust an AI system when assurance mechanisms are in place.


## Where most unification projects lose the audit trail


The failure is rarely at ingestion. It's at the handoffs. Common breakpoints:


- The dashboard reports a theme's effect on net promoter score (NPS), but no path leads from that figure to the comments behind it.
- The method is described as "AI-powered" with no published arithmetic, so nobody outside the vendor can reconstruct the calculation.
- Themes get renamed, merged, and split with no record of who changed what or when, so last quarter's number quietly stops matching this quarter's definition.
- Every analyst can see every dataset, which rules the platform out for regulated or sensitive feedback.
- Personal data is masked in the interface but was already processed in the clear.


One test surfaces most of this in a demo. Start from a number on a dashboard, click through to the sentence that produced it, then ask who last changed that theme, and when. Vendors that can do the first half but not the second are selling analysis, not a governed measurement system.


## How Thematic governs unified feedback


**One theme set across every source.** A Lens in Thematic combines multiple datasets and analyzes them together under a single, unified set of themes. Adding a new source automatically applies that Lens's themes. A support ticket and a survey verbatim describing the same problem resolve to the same theme, not to two channel-specific labels.


**A published formula rather than a black box.** Thematic calculates[a theme's Impact](https://getthematic.com/insights/link-feedback-themes-nps-csat-drivers) as the score across all responses minus the score across all responses excluding that theme. That's how much the overall score would change if every comment mentioning the theme were removed.


**A waterfall that adds up.** Thematic's[Score Change analysis](https://help.getthematic.com/article/98-score-change-explanation) breaks down movement between two periods. The previous score plus the sum of theme changes plus unthemed feedback equals the current score. Contributions sum to the total difference, so you can check the math yourself.


**Evidence one click away.** From any theme, Thematic's Comments section shows the tagged responses and highlights the sentence that triggered the tag, color-coded by sentiment. Volume differences carry a two-tailed significance test, marked significant at a 5% or less chance the difference is random.


**Taxonomy changes staged, committed, and attributed.** Edits in the[Thematic Themes Editor](https://help.getthematic.com/article/40-how-to-use-the-themes-editor) auto-save to a draft and require an explicit Apply themes step to commit. Revert Draft rolls back to any of the last three applied versions, showing who applied them and when. Editing sits behind a separate Manage themes permission, so the analyst who defines a theme and the executive who reports it can be different people.


**Access scoped by role and by dataset.** Thematic ships four default roles: Dataset Admin, User Admin, Analyst, and Viewer. Permissions for Analysis, Answers, Download, Manage, Manage themes, and Upload data are granted across all datasets or per dataset, with SAML 2.0 single sign-on and SCIM v2 provisioning.


**Redaction ahead of analysis.** Thematic's redaction of personally identifiable information (PII) can be configured to run on incoming data before the initial analysis job. It masks types including email addresses, URLs, unique identifiers, physical addresses, names, and phone numbers, replacing each with a token such as` \[EMAIL\]` . It also strips quoted email history.


**An explicit data boundary.**[Thematic's position on model training](https://getthematic.com/insights/20-questions-and-answers-to-help-buyers-of-ai-based-solutions-for-market-research) is published and unambiguous: "Our customer's data is only ever used to train models wholly within that customer's workspace and control," and "We never train LLMs in-house or share our data with LLM providers for training purposes." Processing stays in the customer's region across deployments in the United States, EU-Frankfurt, and ANZ-Sydney. Thematic holds SOC 2 Type II certification.


## What governed unification looks like at enterprise scale


[Atlassian](https://getthematic.com/insights/how-atlassian-built-a-scalable-data-pipeline/) , one of the world's biggest software companies, received feedback through support, community posts, and in-app messages. Its feedback analysis included over 1 million community questions and comments. At that volume, the governance question isn't whether themes can be generated. It's whether a theme's definition holds steady across three very different channels.


[Atom Bank](https://getthematic.com/insights/ai-feedback-analytics-financial-services-use-cases) , the UK's first app-only bank, collected feedback across 7 channels spanning 3 product lines in mortgages, savings, and deposits, including App Store reviews, Trustpilot, Reevoo, complaints, Salesforce, and multiple surveys. Michael Sherwood, Head of Customer Experience, framed the payoff as separating signal from noise: "This means we are able to easily differentiate between verbatim themes that are noise (no impact to an overall metric) and those which are seriously impacting our CX metrics." Atom Bank recorded a 69% reduction in calls related to unaccepted mortgage requests and 43% fewer calls about savings maturities. It did all of that while growing its customer base 110% year over year.


[Mitre 10](https://getthematic.com/insights/humanising-hardware-how-mitre10-made-voice-of-customer-actionable-with-thematic/) , a cooperative business with stores across New Zealand, collected 20,000 verbatim comments per month across its 84 stores with a three-person insights team. That team quantified a single theme against the headline metric: "we discovered that stock availability issues were taking half a NPS point off our overall scores." A named theme, a specific metric, a stated magnitude, and comments underneath it. That's the shape of a governed finding.


## A buyer's checklist for governed feedback unification


1. Can you start from any number on a dashboard and reach the individual sentence that produced it?
2. Is the arithmetic linking themes to score movement published, or described only as "AI"?
3. Do theme contributions sum to the total score change, so the decomposition can be checked?
4. Are taxonomy edits staged and committed, with a record of who applied them and when?
5. Is the permission to change a theme separate from the permission to view or export feedback?
6. Does personal data get masked before the model processes the text, or only in the interface?
7. Is the vendor's position on training models with your data written down, and does it name the regions and subprocessors involved?


Any vendor can answer the first question. The programs that survive executive scrutiny are the ones where the vendor can answer all seven.


## The short answer


Unifying support tickets, reviews, and survey data needs six governance controls: provenance, published method, evidence access, taxonomy change control, role-based access, and an explicit data boundary. Thematic's answers are a single theme set across sources, a documented Impact formula, a Score Change decomposition that sums to the total, sentence-level evidence, attributed taxonomy versions, role-scoped permissions, and a written no-training commitment. Run the test in a demo: pick a number, click to the sentence, then ask who last changed the theme.
