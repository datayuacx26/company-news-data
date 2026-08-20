---
schema_version: "1.0.0"
document_id: "c9c09a7a4c5d8418b2d49da251eabe5742c0bd23306f1680763ae85cc0e77253"
company_key: "navan-inc-class-a-common-stock"
company: "Navan Inc."
source_id: "navan-inc-class-a-common-stock-news-import-0382cbfb66e0"
canonical_url: "https://navan.com/blog/build-proactive-ai-agents"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-15T09:27:13.582995+00:00"
fetched_at: "2026-08-15T09:27:14.989144+00:00"
content_hash: "sha256:7294138f0e14e6dba10a62674df262839c765251604b0c2cfc1f2efd3eed350c"
---

# How to build proactive AI agents that only act when it matters

Most AI agents begin with an explicit request. A user asks a question, the agent gathers context, calls a tool, and returns an answer.


A proactive agent begins somewhere else. An event occurs, the system notices it, and the agent must decide whether that event is important enough to interrupt the user.


Consider a traveler with an upcoming reservation and a cancellation window that’s still open. The system could send a reminder. But should it send one now? Is the reservation still active? Has the user already handled it? Is the information current? Would the message lead to a useful action, or would it become one more notification to dismiss?


The difficult part of the engineering problem isn’t generating the message — it’s deciding whether the system should send it.


That distinction changes the architecture. A proactive system is not simply a chatbot connected to a notification API. It’s a decision system that must know when to act now, when to wait, and when to stay silent.


## Three outcomes, not two


Traditional notifications are binary: send or don’t send. Proactive AI requires three outcomes:


**Outcome**


**Meaning**


**Act now**


The candidate is eligible, timely, sufficiently grounded, and useful.


**Wait**


The signal is premature, but has a future point where it should be reconsidered.


**Stay silent**


Policy, duplication, stale data, or low value makes interruption unjustified.


Silence is not necessarily a failure. In a proactive product, it can be the safest and most useful engineering outcome.


*Figure caption: A proactive decision model with three valid outcomes: act now, wait, or stay silent.*


## Separate detection from authorization


Architectural separation between candidate detection, decisioning, delivery, and conversational continuation ensures clear ownership:


- **Candidate producers** spot moments worth evaluating but do not authorize delivery.
- **The decision engine** evaluates if and when to act.
- **The delivery layer** executes approved decisions without re-evaluating logic.
- **The conversational back end** manages post-engagement experience without owning scheduling.


This modularity allows teams to add new event sources, delivery channels, or experiences without rebuilding the core agent.


*Fig caption: A proactive system separates candidate detection, decisioning, delivery, and the experience that follows engagement.*


## Policy before probability


Deterministic rules must run before invoking an LLM. Application logic should handle eligibility, channel permissions, deduplication, contact cadence, feature flags, and required context presence.


```text
policy_result = evaluate_policy(candidate, context)
if policy_result is blocked:
return stay_silent(policy_result.category)
continue_to_evidence_checks()


```


The governing principle is simple: Policy defines the space in which the model may operate. The model does not define the policy.


This reduces unnecessary model calls, makes hard constraints directly testable, and prevents a persuasive model response from overriding consent, cadence, or safety rules.


The next step is not immediately to “ask the LLM.” It’s to determine whether the system has enough reliable evidence to reason at all.


## Evidence quality before contextual judgment


Before applying LLM reasoning, verify underlying facts deterministically: ensure that records exist, target objects are active, time stamps are current, critical tools succeeded, and attention budgets aren't exceeded.


Scheduling must anchor to explicit domain time stamps and time zones rather than vague relative terms. When evidence is incomplete or stale, remaining silent is safer than generating a fluent but ungrounded message.


## Use the LLM for the ambiguous middle


Once a candidate passes policy and evidence checks, an LLM handles the ambiguous contextual decisions: Is this useful? Which details matter most? How should it be framed?


The model operates within a bounded reasoning layer, receiving approved inputs, adhering to versioned workflow rules, and producing output that must pass deterministic validation before execution. Frameworks like graph orchestration make these boundaries explicit while preserving system portability.


*Figure caption: The LLM operates between approved inputs and deterministic validation rather than directly controlling application state or delivery.*


This creates a clean extension model. A new proactive use case can be introduced through a skill, a controlled tool set, a structured result, and a corresponding evaluation suite. The orchestration layer remains stable while the library of supported workflows grows.


## Turn probabilistic reasoning into an application contract


Unstructured text is an unsafe system interface. The decision engine requires a predictable contract that separates the business decision from the health of the reasoning run.


```text
DecisionEnvelope =
SUCCESS {
evidence_status: COMPLETE | PARTIAL
limitations?: [ ... ]


decision:
ACT_NOW {
rationale,
delivery_intent,
user_message
}


| WAIT {
rationale,
reconsideration_time
}


| STAY_SILENT {
rationale
}
}


| FAILURE {
failure_category,
retryable,
safe_action: NO_USER_ACTION
}


result = validate(model_output, DecisionEnvelope)


if result is invalid:
retry_generation_within_limits()


if result is still invalid:
return FAILURE {
failure_category: OUTPUT_VALIDATION,
retryable: false,
safe_action: NO_USER_ACTION
}


```


A partial failure means that one or more non-critical inputs were unavailable, but enough validated evidence remains to make a safe decision. In that case, the result is still successful, but it carries *evidence_status: PARTIAL* and records the relevant limitations. The system may act, wait, or stay silent only when the missing information is not required for that outcome.


A total failure means that critical evidence is unavailable, the reasoning run cannot produce a valid contract, or validation continues to fail after bounded retries. A total failure does not become an artificial *STAY_SILENT* decision. It is recorded separately as *FAILURE* , and no user-facing action is produced.


When critical evidence is temporarily unavailable and the system knows when the information can be reconsidered, *WAIT* may still be a valid decision. Without a reliable reconsideration point, the run should fail safely instead of manufacturing certainty.


This separation is important because a valid non-delivery decision and a failed execution are not the same thing:


- **STAY_SILENT** means the system successfully concluded that interruption was unjustified.
- **WAIT** means the system successfully concluded that the opportunity may become useful later.
- **FAILURE** means the system could not produce a trustworthy decision.


Validation therefore does more than reject malformed output. It ensures that each outcome has the information it requires, distinguishes degraded evidence from total execution failure, and keeps model failures separate from policy, dependency, and delivery errors.


Scale the runtime, not just the prompt


LLM calls and data-fetching must run asynchronously to prevent long-lived request locks.


- **Asynchronous processing:** Intake acknowledges candidate signals immediately; decisioning processes in the background.
- **Idempotency:** Enforce idempotency across intake jobs, callback delivery, and side-effect boundaries to prevent duplicate notifications.
- **Failure handling:** Invalid inputs fail early, transient errors retry with backoff, and malformed outputs prompt regeneration before defaulting to silence.


*Figure caption: Decision processing is asynchronous, while delivery occurs only after a validated outcome.*


The notification is the doorway


A notification transitions the user into a conversation — it’s not the complete experience. The initial handoff should supply typed context to launch the right workflow, but the conversational back end must retrieve fresh data to handle domain changes. Supporting event-based streaming (tool progress, partial content) further improves user clarity.


The notification is the doorway. The useful agent experience begins after the tap.


## Observe decisions, not hidden reasoning


Traditional service logs can show that a function ran, a model responded, or a callback succeeded. They do not necessarily explain why the system interrupted the user.


A useful decision trace records the policy result, evidence quality, final outcome, and what happened afterward. The following is a conceptual public example rather than a production contract:


```text
{
"traceId": "<correlation-id>",
"candidateCategory": "<candidate-category>",
"runStatus": "completed",
"policy": "allowed",
"evidence": {
"status": "partial",
"limitations": ["Optional context was unavailable"]
},
"decision": {
"outcome": "wait",
"summary": "The signal is valid, but the timing is premature.",
"reconsiderAt": "<ISO-8601 timestamp>"
},
"delivery": "not_requested"
}
```


This compact structure keeps the important concerns independently queryable. runStatus distinguishes a completed decision from a failed execution. evidence.status shows whether the decision used complete, partial, or insufficient evidence. decision.outcome records whether the system acted, waited, or stayed silent, while delivery records what happened after the decision.


For failed runs, the trace should record a broad failure category and avoid producing a user-facing action.


The trace should not contain hidden chain-of-thought, raw prompts, complete model responses, or unnecessary personal data. Short decision summaries, broad categories, and correlation identifiers are enough to answer the operational question:


*Why did the system act, wait, or stay silent?*


If the architecture cannot answer that question, improving relevance and trust becomes guesswork.


## Evaluate behavior at multiple layers


Proactive agents require multi-tiered testing beyond sentence-matching:


- **Unit tests (deterministic core):** Prove hard constraints (consent rules, time formatting, deduplication) pass without live LLM calls.
- **Integration tests (execution path):** Use mock LLM responses to test background workers, tool invocation, retries, and idempotency.
- **Scenario evaluations (behavior):** Evaluate real model reasoning against curated scenarios (clear acts, waits, silences, and partial tool failures).
- **Adversarial evaluations (boundaries):** Stress-test the system with malformed input, stale dates, injection attempts, and outage failures to guarantee safe default states.
- **Regression evaluations (stability):** Re-run scenario suites when updating prompts, schemas, or models to detect hidden degradations.


## Design for the next use case


The real measure of a proactive platform is not how polished its first workflow appears — it’s how safely the architecture can absorb the next one.


That requires versioned workflow guidance, typed contracts, feature flags for controlled rollouts, and evaluations that grow alongside the product. Model selection should remain an implementation detail behind a stable interface, allowing new use cases to fit directly into the existing decision model rather than spawning isolated notification pipelines.


Ultimately, the goal of proactive AI is not to contact users more often, but to know when contact is justified. That requires deterministic policy, trustworthy evidence, structured outputs, asynchronous execution, and decision-level observability.


Hard constraints belong in code, LLMs belong in the ambiguous middle, and *act now* , *wait* , and *stay silent* are all valid engineering outcomes. A proactive AI platform is, at its core, an interruption-control system with an LLM inside it. When engineered well, the agent becomes genuinely helpful without becoming intrusive.


---


Share this article


---


-
-
-


Tags


Smarter Travel. Cleaner Books. Happier Teams.


Navan’s all-in-one platform — built to streamline T&E.


[Get started](https://navan.com/get-started)


---


---


Tags


Share this article


-
-
-


---


This content is for informational purposes only. It doesn't necessarily reflect the views of Navan and should not be construed as legal, tax, benefits, financial, accounting, or other advice. If you need specific advice for your business, please consult with an expert, as rules and regulations change regularly.
