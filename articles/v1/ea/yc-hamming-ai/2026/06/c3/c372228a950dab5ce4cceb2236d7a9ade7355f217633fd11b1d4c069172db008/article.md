---
schema_version: "1.0.0"
document_id: "c372228a950dab5ce4cceb2236d7a9ade7355f217633fd11b1d4c069172db008"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/insurance-claims-intake-voice-agent-testing-runbook"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:23:18.688239+00:00"
content_hash: "sha256:88862131968776285ff8d81349ec8534e23a3ce49361adf1702dc41035e2a8c7"
---

# Insurance Claims Intake Voice Agent Testing Runbook

**Insurance claims intake voice agent testing** should prove that the caller's loss was captured, validated, routed, and preserved with evidence. It is not enough for the agent to sound empathetic and say, "I have started your claim."


If the agent only answers policy FAQs, use a smaller[voice agent workflow testing runbook](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) . This is for teams where a call can create a first notice of loss (FNOL), update a claim file, trigger an adjuster assignment, request documents, or route a distressed claimant to a human.


The failure mode is blunt: the transcript sounds right, but the downstream claim is still a draft, missing a reporter, attached to an unverified policy, assigned to the wrong path, or impossible to reconstruct during review.


> **TL;DR:** Test claims intake as a stateful workflow:
>
>
> - **State:** draft, open, incomplete, escalated, duplicate, withdrawn, or failed.
> - **Required fields:** policy, loss date, reporter, contact route, incident facts, exposure, attachments, and consent.
> - **Invariants:** no future loss dates, no claim submission without required fields, no coverage promise, no payment promise, no silent PII leak.
> - **Evidence:** transcript span, tool trace, claim ID, validation result, escalation reason, reviewer route, and cleanup status.
>
>
> A claims-intake test passes only when the spoken outcome, tool calls, final claim state, and audit evidence agree.


> **Methodology Note:** This runbook is based on Hamming's analysis of production voice agent calls where claim intake, workflow state, and evidence quality changed the customer outcome across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> It also uses public Guidewire and NAIC documentation to ground the FNOL state model and claim-file evidence samples. It is not legal advice; map these patterns to your own claim system, product line, and jurisdiction.


**Last Updated:** June 2026


**Related Guides:**


- [Voice AI Testing for Insurance](https://hamming.ai/industry/insurance) - insurance-specific product context
- [Voice Agent Workflow Testing Runbook](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - broader workflow and state-transition coverage
- [Voice Agent Sandbox Testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) - test side effects without writing to production systems
- [Structured Output Validation](https://hamming.ai/resources/voice-agent-structured-output-validation-checklist) - prove extracted fields match what the caller said
- [Regulatory Script Adherence](https://hamming.ai/resources/regulatory-script-adherence-voice-agents) - required disclosures, prohibited statements, and audit evidence
- [Customer Workflow Rules Testing](https://hamming.ai/resources/voice-agent-customer-workflow-rules-testing-template) - customer-specific policy and tenant rules
- [Failed Production Call Regression Tests](https://hamming.ai/resources/failed-production-call-regression-test-runbook) - convert missed claims paths into replayable tests
- [Production Call Review Triage](https://hamming.ai/resources/voice-agent-production-call-review-triage) - find the few claims calls worth reviewing


## What Makes Claims Intake Hard to Test?


Claims intake is non-deterministic because callers do not report losses in a neat order.


They call from accident scenes. They forget policy numbers. They give a date, then correct it. They mention injuries late. They ask whether something is covered before the agent has enough facts. They may need empathy, but the system still needs structured data.


The test should not demand one fixed conversation. It should demand the right outcome.


> **Claims-intake correctness:** the agent collected enough facts for the allowed claim state, avoided unsupported coverage or payment promises, escalated high-risk cases, and preserved evidence that lets a reviewer reconstruct what happened.


That is a different test from "did the agent answer politely?" Politeness matters. It does not prove the workflow.


We used to treat claims intake as a long form-filling call. Now we treat it as a state transition with guardrails. The agent can take many valid paths, but some things should never happen.


## The FNOL State Model to Test


Start with the claim lifecycle your system actually uses. For many property and casualty systems, the useful model begins with FNOL: first notice of loss.


[Guidewire's ClaimCenter documentation](https://docs.guidewire.com/cloud/is/202603/cloudapibf/cloudAPI/ClaimCenter/fnol/executing-fnol/c_the-FNOL-process-in-ClaimCenter.html) describes an FNOL process where a claim can begin as a draft, then become open when enough information exists to enter adjudication. It also distinguishes verified policies from unverified policies. That distinction is why claims intake tests need state guardrails.


State What It Means Voice-Agent Test Guardrail Block Release When


No claim created Caller gave too little information or declined to proceed Agent explains what is missing and writes no claim Agent says a claim was started but no record exists


Draft claim Enough information exists to save an intake record, but not enough to submit Draft has policy/loss/reporter evidence and missing-field reason Draft is saved without required traceability


Open claim Claim met validation and assignment requirements Open claim ID exists, assignment route is recorded, caller receives safe next steps Agent says claim is open while backend remains draft


Escalated claim Human review is required because risk, ambiguity, distress, fraud, injury, or coverage uncertainty is present Escalation reason, handoff target, and evidence packet exist Agent continues as if routine intake is allowed


Duplicate candidate Caller may be reporting the same loss again Agent checks prior claim context and avoids duplicate submission A retry or repeat caller creates a second claim


Failed intake Tool, validation, or assignment failed Agent gives safe fallback and preserves failed-state evidence Agent promises success after an error


Guidewire's[draft-claim submission docs](https://docs.guidewire.com/cloud/is/202603/cloudapibf/cloudAPI/ClaimCenter/fnol/executing-fnol/c_submitting-a-draft-claim.html) also show why "created" is not enough. A draft can fail submission because of validation or assignment errors. Your test should catch that, not smooth it over.


## Build a Claims Intake Test Matrix


Write the matrix before you write the prompt. A good claims-intake matrix names the caller condition, fixture state, allowed branch, forbidden branch, and evidence.


Scenario Fixture Setup Expected Outcome Forbidden Outcome Evidence Required


Clean auto FNOL Verified policy, loss date today, reporter matches policyholder Draft or open claim with loss facts, reporter, vehicle, contact, and next step Agent invents coverage or settlement timing Claim ID, policy lookup, field map, transcript spans


Missing policy number Caller knows name/address but not policy number Agent uses approved lookup path or creates allowed unverified-policy flow Agent blocks without offering supported lookup Lookup trace, verification decision, missing-field note


Date conflict Caller first says "yesterday," later says "last Friday" Agent clarifies and stores one resolved loss date with rationale Both dates appear as final truth Clarification span, resolved field, reviewer note


Injury or urgent safety signal Caller mentions injury, medical emergency, threat, or unsafe location Agent escalates or gives approved safety handoff Agent continues routine intake Escalation trigger, timestamp, handoff result


Fraud or inconsistency signal Caller gives inconsistent ownership, location, or timing facts Agent records inconsistency and routes to review Agent accuses caller or ignores conflict Conflict fields, review queue ID, neutral language check


Document request Claim needs photos, receipts, police report, or inventory Agent requests allowed documents and records channel Agent asks for unsupported sensitive data Document request list, delivery channel, consent


Duplicate loss Same caller reports same incident twice Agent detects prior claim candidate and routes safely Duplicate open claim is created Prior-claim lookup, duplicate decision, final state


Tool timeout Policy or claims API times out Agent gives safe fallback and writes no false success Agent says the claim is submitted Error trace, no-write guardrail, fallback transcript


[NAIC consumer guidance](https://content.naic.org/article/consumer-insight-navigating-claims-process-recover-rebuild) is useful for scenario design because it names the kinds of information claimants are often asked to provide: insurance information, contact information, damage descriptions, inventories, photos or videos, repair receipts, and follow-up communication.


Do not turn that into a universal legal checklist. Use it to make the test cases feel like real claims calls.


## Invariants That Should Never Break


Non-deterministic paths still need deterministic rules.


These are the rules that should fail the run every time, no matter how natural the conversation sounded.


Invariant Why It Matters Test Method


Loss date is not in the future Future loss dates usually indicate ASR, caller, or extraction error Schema check plus transcript span


Required reporter field exists before submission A claim without reporter context may fail validation or review Field completeness check


Policy status is explicit Unknown policy state should not become a confident coverage statement Policy lookup trace and language check


No guaranteed coverage statement The agent should not promise coverage, liability, or payment before adjudication Semantic prohibited-claim evaluator


High-risk triggers escalate Injury, fraud, distress, threat, legal complaint, or vulnerable-caller signals need a human path Classifier plus handoff guardrail


Duplicate protection exists Repeated calls and retries should not create duplicate claims Idempotency key and prior-claim lookup


Evidence packet is complete Claims workflows need reconstruction, not just a transcript Metadata completeness check


Sensitive fields are minimized Claim details can include PII, financial facts, medical details, or addresses[PII/security review](https://hamming.ai/resources/voice-agent-security-review-questions) and redaction policy


> **Invariant check:** a deterministic rule that must pass even when the conversation path varies. For claims intake, invariants are the guardrails that keep a flexible agent from creating unsafe claim records.


Pair these with[structured output validation](https://hamming.ai/resources/voice-agent-structured-output-validation-checklist) . The agent can summarize beautifully and still extract the wrong loss date, claimant role, address, or exposure.


## How to Test Non-Deterministic Branches


Do not make CI flaky by requiring identical transcripts.


Use repeated scenario runs for the conversational layer, but deterministic checks for the state layer.


Layer What Varies What Must Stay Stable Suggested Gate


Conversation Wording, order of clarification, empathy phrasing Required information is requested before submission 20-50 runs for critical paths


Tool calls Retry timing, optional lookup path, handoff timing Allowed tools only, correct order for state-changing calls 99% allowed-tool compliance


Structured claim Caller phrasing and ASR variants Final normalized fields match the fixture or human-reviewed answer 95%+ field correctness for blocking fields


Escalation Caller wording and emotional state High-risk signals route to the correct human path 100% for severe injury, fraud, legal, or safety triggers


Evidence Transcript chunking and reviewer fields Required packet fields are present 99%+ completeness


This is where[sandbox side-effect testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) matters. Run the claim write against fixture data or a test claims environment. If a production write is unavoidable, keep it allowlisted, owner-approved, and outside normal CI.


For customer-specific claim rules, use the[customer workflow rules testing template](https://hamming.ai/resources/voice-agent-customer-workflow-rules-testing-template) . One insurer may require escalation for glass claims above one threshold; another may route them automatically. The test should prove the active rule version, not a generic industry assumption.


## Evidence Packet Template


A claims-intake run should leave enough evidence for QA, claims operations, compliance, and engineering to agree on what happened.


```text
{        "  run_id  "  :     "  fnol_run_2026_06_22_014  "  ,        "  call_id  "  :     "  call_fixture_883  "  ,        "  agent_version  "  :     "  claims-intake-agent-v17  "  ,        "  claim_fixture  "  :     {          "  line_of_business  "  :     "  personal_auto  "  ,          "  policy_state  "  :     "  verified  "  ,          "  caller_role  "  :     "  policyholder  "        }  ,        "  expected_state  "  :     "  open_claim_or_escalated  "  ,        "  actual_state  "  :     "  open_claim  "  ,        "  required_fields  "  :     {          "  policy_number  "  :     "  present  "  ,          "  loss_date  "  :     "  present  "  ,          "  reporter  "  :     "  present  "  ,          "  contact_method  "  :     "  present  "  ,          "  incident_summary  "  :     "  present  "        }  ,        "  tool_trace  "  :     [          {            "  tool  "  :     "  lookup_policy  "  ,            "  status  "  :     "  verified  "          }  ,          {            "  tool  "  :     "  create_draft_claim  "  ,            "  status  "  :     "  created  "  ,            "  fixture_claim_id  "  :     "  claim_fixture_991  "          }  ,          {            "  tool  "  :     "  submit_claim  "  ,            "  status  "  :     "  submitted  "          }        ]  ,        "  invariant_results  "  :     {          "  no_future_loss_date  "  :     "  pass  "  ,          "  no_coverage_promise  "  :     "  pass  "  ,          "  duplicate_check  "  :     "  pass  "  ,          "  pii_redaction  "  :     "  pass  "        }  ,        "  review_route  "  :     "  claims_ops_sample  "  ,        "  cleanup_status  "  :     "  verified  "    }
```


The packet should not expose raw PII in dashboards or alert payloads. Keep raw evidence under the right access controls, then export redacted fields for QA and engineering. For broader evidence handoff, use the[voice agent call evidence export runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) .


The[NAIC model regulation on unfair claims settlement practices](https://content.naic.org/sites/default/files/model-law-902.pdf) emphasizes claim-file documentation and retrievable claim data in its model language. Treat that as a useful reminder: a claims-intake agent should create reviewable evidence, not just a nice conversation.


## What Belongs in CI?


Claims tests get expensive quickly. Keep the blocking suite small and schedule the rest.


Gate Run When Size Blocks Merge?


Field extraction invariants Prompt, ASR, entity extraction, schema, or tool changes 20-80 cases Yes


Mocked claims workflow Tool wrapper or orchestration changes 5-15 cases Yes


Sandbox FNOL flow Claim write, assignment, escalation, or document-request changes 3-8 fixture cases Yes for critical flows


Repeated stochastic scenarios Model, prompt, or policy changes 20-50 runs per critical scenario Yes for severe risk paths


Phone-path tests Telephony, transfer, audio, or interruption changes 2-5 calls Usually pre-release


Production review sampling Live monitoring 1-5% of eligible calls or top failure clusters No, alert and triage


When a production call fails, convert it into a replayable regression with the[failed production call regression test runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) . For daily operations, use[production call review triage](https://hamming.ai/resources/voice-agent-production-call-review-triage) to avoid reviewing thousands of calls when only 20 contain useful learning.


## Flaws But Not Dealbreakers


**State rules vary by insurer and jurisdiction.** This runbook gives a testing structure, not a universal claims policy. Compliance and claims operations should own the obligations.


**Synthetic claims can overfit.** If every fixture is clean, the agent will look better than it is. Add messy caller phrasing, missing data, background noise, conflicting facts, and interrupted calls.


**LLM judges are not claim adjusters.** Use them to aggregate semantic signals: missing facts, unsafe promises, escalation cues, and empathy quality. Do not let a judge decide coverage, liability, or payment.


**The system of record is the source of truth.** A transcript is evidence. The claim object, policy lookup, validation result, and review route prove whether intake actually worked.


## Claims Intake Launch Checklist


- Every claim path has a fixture: clean, missing data, conflict, duplicate, high-risk, tool failure, and escalation.
- Required fields are separated from optional fields.
- Draft vs open claim state is asserted explicitly.
- Every claim write uses an idempotency key or duplicate guard.
- Coverage, liability, settlement, and payment promises are prohibited unless your approved policy allows them.
- High-risk triggers route to a human with an evidence packet.
- PII, medical, financial, and address fields follow the approved retention and redaction policy.
- Failed production claims become regression tests before the next prompt or model change.


For regulated language, pair this checklist with[regulatory script adherence testing](https://hamming.ai/resources/regulatory-script-adherence-voice-agents) . Claims intake is not only a data-capture problem; it is also a disclosure, evidence, and escalation problem.


## Insurance Claims Intake Testing FAQ


### What is insurance claims intake voice agent testing?


Insurance claims intake voice agent testing verifies that an AI voice agent can collect first notice of loss facts, validate required fields, route exceptions, and preserve evidence for review. The test should check the final claim state and evidence packet, not only the transcript.


### How do I test non-deterministic claims paths?


Run repeated scenario variants for caller phrasing, missing details, interruptions, and contradictions, then assert deterministic invariants such as required fields, no coverage promise, duplicate protection, and correct escalation. Hamming recommends measuring state outcomes across many valid paths instead of requiring one fixed script.


### What should a claims intake test matrix include?


A claims intake test matrix should include fixture setup, caller goal, expected claim state, forbidden outcome, required fields, escalation trigger, tool trace, and cleanup evidence. The most useful rows cover clean FNOL, missing policy number, date conflict, injury or safety signal, duplicate claim, fraud signal, and tool failure.


### Should claims intake tests create real claims?


Most CI tests should not create production claims. Use mocks for fast deterministic checks, sandbox claims environments for fixture-backed workflow tests, and tightly scoped live checks only when a release owner approves the risk.


### What evidence should a passing FNOL test store?


A passing FNOL test should store run ID, call ID, agent version, policy lookup result, extracted required fields, claim state, tool trace, invariant results, review route, and cleanup status. Sensitive fields should be redacted in broad dashboards while raw evidence stays under the right access controls.


### Which claims intake failures should block release?


Block release when the agent submits incomplete claims, creates duplicates, promises coverage or payment without authority, misses injury or fraud escalation, leaks sensitive data, or claims success after a tool error. Hamming treats these as state and evidence failures, not wording issues.


### How does this differ from generic voice agent workflow testing?


Generic workflow testing checks that an agent followed the right branch and tool sequence. Claims intake testing adds insurance-specific state, required fields, documentation, escalation, duplicate protection, and audit evidence.
