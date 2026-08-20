---
schema_version: "1.0.0"
document_id: "013e3d520053660319f68bb1689deef173fe0bd72aabf047730689dd60392a42"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/reduce-token-usage-precomputed-context"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T22:28:52.768378+00:00"
fetched_at: "2026-08-11T22:28:54.327701+00:00"
content_hash: "sha256:5e43267807e16d7ed6258fc2dce6a1df9f105d7f57cdbf2cc8fd4cef5b9d6ecf"
---

# Faster, cheaper support investigations with precomputed context

When a support engineer is assigned a case, the questions can sound straightforward: What happened, what evidence supports the root cause, and what should happen next? The answers may be spread across a case record, a long conversation feed, linked engineering issues and comments, knowledge articles, and related cases. Precomputed context gathers and organizes evidence from those related records before the agent receives a question. The agent can then begin with the case relationships already identified, rather than reconstructing them during each response. In our evaluation, this approach resulted in lower input token use and latency, without a statistically significant reduction in factuality.


Before adding precomputed context, the Support team at Elastic used an agent for case investigation, root cause analysis (RCA), triage, and related work. To answer a question, the agent had to discover relevant indices, inspect schemas, issue several queries, reconcile conflicting updates, and assemble a response. Questions ranged from narrow state lookups to multisource investigations:


-


What is the current status and priority of support case 1234567?


-


After client ABC’s cluster migrated certificates, it returned Secure Sockets Layer (SSL) handshake and certificate_unknown errors. What caused the failure, and how should it be fixed? Which knowledge base articles are relevant?


-


Client ABC’s cluster stopped processing indexing requests and returned 'rejected execution of primary operation' errors. What was the root cause, and how was service restored?


The next question about the same case or about a related case often required much of the same orientation and synthesis work. That repetition consumed input tokens and added latency. It also increased the chance of invalid queries or wrong source selection.


Earlier work on[precomputed context](https://www.elastic.co/search-labs/blog/pre-computed-context-llm-agent-costs) provided a starting point. It described extracting useful context ahead of time and storing it as structured Knowledge Indicators (KIs). Then it explained how an agent retrieves those compact records before scanning raw documents.


For this support use case, we wanted to understand what context was worth preparing in advance and how the choice of context affected response efficiency and reliability. We also wanted to know what retrieval rules were needed when an agent could use several KI types. This article follows that design process and reports the findings.


## **Starting with index profiles**


We began with index profile KIs. A workflow read each support index mapping and sampled a few documents. It generated a compact profile describing the index’s purpose, the questions it could answer, important fields, exclusions, verified joins, time coverage, example Elasticsearch Query Language (ES|QL) queries, and more. A representative KI document with selected fields looked like this:


```text
{
"_id": "index-profile-support-cases",
"_source": {
"type": "ki",
"title": "Index profile: Support cases",
"origin": { "uri": "ki://support-cases" },
"tags": ["ki-kind:index-profile", "index-selection", "support-data"],
"content": [
"BACKING_INDEX: support-cases",
"PURPOSE: Primary metadata and the customer-reported problem for enterprise support cases (status, product, severity, reported symptom).",
"QUESTIONS_ANSWERED: What is the status of a case? | Which cases affect a given product or version? | What symptom was reported? | Which high-severity cases are still open?",
"WHEN_TO_USE: Start here to find or filter cases by status, product, or severity before pulling the conversation thread or root-cause detail from other indices.",
"KEY_FIELDS: case_number, subject, status, product, severity, created_at, resolved_at",
"EXAMPLE_QUERY: FROM support-cases | WHERE product == \"elasticsearch\" AND status == \"open\" | KEEP case_number, subject, severity, created_at | LIMIT 20"
],
"description": "Tells the agent which index holds support-case metadata and how to query it, so it can choose the right data source and narrow to relevant cases before deeper analysis."
}
}
```


The profiles helped the agent choose a source and form a query. They reduced orientation work, especially when index or field names were unclear, but they didn’t remove the main cost of a case investigation. After selecting the indices, the agent still had to gather the case record and reconstruct the conversation. They also still had to follow links to engineering and knowledge sources and then reconcile the evidence. Routing was useful, but the repeated work was synthesis.


## **Changing the unit of context**


That observation led to case-level KIs. The workflow materializes one evidence-aware snapshot per eligible case. It gathers the case record, recent material conversation events, linked engineering issues and comments, and linked knowledge articles. The resulting KI document contains a stable case identifier and source references. It also contains tags and freshness metadata.


The design became more detailed as we worked through support specific nuances. Support conversations contain provisional theories, later corrections, administrative status changes, and occasionally separate incidents inside one case. A single fluent summary can flatten those distinctions. The case schema therefore records incident phases, a hypothesis ledger with confirmed, inferred, contradicted, and unresolved states, root cause status and confidence, technical outcome, reusable learning, and more.


Since large language models (LLMs) can hallucinate some of these details, we added deterministic checks after generation. For example:


-


An unresolved root cause must be empty and have low confidence.


-


An inferred root cause cannot have high confidence.


-


A case marked closed in a customer relationship management (CRM) system is evaluated separately from whether the technical issue was resolved.


These checks repair structural contradictions without requiring another model call to reinterpret the evidence. A representative KI document with selected fields looked like this:


```text
{
"_id": "1234567",
"_source": {
"type": "ki",
"title": "Case 1234567: Cluster writes blocked after disk flood-stage watermark breach",
"origin": { "uri": "case://1234567" },
"tags": [
"ki-kind:case-intelligence",
"support-data",
"cohort:closed",
"technical-outcome:resolved",
"rca-confidence:high"
],
"references": [
{ "uri": "case-number://1234567" },
{ "uri": "https://github.com/elastic/elasticsearch/issues/00000" }
],
"content": [
"CASE_NUMBER: 1234567",
"STATUS: Closed",
"PRIORITY: High",
"SUMMARY: A production cluster stopped accepting writes after a data node crossed the disk flood-stage watermark, which put all indices into read-only mode; freeing disk and clearing the block restored writes.",
"PROBLEM_AND_IMPACT: Indexing failed cluster-wide with 'FORBIDDEN/12/index read-only'; the customer's ingest pipeline was stalled for 1 hour.",
"PRODUCTS_AND_COMPONENTS: Elasticsearch, disk-based shard allocation.",
"INCIDENT_PHASES: Phase 1: disk usage crossed the 95% flood-stage watermark, indices auto-set to read-only. || Phase 2: disk freed and read-only block cleared, writes resumed.",
"HYPOTHESIS_LEDGER: Flood-stage watermark breach auto-applied a read-only block (confirmed); node stats showed disk at 96% and cluster logs recorded the flood-stage event.",
"ROOT_CAUSE: A data node exceeded the flood-stage disk watermark, so Elasticsearch automatically applied a cluster-wide read-only block to protect the nodes.",
"ROOT_CAUSE_STATUS: confirmed",
"ROOT_CAUSE_CONFIDENCE: high",
"RESOLUTION_OR_CURRENT_STATE: Freed disk space (removed stale snapshots/indices), cleared the read-only block, and verified writes resumed; recommended more headroom plus watermark alerting.",
"TECHNICAL_OUTCOME: resolved",
"REUSABLE_LEARNING: When every index goes read-only at once, check disk watermarks first; the flood-stage block is applied automatically but must be cleared manually after space is freed."
],
"description": "Distilled root cause of a single case: symptom, confirmed root cause with high confidence, resolution, and a reusable lesson so the agent can explain the fix and find precedent for similar cases."
}
}
```


*Figure 1. The workflow grew in response to observed constraints. Each addition addresses a specific source of repeated work, noise, or retrieval failure.*


## **Generating KIs only when they’re useful**


After a few iterations, we realized that generating a KI for every case adds cost and noise. Many cases have little usable evidence. In some, there’s only a short intake message; in others, there’s no substantive feed or acknowledgement from a support engineer. So we added gates and filters to the workflow. For example:


-


A case proceeds when it has linked engineering evidence or at least two material conversational events, including evidence of support participation.


-


The workflow compares the stored KI watermark with the newest timestamp across the case and its material feed. An unchanged case reuses its existing KI instead of regenerating it every time.


-


The workflow limits corpus selection by focusing generation on recent cases or those otherwise likely to be queried, especially when the source corpus is large.


-


The workflow also refreshes the case KI when linked sources can change independently, rather than relying on an incomplete watermark.


Deferred cases remain available through raw lookup and can be reconsidered after new activity arrives. Provenance is part of the stored KI: References point back to linked sources, and the output records what was missing or unverifiable. The KI shortens routine investigation, while raw records remain available for current status, complete history, attachments, and evidence outside the generated snapshot.


*Figure 2. Case-level context moves repeated evidence gathering into a precomputed workflow. The response path keeps raw verification and fallback available.*


## **The retrieval skill is part of the system**


While workflows generate KIs, the agent requires retrieval guidance to use them effectively. To address this, we developed a retrieval skill which provides ES|QL templates for querying KIs stored in the index and guidance on when to use each retrieval method. For instance, a known case number is searched lexically because it’s an exact identifier. Questions about similar cases or precedents use hybrid lexical and semantic retrieval, while questions about official knowledge, engineering issues, comments, or other non-case entities begin with an index profile and then query the selected raw source.


The skill also assigns a clear role to each KI family. An index profile provides routing and field guidance, while a case KI provides a bounded snapshot of the evidence relevant to one case. If no case KI is available, the agent treats that as a cache miss and follows the index profile guidance back to the underlying data, rather than assuming that the available context is complete.


That distinction matters during a typical case lookup. For a question such as *What is the current priority of case 1234567?* , the agent retrieves the case KI by case` _id` to understand the investigation and its supporting evidence. It then checks the live case record for values that can change, including status and priority, along with updated_at. If the live record is newer than the KI’s last update, the live value takes precedence. The KI remains useful for durable evidence, while the source record remains the reference for current state. The next eligible refresh rebuilds the KI using timestamps from the case and its material feed, in addition to linked sources.


This separation was shaped by an evaluation failure. In an earlier version, the agent used the case KI to plan retrieval but issued a raw source query using an inferred schema instead of the fields supplied by the index profile. The query failed, causing retries and additional model work. Later versions of the skill made the precedence rules, source roles, field guidance, and recovery steps for mapping errors explicit.


*Figure 3. The retrieval skill routes by question type. Exact case lookup, precedent search, and non-case retrieval use different context paths, with raw evidence available for verification and fallback.*


## **What we observed**


We evaluated the agent using 20 distinct questions, with approximately 10 for each workflow: case investigation/postmortem summarization and shorter technical support Q&A across several source types. We used three trials per question to measure variation from run to run. The examples in this post were tested on an Elastic Cloud Hosted deployment running Elasticsearch 9.4.2.


Different agent configurations used the same answering model and comparable tool conditions. They varied only in whether the agent had access to KIs and, if so, which ones: index-profile KIs, case-level KIs, or both. We measured factuality against expected answers, input and output tokens, latency, and tool execution failures. A separate LLM judge scored factuality.


Case-level KIs produced the clearest signal of operational efficiency in both workflows. Relative to the raw index baseline, observed input token use and latency changed as follows:


**Workflow**


**Input tokens**


**Latency**


Case investigation


About 58% lower


About 40% lower


Technical support Q&A


About 43% lower


About 17% lower


We noticed that index profiles shortened source orientation but left consolidation to the agent, whereas case-level KIs supplied a compact evidence bundle aligned with the requested output. That led to fewer raw queries and reduced the opportunity for schema- and partition-related tool errors.


Factuality varied by workflow, but we didn’t observe a statistically significant reduction in this evaluation.


A note on interpreting these results: The evaluation was performed on a small dataset, which is common and useful early in the agent development lifecycle when large labeled evaluation datasets aren’t yet available. We therefore treat the results as directional signals for comparing variants. This mirrors strategies outlined in engineering blogs by[Elastic](https://www.elastic.co/search-labs/blog/ai-agent-evaluation-elastic) and other companies, such as[Anthropic](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) : Start with a small number of tasks drawn from real failures, and then expand the suite as effects become smaller and the product matures.


## **Why case-level KIs fit support work**


The case-level strategy matched the shape of support investigation work in several ways:


-


A case number is a stable retrieval anchor.


-


The expensive operation is repeated consolidation across the same source relationships.


-


Much of the evidence used for explanation is durable, while volatile states can be verified separately.


-


The KI schema mirrors the type of questions an engineer asks during case summary and RCA work.


-


Maturity and freshness controls limit generation to cases with enough evidence and likely reuse.


Index profiles still have value. They help with source discovery, schema orientation, non-case questions, and fallback. For case investigation, they leave the cross-source reconstruction inside the response path. Case-level KIs remove part of that recurring work, which is the main reason we expected lower token use and latency in this domain.


## **The combined strategy exposed a composition problem**


While evaluating the responses, we made a counterintuitive observation: Providing both index profiles and case KIs didn’t improve on using case KIs alone. Inspection of the trace showed that the agent understood some of the case context but lacked a dependable rule for composing the two KI families. It used a case-level KI for planning but then ignored the index profile’s schema guidance when querying raw data, leading to failures and suboptimal responses.


This resulted in a practical lesson for improving guidance in the retrieval skill: Context sources need roles and precedence. The agent must know which source can support an answer, which source only routes to evidence, when verification is required, and what to do after a cache miss or mapping error. Two individually useful context types can create additional work when those contracts are implicit.


## **What we learned about precomputed context**


In this support workflow, the most useful unit of context was a case and the evidence connected to it: the case record, conversation, linked engineering work, and relevant knowledge. Preparing that evidence ahead of time meant that the agent didn’t have to reconstruct the same relationships for every response. Compared with the raw index baseline, the case-level approach was associated with lower observed input token use and latency, while factuality didn’t show a statistically significant reduction.


Because case state can change, the system still checks source data for values, such as priority and case status, and falls back to raw data when the available evidence doesn’t justify a case-level KI. Next, we plan to test how well this design holds with support data from external systems, such as Salesforce, and to identify any adjustments needed.
