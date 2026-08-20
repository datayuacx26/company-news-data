---
schema_version: "1.0.0"
document_id: "2fc6b8ffde582cd971675f076160df0aa99f7b422b8f69ad793301fbf152af4e"
company_key: "yc-redouble-ai"
company: "Redouble AI"
source_id: "yc-redouble-ai-rss-94bf26a72f2a"
canonical_url: "https://deconvoluteai.com/blog/pii-pseudonymization-agentic-systems"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-27T11:30:56.530208+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:6053875a532c84f6c4b0e05a3ebe62e017fba29034778976b3422dbe9fff2eb0"
---

# PII Pseudonymization for the Full Agentic Loop with Legend

## Summary


AI agents leak PII at every layer of the pipeline, not just the user prompt. Legend (` legend-pii` ) is an open source Python library that intercepts PII at all four boundaries of the agentic loop, replacing sensitive values with plausible pseudonyms and restoring originals in the final response. This post covers the design, the replacement engine, and benchmark results comparing Legend and Presidio across eleven entity types.


## The Problem: PII Leaks Through Every Layer of the Agentic Loop


An AI agent in production does not just read a user prompt and return an answer. It calls tools, fetches records from external APIs, writes intermediate results to memory, and emits traces and observability events at every step. Personally identifiable information enters and exits the pipeline at each of those points, not only at the input boundary.


The exposure risk extends beyond obvious fields like names or email addresses. Rocher et al. (2019) demonstrated that even partial, seemingly innocuous combinations of attributes are sufficient to re-identify individuals in datasets assumed to be anonymous. An agent processing customer support tickets, medical queries, or financial requests will routinely handle exactly that kind of partial PII: account numbers alongside locations, dates alongside names, identifiers scattered across tool results. Prior work on[hidden attack surfaces in agentic systems](https://deconvoluteai.com/blog/attack-surfaces-rag) and[indirect prompt injection and malicious compliance](https://deconvoluteai.com/blog/securing-llms-malicious-compliance-benchmark) has shown that tool results are also a realistic attack vector; an adversarially crafted API response can redirect agent behavior in ways the developer never anticipated, and any PII embedded in that response travels with it.


Regulatory frameworks are catching up to this reality. The EU AI Act and GDPR both treat the pipeline as the system under evaluation, not just its input boundary, and penalties for non-compliance are substantial.


The instinctive engineering response is to strip PII at the prompt boundary before it enters the agent's context, which covers one exposure point while leaving the arguments the agent constructs for tool calls, the results those tools return, and the final response sent back to the user all unaddressed. Tool results are a particular blind spot: an agent fetching a customer record, a calendar event, or a document from an internal knowledge base will receive PII it never saw in the original prompt, sourced from systems the developer may not fully control.


Legend is a Python library that closes all four exposure points. It intercepts PII at each boundary of the agentic loop, replaces it with contextually plausible pseudonyms before it enters any intermediate layer, and restores the original values in the final response. The agent reasons over pseudonyms throughout. Real values never appear in tool arguments, tool results, logs, or traces.


## Sanitation vs Pseudonymization: Why Redaction Breaks Agents


The standard approach to PII in NLP pipelines is sanitation: replacing sensitive values with entity type labels, hashes, or fixed redaction tokens. For example, a name becomes` <PERSON>` , a credit card number becomes` <CREDIT_CARD>` . This is a one-way operation and the original value is discarded.


For a classifier or a batch analytics job this is an acceptable tradeoff, because the model does not need to know that the name was John Doe, it only needs to know that a person's name appeared at that position in the text.


However, agents are a different class of system. An agent reasoning over` \[PERSON\] owes \[PERSON\] $12,000 and the invoice is overdue` cannot draft a follow-up email, look up an account, or cross-reference a payment record, because the values it would need to act on have been replaced with type labels that carry no content. Yermilov et al. (2023) found that pseudonymization, replacing sensitive values with realistic substitutes rather than labels, preserves downstream utility significantly better than anonymization while maintaining the same privacy guarantee. Xiao et al. (2026) offer a useful theoretical grounding for this: privacy in context is better understood as an information sufficiency problem than a labeling one, which is precisely why a plausible substitute name outperforms \[PERSON\] for an agent that needs to act on the value. Zharmagambetov et al. (2025) further demonstrated that autonomous agents leak PII through tool calls and intermediate reasoning steps in ways that are difficult to detect at the model level, reinforcing that the problem is structural, not incidental.


Pseudonymization threads this needle. A name is replaced with a different plausible name, an IBAN with a structurally valid IBAN for the same country, an email with a syntactically correct address at a non-resolving domain. The agent receives text it can reason over while the original values are held in a session-scoped entity map and restored only in the final response.


## The Four-Boundary Model


PII handling in most agent pipelines stops at the user prompt. Legend intercepts at all four points where sensitive data crosses a boundary, see figure 1.


**Boundary A** is the user prompt entering the system. PII is detected and replaced with pseudonyms before the prompt enters the agent's context window, and the entity map is initialized here.


**Boundary B** is tool call arguments leaving the system. In the happy path the agent context already contains only pseudonyms placed there by Boundary A, so Boundary B fires rarely. It serves as defense-in-depth, catching PII introduced via system prompts, injected context, or any value that entered the pipeline through a non-standard path.


**Boundary C** is tool results re-entering the system. A tool may return PII that was never in the original user prompt, fetched from an external database, a calendar, or an internal knowledge base. This data is intercepted and pseudonymized before it enters the agent's context window. Known pseudonyms already in the entity map are recognized and left unchanged.


**Boundary D** is the final response leaving the system. No detection runs here. The entity map is used to revert all pseudonyms back to their original values, and the map is then destroyed. Real values are restored exactly once, at the point they are returned to the user.


```text
pipeline   =   DetectionPipeline  (  entities  =  [  "PERSON"  ,     "IBAN_CODE"  ,     "EMAIL_ADDRESS"  ]  )
engine   =   ReplacementEngine  (  )


async     with   PseudonymContext  (  pipeline  =  pipeline  ,   engine  =  engine  )     as   ctx  :
sanitized_prompt   =     await   ctx  .  sanitize_prompt  (  prompt  )               # Boundary A
sanitized_args     =     await   ctx  .  sanitize_tool_args  (  tool_args  )         # Boundary B
sanitized_result   =     await   ctx  .  sanitize_tool_result  (  tool_result  )     # Boundary C
clean_response     =     await   ctx  .  revert  (  agent_response  )                # Boundary D
```


The entity map is session-scoped, held entirely in memory, and cleared on exit regardless of whether the session completed normally or raised an exception. Real values are never written to disk, never transmitted over the network, and never survive the lifetime of the session. The full source is available[on GitHub](https://github.com/deconvolute-labs/legend) .


Figure 1: The four boundaries of the Legend pipeline. Boundary A intercepts the user prompt before it enters the agent context. Boundary B intercepts tool call arguments before they leave toward an external tool or API. Boundary C intercepts tool results before they re-enter the agent context. Boundary D reverts all pseudonyms in the final response before it reaches the user. The session-scoped entity map connects all four boundaries and is destroyed on session exit.


## How Legend Works


The detection pipeline combines two complementary approaches. Pattern-based entity types such as credit card numbers or email addresses are detected using[YARA rules](https://deconvoluteai.com/blog/yara-rules-llm-prompt-security) . Semantic entity types that require contextual understanding, names, locations, and organizations, are handled by a neural network model, currently spaCy's` en_core_web_lg` . Both detectors run on every input and vote on every span, where a span is a contiguous substring of the input text identified as containing a specific entity type. Where detectors disagree on overlapping spans, the higher-confidence detection wins, with a small confidence boost applied to YARA scores to account for the higher precision of pattern matching.


Once a span is detected, the replacement engine follows a strict three-step lookup before generating anything. First it checks whether the span is already a known pseudonym in the entity map, in which case it is left unchanged. This is the critical case at Boundaries B and C, where the agent may have constructed tool arguments from pseudonymized context or a tool result may echo back a value the map already knows. Second it checks whether this real value has been seen before in the current session, in which case the existing pseudonym is reused. Only on a genuine new detection does the engine call a generator and write a new entry to the entity map. This three-step design guarantees that no value is pseudonymized twice and that the same real value always maps to the same pseudonym within a session.


The generators are designed to produce substitutes an agent can reason over correctly. Names are replaced with locale-appropriate names, IBANs with structurally valid IBANs preserving the country code and a correct MOD-97 check digit, email addresses with syntactically valid addresses at a non-resolving domain, etc. The goal in each case is a value that is plausible in context without being real.


The revert pass at Boundary D works from pre-computed variants stored in the entity map entry at replacement time. It performs exact string matching over the response text, replacing every known fake value and its variants with the corresponding original. No fuzzy matching is used in the current version; if the agent modifies a pseudonym in a way not covered by the variant list, it passes through unchanged. This is a documented limitation and the benchmark results reflect it.


## Benchmark: Legend vs Presidio


To validate Legend's detection and pseudonymization quality, we built an open source benchmark available at[deconvolute-labs/pii-benchmark](https://github.com/deconvolute-labs/pii-benchmark) , designed to run head-to-head evaluations with a reproducible dataset and consistent methodology. As a baseline we use Presidio, a widely recognized open source PII detection library from Microsoft, see Asthana et al. (2025). Presidio is evaluated on detection metrics only, because it is a detector and sanitizer, not a pseudonymizer. Current results and plots are in the repository


## Conclusion


Legend addresses a gap that becomes visible only when you look at the full agentic loop: PII does not only enter through the user prompt, and stripping it at one boundary while leaving the others unprotected is not a solution. The four-boundary model, session-scoped entity map, and format-preserving generators combine to give agents text they can reason over correctly, without real values appearing anywhere in the intermediate pipeline. The benchmark results against Presidio provide a reproducible baseline for detection quality, and the revert accuracy numbers validate the round-trip guarantee that pseudonymization requires but sanitation cannot provide.


The library is open source and available at[deconvolute-labs/legend](https://github.com/deconvolute-labs/legend) . If you find it useful or are building something on top of it, a star on the repository helps others find it.


## Frequently Asked Questions


**What is the difference between sanitation and pseudonymization?** Sanitation is a one-way operation: the original value is replaced with a label or hash and cannot be recovered. Pseudonymization replaces it with a realistic substitute and maintains a mapping that allows the original to be restored at the output boundary.


**Does Legend work with any agent framework?** Yes. Legend has no dependency on LangChain, LangGraph, or any other framework. It integrates via the` PseudonymContext` async context manager, which wraps any async code that calls your agent and its tools.


**What happens if the agent modifies a pseudonym?** The revert pass performs exact string matching against pre-computed variants stored in the entity map. If the agent modifies a pseudonym in a way not covered by the variant list, that span passes through unchanged. Fuzzy matching is on the roadmap.


**Are real values ever written to disk or logs?** No. The entity map lives in memory for the lifetime of a single session and is cleared on exit under all conditions. Observability events contain only pseudonyms, boundary labels, and session identifiers.


**How does Legend handle the same PII value appearing across multiple boundaries?** The forward lookup in the replacement engine finds the existing entity map entry and reuses the same pseudonym rather than generating a new one. The same real value always maps to the same pseudonym within a session.


**Is Legend production-ready?** The current release is suitable for evaluation and integration work. More is on the roadmap. Follow the repository for updates.


**How do I add support for a custom entity type?** Register a custom` GeneratorBase` implementation in the generator registry at construction time. For pattern-based detection, point` YARADetector` at a directory containing your custom YARA rules.


## References


Rocher, L., Hendrickx, J. M., and De Montjoye, Y.-A. (2019). Estimating the success of re-identifications in incomplete datasets using generative models. *Nature Communications* , 10(1), 3069.[https://doi.org/10.1038/s41467-019-10933-3](https://doi.org/10.1038/s41467-019-10933-3)


Yermilov, O., Raheja, V., and Chernodub, A. (2023). Privacy- and utility-preserving NLP with anonymized data: A case study of pseudonymization. In *Proceedings of the 3rd Workshop on Trustworthy Natural Language Processing (TrustNLP 2023)* , pages 232–241.[https://doi.org/10.48550/arXiv.2306.05561](https://doi.org/10.48550/arXiv.2306.05561)


Asthana, S., Mahindru, R., Zhang, B., and Sanz, J. (2025). Adaptive PII mitigation framework for large language models. arXiv:2501.12465.[https://doi.org/10.48550/arXiv.2501.12465](https://doi.org/10.48550/arXiv.2501.12465)


Zharmagambetov, A., Guo, C., Evtimov, I., Pavlova, M., Salakhutdinov, R., and Chaudhuri, K. (2025). AgentDAM: Privacy leakage evaluation for autonomous web agents. arXiv:2503.09780.[https://doi.org/10.48550/arXiv.2503.09780](https://doi.org/10.48550/arXiv.2503.09780)


Xiao, Y., Li, W., Wu, X., Ma, N., Song, Y., and Xuan, W. (2026). Say something else: Rethinking contextual privacy as information sufficiency. arXiv:2604.06409.[https://doi.org/10.48550/arXiv.2604.06409](https://doi.org/10.48550/arXiv.2604.06409)
