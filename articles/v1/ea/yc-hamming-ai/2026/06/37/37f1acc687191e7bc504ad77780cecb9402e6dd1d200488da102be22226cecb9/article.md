---
schema_version: "1.0.0"
document_id: "37f1acc687191e7bc504ad77780cecb9402e6dd1d200488da102be22226cecb9"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/voice-agent-handoff-transfer-testing-runbook"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:23:16.325685+00:00"
content_hash: "sha256:197f3989811053240f5e24282afcc9bdf61f5e5e9d663239884c4a93c8ae4aae"
---

# Voice Agent Handoff and Transfer Testing Runbook

**Voice agent handoff testing** proves that an AI voice agent escalates, transfers, or hands control to the right destination with the right context, at the right time, and with evidence that the next step actually happened.


If your agent never routes a caller anywhere else, this runbook is too much. Use normal conversation tests.


If your agent can transfer to a human, route to a queue, hand off to a specialist AI agent, leave voicemail, or escalate regulated workflows, transcript-only QA is not enough. The transcript can look clean while the caller lands in the wrong queue, the receiving agent gets no summary, the transfer fails silently, or the AI escalates a case it should have resolved.


We call this **transfer theater** : the system records a transfer event, but nobody proves whether the right handoff happened.


> **TL;DR:** Test voice-agent handoffs as operational workflows:
>
>
> - **Trigger:** Did the caller, policy, or safety condition justify escalation?
> - **Destination:** Did the agent route to the right person, queue, number, SIP endpoint, or specialist agent?
> - **Context:** Did the handoff include caller identity, reason, collected fields, transcript summary, and next action?
> - **Bridge state:** Did the call connect, hold, consult, merge, fail, or fall back as expected?
> - **Receipt:** Did the receiving system or human path acknowledge the handoff?
> - **Outcome:** Did the caller avoid repeating themselves, or did the workflow fail after the transfer event?


> **Methodology Note:** This runbook is based on Hamming's analysis of production voice agent calls where escalation, queue routing, human transfer, and workflow state changed the caller outcome across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> Use it as a release-safety template for healthcare, financial services, insurance, BPO, QSR, and support workflows where a bad handoff creates customer effort or operational risk.


**Last Updated:** June 2026


**Related Guides:**


- [Voice Agent Workflow Testing Runbook](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - broader tool-call, state, and side-effect testing
- [Voice Agent Sandbox Testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) - prove tool calls and side effects without production writes
- [IVR and Voice Agent Log Correlation](https://hamming.ai/resources/ivr-voice-agent-log-correlation-runbook) - connect call IDs, routing events, transcripts, and outcomes
- [Voice Agent Call Evidence Export Runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - package evidence for review
- [Customer-Specific Workflow Rules Template](https://hamming.ai/resources/voice-agent-customer-workflow-rules-testing-template) - test account, tenant, and rule-specific routing
- [Voice Agent Tests as Code](https://hamming.ai/resources/voice-agent-tests-as-code-template) - keep routing rules and expected receipts reviewable
- [Voice Agent Production Readiness Checklist](https://hamming.ai/resources/voice-agent-production-readiness-checklist) - launch gates for critical voice workflows
- [Voice Agent Incident Response Runbook](https://hamming.ai/resources/voice-agent-incident-response-runbook) - respond when handoffs fail in production
- [Failed Production Call Regression Runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) - convert escaped handoff bugs into tests
- [DTMF Support for Voice Agent Testing](https://hamming.ai/blog/dtmf-support-voice-agent-testing) - test keypad paths before or after transfer


## What Should Handoff Testing Prove?


Handoff testing should prove that the caller moved from one responsible system or person to another without losing the reason for the call.


That means a passing test needs more than "transfer tool called." It needs trigger evidence, routing evidence, context evidence, bridge evidence, and post-transfer evidence.


Layer What it proves Sample failure


Escalation trigger The agent escalated only when a caller request, policy, risk, or workflow condition required it Agent transfers every frustrated caller even when self-service should continue


Destination The selected queue, number, SIP endpoint, human team, or specialist AI agent matches the policy Billing question goes to technical support


Context payload The receiver gets caller identity, reason, summary, collected fields, and next action Human answers but asks the caller to repeat everything


Bridge state Hold, consult, merge, cold transfer, warm transfer, no-answer, and cancel states behave correctly Caller sits on hold after destination fails


Receipt The destination acknowledges the handoff or emits a transfer result System logs "transfer requested" but not "transfer connected"


Fallback Failed transfers produce a safe next step Agent says "I transferred you" after no one answered


> **Handoff success:** the escalation reason, destination, transferred context, bridge state, receipt, and caller-facing outcome all agree. If any one is missing, the handoff is not proven.


In Hamming workflow reviews, we found that expensive handoff failures rarely look dramatic in the transcript. The agent often says the correct sentence. The failure is in the receipt, the queue, the summary, or the transfer target.


## Choose the Handoff Type Before Writing Tests


Do not write one generic "transfer test." The evidence is different for each handoff type.


Handoff type Use when Required evidence Usually blocks release?


AI-agent handoff One specialist agent should take over from another Source agent, destination agent, handoff reason, preserved history, active instructions Yes for critical workflows


Human warm transfer A human needs context before speaking with the caller Consult connection, private summary, merge/bridge result, receiving participant Yes


Human cold transfer The caller can be routed directly without consult Destination, announcement, transfer result, no-answer fallback Sometimes


Queue transfer Routing depends on skill, department, region, priority, or tenant Queue ID, routing rule version, priority, wait behavior, receipt Yes for support-critical queues


IVR or DTMF transfer Destination requires keypad navigation or phone-tree traversal Digits sent, menu branch, destination answer, timeout path Pre-release


Voicemail fallback Human path unavailable but a message should be captured Voicemail detection, message policy, callback record, follow-up owner Sometimes


No-transfer fallback Transfer is forbidden or unavailable Reason not transferred, caller explanation, safe next step Yes when policy-sensitive


[LiveKit's supervisor pattern documentation](https://docs.livekit.io/agents/logic/supervisor-pattern/) distinguishes between keeping one supervisor agent in control and handing control to another agent with different instructions or tools. That distinction matters in tests. A task delegation test is not the same as a full handoff test.


Provider transfer behavior also varies.[Retell's transfer docs](https://docs.retellai.com/build/single-multi-prompt/transfer-call) describe cold and warm transfer modes for phone calls, while[Vapi's dynamic transfer docs](https://docs.vapi.ai/calls/call-dynamic-transfers) describe server-controlled destination decisions using call context. Your test should match the specific runtime behavior you use.


## Build the Handoff Test Contract


Start with a contract. Then run the call.


```text
handoff_test_contract     =        caller     scenario        +     escalation     trigger        +     allowed     transfer     mode        +     expected     destination        +     required     context     payload        +     bridge  -  state     guardrails        +     receipt     guardrails        +     fallback     rule        +     evidence     retention
```


Contract field Required? Sample


Scenario ID Yes` support_escalation_billing_014


`


Caller goal Yes Caller asks about a disputed invoice and requests a person


Trigger condition Yes Billing dispute over threshold plus explicit human request


Transfer mode Yes Warm transfer to billing queue


Destination Yes` queue_billing_priority_2


` or approved SIP endpoint


Context payload Yes Caller ID, account fixture, issue category, amount, summary, next action


Forbidden actions Yes No transfer to general support; no account update before human review


Bridge guardrail Yes Consult call established, then customer bridged, or fallback fires


Receipt guardrail Yes Transfer event, receiving participant, queue/task ID, or destination ack


Fallback rule Yes If no answer in 30 seconds, offer callback or case creation


Evidence Yes Transcript, routing rule, tool trace, transfer events, call IDs, final state


Keep this next to your[tests-as-code definitions](https://hamming.ai/resources/voice-agent-tests-as-code-template) . A teammate should be able to review the escalation policy without listening to the whole call.


### Copyable Test Definition


```text
suite  :     voice_agent_handoff_testing    owner  :     voice  -  platform    environment  :     staging      scenarios  :        -     id  :     support_escalation_billing_014          caller_goal  :     "  Dispute an invoice and ask for a person  "          trigger  :            explicit_human_request  :     true            issue_category  :     billing            amount_disputed_cents  :     18420          expected_handoff  :            mode  :     warm_transfer            destination_type  :     queue            destination_id  :     queue_billing_priority_2            context_required  :              -     caller_id              -     verified_account_id              -     issue_category              -     dispute_amount              -     summary              -     next_action          forbidden  :            -     transfer_to_general_support            -     account_credit_before_human_review            -     caller_repeats_identity_after_bridge          fallback  :            no_answer_after_seconds  :     30            expected_action  :     offer_callback_or_case_creation          evidence  :            retain_transcript  :     true            retain_transfer_events  :     true            retain_routing_rule_version  :     true            retain_receipt  :     true
```


That is more useful than a 1-10 transcript score. It tells you what the agent was allowed to do, where the caller should land, and what proof should exist afterward.


## Test Escalation Triggers and Forbidden Transfers


Most handoff bugs start before the transfer. The agent either escalates too late, escalates too early, or escalates for the wrong reason.


Trigger class Should transfer when Should not transfer when Test guardrail


Explicit caller request Caller says "representative," "human," or "transfer me" and policy allows it Caller asks a normal question with no escalation request Transfer reason includes explicit request


Safety or compliance Medical, legal, payment, account, or emergency policy requires human review Low-risk FAQ or read-only status question Policy rule version is recorded


Tool failure Required lookup, booking, payment, or CRM tool fails after allowed retry Non-critical tool times out but fallback answer is safe Agent explains failure and follows approved route


Negative sentiment Caller frustration passes threshold and self-service is failing Caller uses mild frustration but task is progressing Sentiment is supporting evidence, not sole trigger


Unsupported intent Caller asks for something outside scope Agent can answer safely or gather missing info Unsupported scope maps to correct fallback


VIP or account rule Tenant/customer rule requires special queue Generic account has no special routing Rule fixture and destination match


The negative cases matter. A voice agent that transfers too easily can destroy containment and create unnecessary queue load. A voice agent that refuses to transfer when policy requires it creates a worse problem.


For customer-specific routing, pair this with the[customer workflow rules template](https://hamming.ai/resources/voice-agent-customer-workflow-rules-testing-template) . The same caller phrase can require different destinations depending on tenant, region, priority, or regulated workflow.


## Verify Destination, Context, and Bridge State


Transfer tests should inspect the receiving side, not just the sending side.


[Twilio's warm transfer documentation](https://www.twilio.com/docs/flex/end-user-guide/warm-transfer) describes a consult pattern where the initiating agent talks to the receiving agent before the customer is bridged.[Twilio Conference](https://www.twilio.com/docs/voice/conference) also exposes participant states such as waiting, hold, mute, join, and leave. Those states are testable evidence.


Evidence What to check Fail when


Destination Queue, number, SIP URI, assistant, or agent ID Destination is missing, stale, or not policy-approved


Caller context Verified caller ID, account, preferred language, reason, collected fields Receiver gets no usable summary


Transfer mode Cold, warm, queue, AI-agent handoff, voicemail fallback Mode differs from policy


Hold and consult Caller hold state, receiving participant answer, private context delivery Caller hears private summary or waits after failure


Bridge or merge Both parties connected, original agent leaves or stays as designed Transfer event fires but no bridge occurs


Receipt Transfer update, conference participant, queue task, destination ack Only request event exists


Post-transfer outcome Caller continues with preserved context Caller repeats identity, reason, and details


> **Transfer receipt rule:** a handoff test should fail unless the destination emits an acknowledgment or bridge state that proves the caller had somewhere to go.


Some providers expose this as a webhook event.[Vapi server events](https://docs.vapi.ai/server-url/events) include transfer-related event types such as transfer destination requests and transfer updates. Other stacks require joining telephony logs, queue events, or conference participant state. The data source can vary. The requirement does not.


## Save an Evidence Envelope for Every Handoff


When a transfer fails in production, the hardest question is usually basic: where did the caller go?


Use an evidence envelope that connects the transcript, routing rule, transfer event, destination, and final state.


```text
{        "  run_id  "  :     "  handoff_run_2026_06_24_0019  "  ,        "  call_id  "  :     "  call_fixture_442  "  ,        "  scenario_id  "  :     "  support_escalation_billing_014  "  ,        "  agent_version  "  :     "  support-agent-pr-913  "  ,        "  escalation  "  :     {          "  trigger  "  :     "  explicit_human_request  "  ,          "  policy_rule_version  "  :     "  routing_rules_2026_06_24  "  ,          "  reason  "  :     "  billing_dispute  "        }  ,        "  handoff  "  :     {          "  mode  "  :     "  warm_transfer  "  ,          "  expected_destination  "  :     "  queue_billing_priority_2  "  ,          "  actual_destination  "  :     "  queue_billing_priority_2  "  ,          "  context_fields_sent  "  :     [            "  caller_id  "  ,            "  verified_account_id  "  ,            "  issue_category  "  ,            "  summary  "  ,            "  next_action  "          ]  ,          "  bridge_status  "  :     "  connected  "  ,          "  receipt_id  "  :     "  transfer_receipt_77  "        }  ,        "  post_transfer  "  :     {          "  caller_repeated_identity  "  :     false  ,          "  receiver_acknowledged_context  "  :     true  ,          "  fallback_used  "  :     false        }    }
```


Redact sensitive fields before this leaves your system. The reviewer does not need raw account data or the full transcript. They need enough structure to know whether the handoff matched policy.


For packaging this kind of evidence across teams, use the[call evidence export runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) . For joining telephony and IVR events to a single call, use the[IVR log correlation runbook](https://hamming.ai/resources/ivr-voice-agent-log-correlation-runbook) .


## Decide What Runs in CI Versus Pre-Release


Do not put every phone-path transfer in CI. You will create slow, flaky, expensive gates.


Block CI on deterministic logic and fixture-backed routing. Run telephony-heavy and live destination checks as pre-release or scheduled suites.


Gate Run when Recommended size Blocks merge?


Routing policy tests Prompt, policy, tenant rule, or escalation logic changes 8-20 fixture cases Yes


Context payload tests Summary, extracted fields, or destination contract changes 5-12 cases Yes


Mocked transfer tool tests Tool schema, function name, destination argument, or fallback changes 5-15 cases Yes


Sandbox queue tests Queue/task integration or CRM routing changes 3-8 cases Yes for critical workflows


Phone-path warm transfer tests Telephony provider, SIP, queue, IVR, or human bridge path changes 2-5 calls Pre-release


Live scoped transfer checks Production-only routing or provider behavior 1-3 allowlisted runs Release owner decision


Production monitoring Continuous transfer quality tracking Sample or 100% metadata Alert, do not block CI


The rule of thumb: if a failed handoff can affect account access, healthcare decisions, payments, legal state, safety, or a high-value customer, it deserves blocking coverage somewhere. If it only checks a rarely used queue variant, schedule it and alert the owner.


When a handoff fails in production, add it to the[failed production call regression runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) . A transfer bug that escapes once should not rely on memory next time.


## Provider and Runtime Caveats


Provider docs are useful, but they are not interchangeable. Test the specific surface you deploy.


Surface Useful public behavior Test implication


[LiveKit agent handoffs](https://docs.livekit.io/agents/logic/agents-handoffs/) Agent handoffs and supervisor/task patterns can model different control-transfer shapes Test whether control truly changes hands or a supervisor remains in charge


[Vapi dynamic transfers](https://docs.vapi.ai/calls/call-dynamic-transfers) Server-controlled destination selection can use conversation and customer context Assert request payload, routing decision, control action, and transfer update


[Retell transfer tools](https://docs.retellai.com/build/single-multi-prompt/transfer-call) Phone-call transfers can be cold or warm, with human-detection and transfer settings Test phone-call-only assumptions, no-answer, human detection, and fallback


[Retell conversation-flow transfer nodes](https://docs.retellai.com/build/conversation-flow/call-transfer-node) Transfer nodes can branch on transfer failure Assert the failure edge, not just the successful transfer path


[Twilio Conference](https://www.twilio.com/docs/voice/conference) Conferences expose participant, hold, join, leave, and bridge behavior Assert participant state and conference lifecycle for warm transfers


[Twilio Flex warm transfer](https://www.twilio.com/docs/flex/end-user-guide/warm-transfer) Warm transfer includes consult, hold/unhold, bridge, and leave steps Test the consult and bridge sequence, not only final connection


This is why[workflow testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) and handoff testing are separate. Workflow tests prove the agent chose the right action. Handoff tests prove the caller landed somewhere useful after that action.


## What This Runbook Cannot Prove


Handoff testing does not prove the human resolved the issue, the queue staffing was adequate, or the downstream team had the right policy.


It proves a narrower thing: the voice agent escalated for the right reason, routed to the right destination, included the right context, handled bridge/failure states, and retained enough evidence to debug the result.


Three limitations matter:


Limitation Why it matters Practical response


Human availability changes A transfer test can pass while real queues are understaffed Separate workflow correctness from workforce planning


Provider transfer behavior differs Caller ID, SIP REFER, warm transfer, hold, and no-answer semantics vary Keep provider-specific tests and read official docs before assuming parity


Context quality is subjective A summary can include required fields but still be hard for a human to use Add receiver feedback and production monitoring after launch


The goal is not to automate judgment away. The goal is to remove the easy-to-miss failure modes before a caller finds them.


## Voice Agent Handoff Testing FAQ


### How do I test a voice agent handoff end to end?


Test the escalation trigger, destination, context payload, bridge state, transfer receipt, fallback path, and post-transfer outcome in one contract. A passing handoff test should prove where the caller went and what context arrived, not just that the agent said "I will transfer you."


### What is the difference between handoff testing and transfer testing?


Transfer testing usually checks the telephony or routing event. Handoff testing checks the full operational outcome: why the transfer happened, who received it, what context moved with it, and whether the caller could continue without starting over.


### How do I test warm transfers from a voice agent to a human?


Test warm transfers by asserting the consult call, hold state, private summary, receiving participant, bridge/merge event, and original-agent leave behavior. The test should fail if the caller hears private context, if the receiver gets no summary, or if the bridge never completes.


### What evidence should a voice agent transfer test save?


Save the run ID, call ID, agent version, escalation reason, routing rule version, transfer mode, expected destination, actual destination, context fields sent, bridge state, receipt ID, fallback result, and post-transfer outcome. Redact sensitive values, but keep enough structure to reproduce the failure.


### Should transfer tests run in CI?


Run deterministic routing, context payload, mocked transfer-tool, and fixture-backed queue tests in CI. Keep full phone-path warm transfer, IVR traversal, live scoped destination, and provider-specific checks in pre-release or scheduled suites unless the change touches that specific path.


### How do I test that a voice agent escalated to the right queue?


Seed a caller fixture, rule version, issue category, priority, and expected destination before the call. After the call, assert the queue ID, routing reason, priority, context payload, transfer receipt, and no-answer fallback instead of relying on transcript language.


### How do I test failed transfers or no-answer paths?


Create a destination fixture that rejects, times out, returns busy, or never answers. The agent should explain the failure, offer an approved fallback such as callback or case creation, and avoid claiming the caller was transferred.


### What is the most common handoff testing mistake?


The most common mistake is treating a transfer request as proof of a handoff. Hamming recommends failing the test unless the destination, context payload, bridge state, receipt, and fallback behavior are all verified.
