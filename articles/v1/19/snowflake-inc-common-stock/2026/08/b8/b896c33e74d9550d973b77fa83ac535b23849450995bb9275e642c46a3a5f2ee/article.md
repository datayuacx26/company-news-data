---
schema_version: "1.0.0"
document_id: "b896c33e74d9550d973b77fa83ac535b23849450995bb9275e642c46a3a5f2ee"
company_key: "snowflake-inc-common-stock"
company: "Snowflake Inc."
source_id: "snowflake-inc-common-stock-rss-c0c3a844dab6"
canonical_url: "https://www.snowflake.com/content/snowflake-site/global/en/blog/agentic-intelligence-contract-review-snowflake"
published_at: "2026-08-05T18:02:34+00:00"
first_seen_at: "2026-08-05T20:31:56.080800+00:00"
fetched_at: "2026-08-05T20:31:58.162292+00:00"
content_hash: "sha256:595639beb935369e0300f3c40fac3fa0a2be5c2cd8e6b3ab68f59e77fe2e844c"
---

# Agentic Intelligence for Contract Review on Snowflake

*This is an internal case study sharing how Snowflake's Forward Deployed Engineer team built an AI-powered contract review agent on top of the Snowflake platform. The tool is used internally at Snowflake and is shared here as an example of how frontier AI capabilities can transform high-stakes, regulated workflows.*


*Snowflake Technology used:[Snowflake Openflow](https://docs.snowflake.com/en/user-guide/data-integration/openflow/about) ,[Snowflake Cortex AI](https://docs.snowflake.com/en/user-guide/snowflake-cortex/ai-documents) ,[Cortex Agent](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents) ,[Data Agent Run](https://docs.snowflake.com/en/sql-reference/functions/data_agent_run-snowflake-cortex) ,[Snowflake AI Extract](https://docs.snowflake.com/en/sql-reference/functions/ai_extract) ,[Streamlit in Snowflake](https://docs.snowflake.com/en/developer-guide/streamlit/about-streamlit) ,[Snowflake Intelligence](https://docs.snowflake.com/en/user-guide/snowflake-cortex/snowflake-intelligence)*


## Traditional review process


In the high-stakes world of enterprise software, the quote-to-cash lifecycle hinges entirely on the speed and accuracy of contract review. Traditionally, scaling accounting operations means adding manual headcount to painstakingly review bespoke customer order forms, MSAs and amendments. This manual approach creates an operational bottleneck and can introduce human-error risks when identifying nonstandard clauses that impact revenue recognition.


At enterprise scale, Snowflake processes a high volume of customer contracts — ranging from structured capacity commitments to specialized marketplace agreements. Each of these contractual events carries unique commercial terms — such as capacity commitments, discount structures, billing frequencies and incentive clauses — that must be meticulously reviewed for revenue recognition compliance and internal audit controls.


When it comes to testing internal controls over this process, auditors read each PDF line by line, check terms for standard/nonstandard implications and review the relevant revenue treatment for appropriateness. But as Snowflake's deal volume grows, this model doesn't scale. Thousands of order forms per quarter, each requiring careful human review …


"The audit team was spending hours manually scanning PDFs to find nonstandard terms and yet only providing assurance on a sample of the entire population," says Amrita Kapoor, VP Internal Audit. "We needed to flip the model. By letting AI handle the exhaustive reading across the entire population, our auditors can focus their expertise on exception handling — ultimately providing significantly expanded assurance and eventually also passing these time savings back to process owners.”


## Building an auditor's copilot, not a black box


The team's design principle was straightforward: automate the detection layer, but keep auditors in complete control of what counts as "nonstandard." This wasn't about replacing judgment; it was about removing the tedious hours spent finding the needle in the haystack so experts could focus on deciding what to do about it.


#### The Contract Review Agent works in three layers:


**Ingest and extract:** Order form PDFs flow from source systems through Google Drive via Snowflake Openflow into a Snowflake stage. A Cortex Agent powered by Cortex AI Functions handles layout extraction and uses AI Extract to pull structured fields: customer name, capacity amount, discount terms, payment schedules and dozens of revenue-relevant data points.


**Classify against a playbook and flag novel terms:** Rather than relying solely on the model's judgment, the agent evaluates every extracted term against a user-managed playbook — a living set of rules that define what "standard" and "nonstandard" look like. The playbook is a governed Snowflake table that the audit team owns and edits directly, no engineering ticket required. The agent scores every clause against it, producing a classification of standard or nonstandard — with confidence scores, clause-level excerpts, page references and a natural-language explanation of why a term was flagged.


**Surface, review and learn:** Findings are presented in a reviewer-centric application with an admin dashboard for portfolio-level visibility and a contract detail view for deep-dive review. Auditors can approve, override or escalate every finding. Every correction — whether to an extracted value or a classification — is logged and persisted as extraction tips, similar to playbook rules, that are stored in long-term memory and fed into the agent on every subsequent run. Custom AI Extract questions for each schema field allow auditors to refine how the agent parses specific terms, tightening precision and recall with each batch. A parallel Snowflake Intelligence agent gives stakeholders a natural-language interface to audit status and nonstandard rates without needing direct app access.


"The agent is not just parsing but reasoning. It explains why a term is considered nonstandard, cites the playbook rule and presents the contract excerpt, letting the reviewer confirm or correct. Every decision is logged."


##### Charles Xu


Engineering Manager of Applied AI


*AI-generated illustrative example. Not based on actual customer or company data.*


## The playbook: auditors write the rules


Most AI contract tools ship with a fixed model of what's "normal." That's a problem in audit, where the definition of nonstandard evolves with business context, regulatory changes and deal structure.


The playbook manager puts control directly in the auditors' hands. It's an editable rules engine stored in Snowflake, accessible through the application UI:


- Each rule defines a term, its detection criteria and its classification (standard vs. nonstandard).
- Every change is logged to an immutable audit trail — action, rule ID, who changed it, when, old value, new value.
- Rules take effect immediately on the next processing run, without code changes or redeployment.


This means the audit team can respond to a new contract pattern in minutes. When a novel discount structure shows up in Q4 deals, auditors add a rule and the agent starts flagging it across the entire corpus. The playbook evolves at the speed of business, not at the speed of engineering sprints.


## Catching what you didn't know to look for


Rule-based detection catches known patterns. But contracts are creative documents — new clauses appear that no existing rule anticipates. This is where novel term detection comes in.


The system uses a two-layer approach. First, a corpus of known-standard contract language is embedded and indexed. When the agent encounters a clause that is semantically distant from this corpus, it's flagged as potentially novel. Second, flagged terms are evaluated against existing playbook rules to separate genuinely novel language from known patterns.


Surfaced novel terms land in a separate view where auditors label them: meaningful anomaly or routine noise. These labels flow back into the system, teaching it what to prioritize in future runs. Over time, the novel term detector gets sharper, tuned by the people who understand the contracts best.


## Results


- **Review time cut by 70%:** What previously took multiple levels of prep is now handled by AI and instead of days is hours.
- **Full coverage without linear headcount growth:** Thousands of order forms reviewed for revenue implications per quarter without scaling the audit team in lockstep with deal volume.
- **Auditable by design:** Every extraction, classification, rule change and correction is logged with full provenance — the kind of evidence trail external auditors expect.
- **Improving accuracy over time:** Each review cycle feeds corrections and labels back into the system, compounding accuracy with every batch.


## Beyond customer contracts


The same architecture is being extended to other agreement types. The pattern generalizes: define an extraction schema, write playbook rules for what "standard" means in that document type and let the agent do the exhaustive reading while experts focus on judgment.


"The only way contract review scales with the business is through this kind of system," says Nikolai Scholz, PM Unstructured Data Application. "Without it, you grow your operational team linearly. With it, you keep the team lean while expanding coverage — and the system gets smarter every quarter because the experts are teaching it."
