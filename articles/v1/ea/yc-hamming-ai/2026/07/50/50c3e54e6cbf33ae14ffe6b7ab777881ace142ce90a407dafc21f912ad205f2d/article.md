---
schema_version: "1.0.0"
document_id: "50c3e54e6cbf33ae14ffe6b7ab777881ace142ce90a407dafc21f912ad205f2d"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/voice-agent-tool-call-contract-testing-template"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:35080bcbf12b953836767a2c7d7a524114e03c28175484b20ea73e089637c073"
---

# Voice Agent Tool Call Contract Testing Template

**Voice agent tool call testing** should start with a contract, not a transcript.


A tool call is where a voice agent stops only talking and starts touching systems: calendars, CRMs, ticket queues, payment processors, identity services, order systems, message senders, transfer targets, or internal APIs. If the contract is loose, the call can sound correct while the backend state is wrong.


> **Tool-call contract:** a reviewable test definition that says which tool may run, under which preconditions, with which arguments, what result means success, what side effects are allowed, how retries are deduped, and what evidence must be saved.


Use this template when a voice agent can create, update, route, transfer, send, cancel, refund, or schedule anything. If your agent only answers static FAQs, a lighter conversation test is usually enough.


> **TL;DR:** A passing voice-agent tool test should prove 4 things:
>
>
> - The agent called the right tool at the right moment.
> - The arguments were valid and supported by caller context.
> - The downstream system ended in the expected state.
> - Retries, timeouts, and cleanup could not create duplicate or unsafe side effects.


> **Methodology Note:** This template is based on Hamming's analysis of production voice agent calls where tool calls, workflow state, backend writes, and handoff behavior affected the caller experience across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> Treat the template as a release-safety contract. Regulated workflows, payments, account access, healthcare data, and financial changes need stricter approval than low-risk lookup tools.


**Last Updated:** July 2026


**Related Guides:**


- [Voice Agent Workflow Testing Runbook](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - broader state-transition and side-effect testing
- [Voice Agent Sandbox Testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) - run tool calls safely against fixtures and sandboxes
- [Voice Agent Structured Output Validation](https://hamming.ai/resources/voice-agent-structured-output-validation-checklist) - prove fields and tool arguments match caller evidence
- [Voice Agent Tests as Code](https://hamming.ai/resources/voice-agent-tests-as-code-template) - store test contracts in Git
- [Voice Agent Call Evidence Export](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - package transcript, trace, and tool evidence for review
- [Voice Agent Transcript Search Schema](https://hamming.ai/resources/voice-agent-transcript-search-schema) - make call evidence searchable
- [Voice Agent Caller Identity Testing](https://hamming.ai/resources/voice-agent-caller-identity-testing-checklist) - decide which caller identity fields are trusted
- [Voice Agent CI/CD Testing](https://hamming.ai/resources/voice-agent-testing-ci-cd-regression-load-security) - connect blocking tests to release gates
- [OpenTelemetry for Voice Agents](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) - trace ASR, LLM, tool, and TTS steps together
- [IVR Log Correlation Runbook](https://hamming.ai/resources/ivr-voice-agent-log-correlation-runbook) - connect call IDs, routing events, and tool traces


## Schema-Valid Is Not Contract-Valid


Tool schemas matter. They are just not the whole test.


[OpenAI's function-calling documentation](https://developers.openai.com/api/docs/guides/function-calling) describes function tools as tools defined by JSON schema that connect a model to data and actions in your application.[Anthropic's tool-use documentation](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools) describes tool definitions with a name, description, and input schema.[Google Cloud's function-calling reference](https://cloud.google.com/vertex-ai/generative-ai/docs/model-reference/function-calling) describes function declarations that let the model generate structured function names and arguments, while the application invokes the real external system.


That last distinction is the testing problem. The model suggests or emits a tool call. Your application decides what happens next.


Test Layer What It Proves What It Does Not Prove


JSON parses The tool payload can be read. Required fields, enums, and business rules are correct.


Schema validates The payload shape matches the declared schema. The values came from the caller or trusted context.


Tool request is allowed The agent chose an approved tool under known preconditions. The downstream system changed correctly.


Tool result is handled The agent saw success, failure, timeout, or partial state. The spoken response matches durable state.


Side effect is verified The calendar event, CRM update, ticket, transfer, or message exists as expected. The test is replay-safe or production-safe.


> **Contract-valid tool call:** the tool name, arguments, ordering, result handling, side effect, idempotency behavior, and saved evidence all match the test contract.


For workflow agents, schema validity is the starting line. Contract validity is the finish line.


## Copyable Tool-Call Contract


Use this as a starting YAML shape. Keep it short enough for code review.


```text
version  :     1    id  :     appointment_booking_create_event_contract    owner  :     voice  -  platform    risk  :     blocking      agent_ref  :        environment  :     staging        agent_slug  :     scheduling  -  agent        prompt_version  :     pr  -  482      tool_under_test  :        name  :     create_calendar_event        schema_version  :     calendar_event_v3        dependency_mode  :     sandbox        allowed_call_paths  :          -     inbound_phone          -     web_voice      preconditions  :        caller_identity  :          source  :     verified_phone_match          fixture_id  :     caller_fixture_104        workflow_state  :          required_state  :     slot_offered          forbidden_states  :            -     caller_unknown            -     payment_pending        upstream_evidence  :          required_tools  :            -     lookup_caller_identity            -     check_availability          required_caller_confirmation  :     true      argument_policy  :        required_fields  :          -     caller_id          -     slot_id          -     timezone          -     idempotency_key        field_sources  :          caller_id  :     trusted_identity_lookup          slot_id  :     check_availability_result          timezone  :     caller_profile_or_confirmed_locale          idempotency_key  :     run_id     +     tool_name     +     caller_id     +     slot_id        forbidden_fields  :          -     raw_phone_number          -     unredacted_payment_data          -     production_calendar_id      side_effect_policy  :        allowed_side_effects  :          -     one_sandbox_calendar_event_created        forbidden_side_effects  :          -     production_calendar_write          -     duplicate_event          -     live_sms_send        exactly_once_required  :     true        cleanup  :          method  :     delete_by_run_id          verify_zero_records_after_cleanup  :     true      expected_result  :        tool_status  :     created        durable_state  :          calendar_event_count_for_run  :     1          attendee_matches_fixture  :     true          timezone_matches_request  :     true        agent_response  :          must_confirm_only_after_tool_success  :     true          must_include_local_time  :     true          must_not_claim_booking_after_timeout  :     true      evidence  :        retain  :          -     call_id          -     run_id          -     transcript          -     tool_request          -     tool_response          -     final_state_query          -     cleanup_result        redact  :          -     phone_number          -     email          -     payment_data      replay_policy  :        same_run_id_second_execution  :     no_duplicate_side_effect        timeout_retry  :     same_idempotency_key        cleanup_failure  :     fail_test
```


This belongs next to your[tests-as-code definitions](https://hamming.ai/resources/voice-agent-tests-as-code-template) . A reviewer should be able to answer 3 questions before the test runs:


1. What is the agent allowed to touch?
2. What would make the tool call unsafe?
3. What evidence proves the caller outcome and backend state agree?


## Contract Fields Explained


Treat the contract as a pre-flight checklist for one tool or one tool family.


Field Why It Exists Common Failure


` risk


` Decides whether the test blocks merge, release, or only alerts. A refund or booking tool runs as a non-blocking smoke test.


` agent_ref


` Ties the result to a prompt, branch, model, or environment. Nobody can reproduce which agent version created the side effect.


` dependency_mode


` Separates mock, sandbox, and live-scoped checks. A routine test writes to production data.


` preconditions


` Prevents tools from running before identity, consent, availability, or policy gates. Agent books while caller identity is still unknown.


` argument_policy


` Names which fields are allowed and where they must come from. Model invents a customer ID or reuses stale context.


` side_effect_policy


` Defines what the tool may mutate. Transcript passes, but CRM has the wrong owner or duplicate case.


` expected_result


` Connects tool response, durable state, and spoken confirmation. Agent says "you are booked" after a timeout or partial failure.


` evidence


` Makes failures debuggable and reviewable. Test fails with no request payload, trace, or final-state query.


` replay_policy


` Proves retry behavior before CI runs the test repeatedly. Re-running the same test creates 2 appointments or tickets.


The contract is not only for QA. It is also a product and engineering agreement. It says what a caller is allowed to accomplish, which system owns the truth, and what the agent must do when the tool path is uncertain.


## Build a Guardrail Ledger


Most tool failures are not a single event. They are a mismatch between layers.


Use a ledger so the test can show exactly where the contract broke.


Guardrail Required Evidence Fail When


Preconditions satisfied Identity result, workflow state, caller confirmation, policy flags Tool runs before the caller is eligible or verified.


Tool selected Tool name, call ID, sequence number Agent calls the wrong tool, skips a required read, or calls twice.


Arguments allowed Parsed arguments, schema result, source for each high-risk field Required field is missing, invented, stale, or copied from raw PII.


Ordering correct Tool trace and state transition Write happens before lookup, consent, availability, or confirmation.


Result interpreted Tool status, error code, timeout state, partial result Agent treats timeout, pending, or partial write as success.


Durable state verified Query against fixture, sandbox, or destination record Backend state is missing, duplicated, wrong, or uncleaned.


Caller response safe Transcript after tool result Agent confirms an action before it succeeded.


Replay safe Second run with same run ID or retry condition Duplicate side effect appears or cleanup hides the duplicate.


For high-risk calls, do not grade the transcript alone. The transcript is one row in the ledger.


Pair this with the[sandbox testing guide](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) when a real dependency needs fixture data. Pair it with the[structured output checklist](https://hamming.ai/resources/voice-agent-structured-output-validation-checklist) when arguments come from extracted caller facts.


## Idempotency Is a Required Test, Not an Implementation Detail


Voice agents retry. Callers repeat themselves. Webhooks time out. Tool handlers crash after writing but before returning a response. Phone calls reconnect. CI reruns jobs.


If the tool can create a side effect, the test should prove safe retry behavior.


Scenario Test Passing Behavior


Caller repeats the request Same caller asks to book the same slot again. Agent finds or reuses the existing hold instead of creating a duplicate.


Tool timeout after write Tool creates record but response times out. Retry uses the same idempotency key and returns the same record or safe no-op.


CI rerun Same test run ID executes twice. Final-state query still finds exactly one target record.


Cleanup fails Delete or reset endpoint returns failure. Test fails and reports residue instead of hiding it.


Partial downstream failure CRM update succeeds but SMS send fails. Agent does not summarize the whole workflow as complete.


> **Replay-safe voice-agent tool test:** a test that can run twice with the same contract and run ID without creating duplicate customer-facing state.


If a tool is not replay-safe, keep it out of merge-blocking CI until the dependency supports idempotency, fixture isolation, and verified cleanup.


## CI Gate Example


[GitHub Actions workflows](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions) are YAML-defined automated processes. Your voice-agent tests do not have to use GitHub Actions, but the same principle applies: the gate should be explicit, repeatable, and tied to the files that changed.


```text
name  :     Voice     agent     tool     contracts      on  :        pull_request  :          paths  :            -     "  agents/**  "            -     "  voice-tests/**  "            -     "  tool-schemas/**  "      jobs  :        tool  -  contracts  :          runs  -  on  :     ubuntu  -  latest          steps  :            -     uses  :     actions  /  checkout  @  v4            -     name  :     Validate     test     contracts              run  :     npm     run     validate  :  voice  -  tool  -  contracts            -     name  :     Run     blocking     mocked     tool     tests              run  :     npm     run     test  :  voice  -  tools     -  -     -  -  mode  =  mock     -  -  risk  =  blocking            -     name  :     Run     sandbox     side  -  effect     tests              run  :     npm     run     test  :  voice  -  tools     -  -     -  -  mode  =  sandbox     -  -  risk  =  blocking            -     name  :     Upload     evidence              if  :     always  (  )              run  :     npm     run     voice  -  tests  :  upload  -  evidence
```


The exact command names will differ. The important part is the split:


Gate What It Catches Blocks Merge?


Contract schema validation Missing fields, invalid risk, unknown dependency mode, unsafe forbidden fields Yes


Mocked tool tests Tool choice, argument shape, failure handling, prompt regressions Yes


Sandbox side-effect tests Real integration behavior, durable state, cleanup, idempotency Yes for critical tools


Phone-path workflow tests ASR, latency, interruption, transfer, and voice-runtime behavior Usually pre-release


Production sampling Drift after launch No, alert and convert failures into regression tests


Use the[CI/CD testing guide](https://hamming.ai/resources/voice-agent-testing-ci-cd-regression-load-security) for the broader release strategy. The tool contract is the smallest artifact that tells CI what to prove.


## Review Checklist Before a Tool Reaches Production


Use this during PR review or launch review.


Question Pass Condition


Is the tool name specific? It names the business action, not a vague helper.


Is the tool description operational? It says when to use the tool, when not to use it, and what result statuses mean.


Are high-risk arguments sourced? Caller IDs, account IDs, dates, amounts, and destinations come from trusted context or confirmed caller evidence.


Are forbidden arguments named? Raw PII, production IDs, unverified payment data, and stale context are blocked.


Is the side-effect mode explicit? Mock, sandbox, or live-scoped is declared before the call.


Is idempotency required? Creates, sends, transfers, refunds, and updates have dedupe keys or equivalent safeguards.


Does the test verify durable state? It queries the destination system, fixture, or evidence envelope after the call.


Does the agent handle failure safely? Timeout, partial success, denied action, and unavailable dependency do not become false confirmations.


Is cleanup verified? Fixture residue fails the test instead of being ignored.


Can a reviewer replay the evidence? Run ID, call ID, transcript, tool request, result, final-state query, and cleanup result are saved.


The review should feel strict. A bad tool call can create work for support teams, break compliance expectations, or tell a caller something happened when it did not.


## What This Template Cannot Prove


The contract proves that one tool path behaved under controlled conditions. It does not prove the whole voice system is safe.


Limitation Why It Matters Add This


Audio path is outside the contract ASR errors, barge-in, silence, and latency can change when tools run. Phone-path tests and trace review.


Fixtures are cleaner than production Real customer records can have missing fields, duplicates, or stale ownership. Production sampling and data-quality checks.


Provider behavior changes Tool-call and webhook runtimes evolve. Versioned schemas and scheduled regression runs.


Business policy can drift Refund, booking, escalation, and compliance rules change. Owner review and rule fixtures.


A single tool is not the whole workflow Multi-step flows can pass one tool test and fail at handoff or cleanup. Workflow tests and incident-derived regressions.


Start with the tool contract. Then connect it to[workflow testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) ,[tracing](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) , and[call evidence export](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) .


## FAQ


### What is a voice agent tool-call contract?


A voice agent tool-call contract is a test definition that specifies which tool the agent may call, the required preconditions, allowed arguments, expected result, permitted side effects, idempotency behavior, evidence retention, cleanup, and replay policy.


### How is tool-call testing different from workflow testing?


Tool-call testing focuses on one tool boundary: request, arguments, result, side effect, and retry behavior. Workflow testing covers the larger state machine across multiple tools, caller turns, handoffs, and business rules.


### Do I need this if my tool schema already validates?


Yes, if the tool can create or change customer-facing state. Schema validation proves payload shape. It does not prove the caller was eligible, the arguments came from trusted evidence, the downstream system changed once, or the agent handled failure safely.


### Which voice-agent tools should block CI?


Block CI for tools that book appointments, create tickets, update CRMs, send customer messages, initiate refunds, route calls, change account access, process regulated data, or trigger workflows that a human team depends on.


### What evidence should a tool-call test save?


Save the run ID, call ID, transcript, tool request, tool response, relevant trace, final-state query, guardrail results, and cleanup result. Redact phone numbers, email addresses, payment data, and other sensitive fields before sharing evidence outside the owning system.


### How do I test tool calls when the QA system cannot see internal traces?


Export a redacted evidence envelope. It should include the run ID, call ID, tool name, status, fixture record ID, idempotency key, final-state summary, and cleanup result without exposing raw PII or private database rows.


### Should tool-call tests run against mocks or sandboxes?


Use mocks for fast schema, prompt, ordering, and failure-path tests. Use sandboxes when the risk is in auth, provider behavior, real schemas, durable side effects, cleanup, or idempotency. Use live-scoped checks only with allowlists and release-owner approval.


### What is the most common tool-call testing mistake?


The most common mistake is accepting a clean transcript as proof of workflow success. For side-effecting tools, the test must also inspect the tool request, result, durable state, retry behavior, and cleanup evidence.
