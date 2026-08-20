---
schema_version: "1.0.0"
document_id: "1ce10de2b3b6059e59e4b968a2cb87d9b0660abe41e68ed706ccd2c5aef1275d"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/persistent-caller-id-inbound-voice-agent-testing"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:c2dff850860747f0578ea97e9852b7155205bb5edf724c8e8005b16469f357b7"
---

# Persistent Caller ID Testing for Inbound Voice Agents

Persistent caller ID testing answers a practical question: can you run the same inbound voice agent test tomorrow and prove it hit the same route, caller fixture, workflow state, and cleanup path?


If your test depends on whichever phone number someone happened to dial from, it is not repeatable. It might still be useful for a smoke test. It is not good enough for regression testing, caller-specific workflows, or regulated launch gates.


> **Persistent caller ID testing** uses stable caller numbers, dialed numbers, route fixtures, leases, and evidence records so inbound voice-agent tests can be replayed without guessing which caller, account, queue, or workflow branch was exercised.


**Quick filter:** if your voice agent changes behavior based on` From


` ,` To


` , SIP metadata, caller history, customer tier, routing line, or prior conversation state, you need persistent caller ID tests.


> **TL;DR:** Treat phone numbers as test fixtures:
>
>
> - Reserve stable caller numbers or a managed caller pool for automated tests.
> - Bind each dialed number to an expected route, agent, workspace, and fixture.
> - Lease numbers during runs so concurrent tests cannot steal the same identity.
> - Assert` from
>
>
> ` ,` to
>
>
> ` , provider call ID, route, fixture ID, and cleanup status.
> - Keep caller ID separate from authorization. It is a lookup signal, not proof that the human is allowed to access an account.


> **Methodology Note:** This checklist is based on Hamming's analysis of production voice agent calls where inbound routing, caller identity, tool calls, and workflow state affected the test result across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> It also uses public Twilio, Vapi, Retell, and LiveKit documentation to ground provider-specific inbound-call behavior.


**Last Updated:** June 2026


**Related Guides:**


- [Voice Agent Caller Identity Testing](https://hamming.ai/resources/voice-agent-caller-identity-testing-checklist) - separate caller lookup from authorization
- [Voice Agent Workflow Testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - prove tool calls, state, side effects, and handoffs
- [WebSocket Voice Agent Testing](https://hamming.ai/resources/websocket-voice-agent-testing-guide) - test endpoint logic before phone-path complexity
- [Voice Agent Sandbox Testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) - verify side effects without production writes
- [Voice Agent Tests as Code](https://hamming.ai/resources/voice-agent-tests-as-code-template) - keep number fixtures reviewable
- [Voice Agent Production Readiness Checklist](https://hamming.ai/resources/voice-agent-production-readiness-checklist) - decide which telephony checks block launch
- [Voice Agent CI/CD Testing](https://hamming.ai/resources/voice-agent-testing-ci-cd-regression-load-security) - connect repeatable tests to release gates
- [IVR and Voice Agent Log Correlation](https://hamming.ai/resources/ivr-voice-agent-log-correlation-runbook) - join phone events, transcripts, traces, and results


## When Do You Need Persistent Caller IDs?


You need persistent caller IDs when the phone path is part of the behavior under test.


Test Goal Persistent Caller ID Needed? Why


Public FAQ agent answers the same question for everyone No The caller number should not change the answer.


Caller-specific greeting or account lookup Yes The same number should map to the same fixture record.


Multi-tenant BPO or enterprise routing Yes The dialed number and caller context determine the customer route.


Regression test for a failed inbound production call Yes The replay needs stable call identity, route, and fixture state.


WebSocket-only endpoint test No Use the endpoint contract instead of phone numbers.


SIP, IVR, DTMF, transfer, or voicemail path Usually The dialed route, provider call ID, and caller signal are evidence.


The named failure mode is the roaming-number trap: the test passes from one engineer's phone, fails from another phone, and nobody can tell whether the prompt, route, caller lookup, or telephony provider changed.


> **Inbound test identity:** the combination of caller number, dialed number, provider call ID, route, fixture record, and test run ID. A useful inbound voice-agent test records all 6 so a failure can be replayed instead of argued about.


## The Persistent Caller ID Test Contract


Start with a contract. Do not start by dialing a number and hoping the right agent answers.


```text
Persistent     inbound     test     contract     =        caller     number     fixture        +     dialed     number     fixture        +     route     expectation        +     caller  /  account     fixture        +     number     lease        +     provider     call     evidence        +     workflow     guardrails        +     cleanup     rule
```


Field Required? Sample


Test run ID Yes` inbound_identity_2026_06_23_014


`


Caller number Yes` +


15550101014


` or provider test identity alias


Dialed number Yes` +


15550990001


` assigned to staging support route


Expected route Yes support agent, billing agent, clinic scheduling agent, or tenant-specific workspace


Fixture record Yes account, patient, booking, order, or anonymous-caller fixture


Lease owner Yes CI job ID, test suite, engineer, or scheduled run


Evidence Yes provider call ID,` from


` ,` to


` , route, transcript, trace, tool events, final outcome


Cleanup Yes release lease, reset fixture, delete sandbox side effects


This belongs near your[tests-as-code definitions](https://hamming.ai/resources/voice-agent-tests-as-code-template) . The value is reviewability: a teammate should be able to see which number hits which route before the test runs.


## Allocate Test Numbers Like Infrastructure


Treat test phone numbers the way you treat test databases. Shared, undocumented numbers become flaky.


Allocation Pattern Use It When Watch Out For


One caller number per critical fixture You have account-specific or regulated flows. More numbers to manage, but failures are easier to debug.


Small caller pool with leases CI runs need parallelism. Lease collisions cause wrong-account or wrong-route failures.


One dialed number per route Agent behavior differs by line, tenant, queue, or region. Route changes must be reviewed like code.


Provider-managed test identity Real caller ID is unavailable or expensive. It may not prove the PSTN path. Mark the limitation.


Manual engineer phone Early smoke testing only. Not suitable for CI or regression gates.


Twilio's Programmable Voice docs describe inbound calls as requests to the application associated with the dialed Twilio number, with parameters such as` CallSid


` ,` From


` , and` To


` . Vapi's personalization docs show inbound calls can ask your server to choose an assistant based on the caller phone number. Retell's inbound webhook docs include` from_number


` and` to_number


` and allow dynamic variables and metadata for the call. LiveKit telephony uses inbound trunks and dispatch rules for SIP-based routing.


The provider names differ. The test discipline is the same: capture the caller signal, capture the dialed route, and assert the selected agent or workflow.


## Build a Caller Pool and Lease Table


If more than one test can run at once, add leases. Without leases, 2 tests can use the same caller identity and corrupt each other's fixture state.


Column Purpose Sample


` caller_number


` Stable` From


` value or provider test identity` +


15550101014


`


` dialed_number


` Expected` To


` value` +


15550990001


`


` fixture_id


` Record the agent should load` acct_fixture_repeat_caller_014


`


` route_name


` Expected agent, tenant, or queue` staging_billing_agent


`


` lease_owner


` Current test run or user` ci_8421


`


` lease_expires_at


` Automatic recovery from abandoned runs` 2026


-


06


-


23T18


:


45


:


00Z


`


` last_cleanup_status


` Whether previous side effects were removed` verified


`


> **Number lease:** a short-lived reservation that prevents two inbound tests from using the same caller number, dialed route, or fixture record at the same time.


Use a lease even if the provider lets you place many calls from the same number. The provider only sees telephony. Your test sees state: account records, bookings, tickets, prior call memory, and workflow side effects.


## Assert Both Caller and Dialed Number


Inbound tests fail when teams only check one side of the call.


Signal What It Proves Fail When


Caller number (` From


` ) Which fixture or caller pool entry initiated the test Unknown caller gets matched to a fixture or shared number is used without a lease


Dialed number (` To


` ) Which route, agent, tenant, line, or queue received the call Call reaches the wrong route but transcript still sounds plausible


Provider call ID Which call generated webhooks, recordings, transcripts, and status events Evidence from another call is attached to the run


Route or dispatch rule Which assistant, workspace, or SIP path handled the call Default route handles a tenant-specific test


Fixture ID Which account, booking, patient, order, or lead was loaded Caller state does not match the test case


Cleanup status Whether test data is safe for replay Stale state makes the next run pass or fail incorrectly


This is where[caller identity testing](https://hamming.ai/resources/voice-agent-caller-identity-testing-checklist) and persistent caller ID testing meet. Caller identity testing asks whether the agent trusted the right evidence. Persistent caller ID testing makes that evidence repeatable.


## Provider-Specific Checks


Use provider docs for the specific fields, then normalize them into one evidence envelope.


Provider Surface Public Behavior to Test Inbound Test Guardrail


[Twilio Voice webhooks](https://www.twilio.com/docs/usage/webhooks/voice-webhooks) and[TwiML](https://www.twilio.com/docs/voice/twiml) Inbound calls to a Twilio number invoke your app and include call parameters such as` CallSid


` ,` From


` , and` To


` . Store` CallSid


` , normalized` From


` , normalized` To


` , route, and request-verification result.


[Vapi personalization](https://docs.vapi.ai/assistants/personalization) Your server can identify the caller by phone number and return dynamic variables or assistant configuration. Test matched, unknown, duplicate, and timeout callers against the same number fixture.


[Vapi server events](https://docs.vapi.ai/server-url/events.mdx) Inbound` assistant


-


request


` responses can choose an assistant, transient assistant, transfer destination, or error. Assert fallback or transfer behavior when lookup fails inside the provider response window.


[Retell inbound webhook](https://docs.retellai.com/features/inbound-call-webhook) Inbound webhooks include` from_number


` and` to_number


` and can set dynamic variables or metadata. Assert number metadata enters the expected route and does not leak raw sensitive data into prompts.


[Retell receive calls](https://docs.retellai.com/deploy/inbound-call) Phone numbers can bind inbound agents and use inbound webhooks for per-call context. Test agent binding, webhook override, and concurrency fallback for the dialed number.


[LiveKit inbound trunks](https://docs.livekit.io/telephony/accepting-calls/inbound-trunk/) and[dispatch rules](https://docs.livekit.io/telephony/accepting-calls/dispatch-rule.md) SIP trunks and dispatch rules route inbound calls into LiveKit. Assert trunk, dispatch rule, room/session metadata, and SIP participant evidence.


Do not hide provider differences behind vague "call metadata." Normalize the evidence after capture, not before. A missing` From


` value, anonymous caller, SIP header mismatch, or route fallback should be visible in the failed test.


## What Should the Evidence Envelope Store?


Keep the evidence small and useful. Redact raw PII when possible.


```text
{        "  test_run_id  "  :     "  inbound_identity_2026_06_23_014  "  ,        "  provider  "  :     "  twilio  "  ,        "  provider_call_id  "  :     "  CA_redacted  "  ,        "  caller_number_hash  "  :     "  sha256:caller_fixture_014  "  ,        "  dialed_number_alias  "  :     "  staging_billing_line  "  ,        "  route_name  "  :     "  staging_billing_agent  "  ,        "  fixture_id  "  :     "  acct_fixture_repeat_caller_014  "  ,        "  lease_owner  "  :     "  ci_8421  "  ,        "  guardrails  "  :     {          "  caller_number_matched_fixture  "  :     true  ,          "  dialed_number_matched_route  "  :     true  ,          "  agent_context_matched_fixture  "  :     true  ,          "  cleanup_verified  "  :     true        }    }
```


The envelope does not need the raw phone number in every system. It does need enough structure to debug a bad route, wrong fixture, duplicate lease, or stale cleanup state.


For trace correlation, connect this envelope to your[OpenTelemetry voice-agent spans](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) and[IVR log correlation](https://hamming.ai/resources/ivr-voice-agent-log-correlation-runbook) . The test should let an engineer jump from the test run to the provider call, transcript, tool trace, route decision, and fixture cleanup.


## What Belongs in CI?


Put deterministic inbound identity checks in CI. Keep expensive provider-live tests narrow.


Gate Run When Recommended Size Blocks Merge?


Number-fixture unit tests Route, tenant, or fixture mapping changes 10-30 rows Yes


Caller-pool lease tests CI runner, scheduler, or test harness changes 5-10 lease cases Yes


Provider webhook contract tests Provider config or webhook code changes 3-5 payload fixtures per provider Yes


Live inbound phone smoke tests Telephony, SIP, routing, or assistant-selection changes 2-5 calls Usually pre-release


Nightly replay suite High-risk workflows with caller-specific behavior 10-25 calls Alert, then decide


Production sampling After launch 1-5% of eligible calls No, but alert on mismatch


The[voice agent CI/CD testing guide](https://hamming.ai/resources/voice-agent-testing-ci-cd-regression-load-security) covers broader release gates. The inbound-specific rule is simple: if you cannot prove which caller and route the test used, do not let that test block a pull request.


## Troubleshoot Flaky Inbound Phone Tests


Classify the failure before changing prompts.


Symptom Likely Layer First Diagnostic Fix


Test reaches wrong agent Dialed number, route, or dispatch rule Compare` To


` , route name, and provider config. Pin dialed number to route and add route guardrail.


Caller gets wrong account state Caller pool or fixture lookup Compare` From


` , fixture ID, and lookup result. Lease caller number and reset fixture before run.


Test passes locally but fails in CI Shared number or race Check concurrent runs using the same caller. Add number leases and per-run idempotency keys.


Unknown caller gets personalized context Fallback policy Replay anonymous and unknown caller fixtures. Require generic path or step-up verification.


Provider call evidence mismatches transcript Correlation Compare provider call ID, transcript ID, and test run ID. Attach run ID at call creation and webhook ingestion.


Cleanup fails silently Fixture hygiene Query fixture state after cleanup. Fail the run when cleanup cannot be verified.


Caller ID is anonymous or raw Provider normalization Inspect raw provider request. Treat as anonymous and test the fallback route.


The fastest fix is often not a prompt change. It is a fixture change: reserve the number, bind the route, reset the account, and record the evidence.


## Privacy and Security Guardrails


Persistent caller IDs can make tests reliable, but they can also make logs riskier if teams store raw phone numbers everywhere.


Use these guardrails:


- Hash or alias caller numbers in test reports unless raw numbers are approved for that system.
- Keep raw phone numbers in the telephony system of record or approved secrets store.
- Never treat caller ID alone as authorization for sensitive actions.
- Redact provider request bodies before attaching them to tickets or PRs.
- Separate "matched fixture" from "verified caller" in test state.
- Rotate or retire test numbers when they are exposed outside approved systems.


For security-sensitive launches, pair this checklist with the[voice agent security review questions](https://hamming.ai/resources/voice-agent-security-review-questions) . For vendor evaluations, add one request to your[voice testing vendor questions](https://hamming.ai/resources/questions-to-ask-voice-testing-vendors) : "Show the same inbound scenario with a matched caller, unknown caller, duplicate fixture, anonymous caller, and route mismatch. Then show the evidence envelope for each run."


## What This Checklist Cannot Prove


Persistent caller ID testing proves repeatability. It does not prove the caller is the right human.


Limitation Why It Matters Practical Response


Caller ID can be spoofed, shared, or forwarded A stable` From


` value is not identity proof. Use it as a lookup signal and require step-up verification for sensitive flows.


Provider fields differ Twilio, Vapi, Retell, LiveKit, and SIP paths expose different metadata. Normalize after capture and keep provider-specific raw evidence available.


Test numbers can leak A public or reused test number may receive unrelated calls. Keep leases, allowlists, and route guards in place.


Sandboxes drift The route can pass in staging and fail in production because provider config differs. Run narrow pre-release live checks for launch-critical routes.


We used to treat inbound phone tests as "dial the number and grade the transcript." That is too loose. The better standard is: prove the caller signal, dialed route, fixture state, workflow result, and cleanup. Then the transcript has context.


## Minimum Production-Ready Checklist


- Every automated inbound test has a reserved caller number or managed caller-pool entry.
- Every dialed number maps to an expected route, agent, workspace, tenant, or queue.
- Number leases prevent concurrent runs from using the same caller fixture.
- The run stores provider call ID, caller signal, dialed signal, route, fixture ID, transcript, trace, tool evidence, and cleanup status.
- Unknown, anonymous, duplicate, stale, and route-mismatch callers are tested.
- Caller ID is not treated as authorization for sensitive data or account actions.
- Raw phone numbers are hashed, aliased, or stored only in approved systems.
- Cleanup is verified before the caller number returns to the pool.
- Failed production inbound calls can be converted into repeatable fixtures within 1 business day.


Persistent caller IDs are not glamorous. They are plumbing. But this plumbing is what turns inbound voice-agent testing from a manual demo into a regression suite engineers can trust.
