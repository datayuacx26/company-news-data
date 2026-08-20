---
schema_version: "1.0.0"
document_id: "e7abdeabd94760e27f4a85d78d0be64314a85dc446a0ededcca6abd48ef5cecf"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/voice-agent-tests-as-code-template"
published_at: "2026-05-25T00:00:00+00:00"
first_seen_at: "2026-07-27T03:05:01.547836+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:973474a873b876ef778bc325bde60fb5daee185496f437a381feacc8da631e33"
---

# Voice Agent Tests as Code: YAML Template for CI

**Voice agent tests as code** means defining test cases in version-controlled files so prompts, personas, call paths, guardrails, and expected evidence can be reviewed before they run against production agents.


If your team changes one demo agent by hand once a month, this is probably too much process. Use the dashboard, listen to the calls, and keep moving.


This is for teams where voice-agent behavior changes through pull requests: prompt edits, tool schemas, routing rules, language support, compliance scripts, and workflow fixes. Once those changes ship through Git, the tests should live there too.


"The programmatic part of it to me is the big unique offering and advantage," says[Blake Jones, AI Engineer at Basata](https://hamming.ai/case-studies/basata) . We found that the value appears when the test definition, evidence, and agent change can be reviewed together.


> **TL;DR:** Put voice agent test cases in YAML, validate them against a schema, run the blocking subset in CI, and store the result with the same commit that changed the prompt or workflow.
>
>
> A test that only exists in a vendor dashboard can still be useful. It is just hard to review, diff, export, or connect to the code change that made it necessary.


> **Methodology Note:** The template in this guide is based on Hamming's analysis of production voice agent calls across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected.
>
>
> Treat this as a starting contract. Regulated flows, payments, and account changes need stricter approvals and side-effect controls than low-risk FAQ flows.


**Last Updated:** July 2026


**Related Guides:**


- [Voice Agent Tool Call Contract Testing Template](https://hamming.ai/resources/voice-agent-tool-call-contract-testing-template) - copy a reviewable contract for side-effecting tools into CI
- [Voice Agent Sandbox Testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) - run side-effecting tests against fixture calendars and sandboxes
- [Voice Agent Testing in CI/CD](https://hamming.ai/resources/voice-agent-testing-ci-cd-regression-load-security) - broader release-gate strategy
- [Testing Voice Agents for Production Reliability](https://hamming.ai/resources/testing-voice-agents-production-reliability) - regression and production-failure conversion
- [Voice Agent Workflow Testing Runbook](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - tool-call and side-effect guardrails
- [Voice Agent Production Readiness Checklist](https://hamming.ai/resources/voice-agent-production-readiness-checklist) - launch gates and owners
- [Voice Agent Response Coverage](https://hamming.ai/resources/voice-agent-response-coverage) - find gaps from production calls
- [Testing LiveKit Voice Agents](https://hamming.ai/resources/testing-livekit-voice-agents-complete-guide) - runtime-specific test coverage
- [Questions to Ask Voice Testing Vendors](https://hamming.ai/resources/questions-to-ask-voice-testing-vendors) - exportability and CI/CD vendor checks
- [Voice Agent Observability and Tracing](https://hamming.ai/resources/voice-agent-observability-tracing-guide) - evidence that makes failures debuggable
- [Debugging Voice Agents](https://hamming.ai/resources/debugging-voice-agents-real-time-logs-missed-intents-error-dashboards) - trace failures after the gate catches them


## What Belongs in a Voice Agent Test File


The file should be boring. A reviewer should understand what the call will do, what must pass, what is allowed to touch a real system, and what evidence will be saved.


Test field What it captures Why it belongs in Git


` id


` Stable test identifier Lets failures stay searchable across runs


` owner


` Team or person responsible Prevents orphaned tests


` agent_ref


` Agent, prompt, branch, or environment Ties the test to a deployable target


` persona


` Caller language, accent, goal, constraints Makes caller coverage reviewable


` setup


` Fixture state before the call Keeps tests reproducible


` call_path


` Inbound, outbound, WebRTC, SIP, or provider mode Prevents text-only tests from pretending to be voice tests


` guardrails


` Outcome, transcript, tool, latency, policy, and side-effect checks Defines pass/fail before the run starts


` evidence


` Audio, transcript, trace, tool result, dashboard link Makes failures debuggable


` cleanup


` How test data is removed or isolated Prevents synthetic calls from polluting production


[GitHub Actions workflows](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions) are YAML files under` .


github


/


workflows


` , and[GitLab CI](https://docs.gitlab.com/ci/yaml/) uses YAML for pipeline configuration. That does not mean voice-agent tests must use YAML forever. It means YAML is familiar enough for code review, comments, ownership, and CI wiring.


## A Copyable YAML Template


Use this as the starting shape. Keep IDs stable and names human-readable.


```text
version  :     1    suite  :     appointment_booking_smoke    owner  :     growth  -  eng    agent_ref  :        environment  :     staging        agent_slug  :     scheduling  -  agent        prompt_version  :     pr  -  482      defaults  :        call_path  :     inbound_phone        timeout_seconds  :     180        evidence  :          retain_audio  :     true          retain_transcript  :     true          retain_tool_trace  :     true          retain_days  :     30      tests  :        -     id  :     booking_reschedule_verified_caller          title  :     Reschedule     an     appointment     after     identity     check          risk  :     blocking          persona  :            language  :     en  -  US            caller_goal  :     Move     my     Tuesday     appointment     to     Friday     afternoon            speech_conditions  :              accent  :     neutral              background_noise  :     office          setup  :            fixtures  :              caller_id  :     caller_qa_104              appointment_id  :     appt_fixture_882            side_effect_mode  :     sandbox          call_script  :            -     user  :     I     need     to     move     my     appointment     from     Tuesday     to     Friday     after     2  .            -     user  :     Yes  ,     Friday     at     3     works  .          guardrails  :            outcome  :              task_completed  :     true              no_unplanned_handoff  :     true            transcript  :              must_include  :                -     Friday     at     3              must_not_include  :                -     I     cannot     access     your     account            tools  :              required_order  :                -     lookup_identity                -     list_appointments                -     hold_slot                -     update_booking                -     send_confirmation            side_effects  :              calendar  :                appointment_id  :     appt_fixture_882                expected_status  :     rescheduled                duplicate_events_allowed  :     false            latency  :              turn_p95_ms_max  :     1500          cleanup  :            delete_sandbox_records  :     true
```


This template is intentionally more specific than a generic "run test" payload. Voice-agent failures hide in the details: wrong fixture, wrong call path, missing tool trace, duplicate calendar write, or a human handoff that technically happened but carried no context.


## Field-by-Field Review Checklist


Review the test file like production code.


Review question Good answer Block the PR when


Does the test have an owner?` owner


` maps to a team or person Nobody can triage failures


Is the target clear? Environment, agent slug, and prompt version are explicit Test could run against the wrong agent


Is the caller realistic? Persona includes goal, language, and relevant speech condition Test only mirrors the happy-path script


Are guardrails layered? Outcome, transcript, tool, side-effect, and latency checks are separated A transcript check stands in for the whole workflow


Are writes safe?` side_effect_mode


` is mock, sandbox, or explicitly allowlisted CI can touch real customer systems


Is evidence retained? Audio, transcript, tool trace, and run ID are saved Failure cannot be debugged later


Is cleanup defined? Fixtures reset or expire Synthetic data leaks into later runs


The important shift is that QA changes become reviewable. A teammate can ask, "Why is this test non-blocking?" or "Why does this prompt change not add a regression case?" before the agent reaches customers.


> **Git review rule:** a voice agent test is reviewable only when the reviewer can see the caller setup, target agent, guardrails, side-effect policy, and retained evidence before the test runs. If those details live only in a dashboard, the PR cannot prove what behavior it is protecting.


## How to Import a Golden Dataset


Most teams start with a spreadsheet. That is fine. The mistake is importing the spreadsheet as loose rows with no schema.


Use an import map:


Spreadsheet column YAML field Required? Notes


` case_id


`` tests


\[


\]


.


id


` Yes Stable, lowercase, no spaces


` caller_goal


`` persona


.


caller_goal


` Yes Plain English job-to-be-done


` language


`` persona


.


language


` Yes Use locale format when possible


` fixture_customer


`` setup


.


fixtures


.


caller_id


` Conditional Required for account workflows


` opening_utterance


`` call_script


\[


0


\]


.


user


` Yes First caller turn


` expected_outcome


`` guardrails


.


outcome


` Yes Convert prose into typed checks


` must_call_tool


`` guardrails


.


tools


.


required_order


` Conditional Required for workflow tests


` max_latency_ms


`` guardrails


.


latency


.


turn_p95_ms_max


` Optional Use only when latency is part of the risk


` risk


`` tests


\[


\]


.


risk


` Yes` blocking


` ,` scheduled


` , or` manual


`


After import, validate the file.[JSON Schema](https://json-schema.org/learn/getting-started-step-by-step) is a good fit because it can require fields, constrain object shapes, and validate nested data. The point is not ceremony. The point is refusing a "golden dataset" where half the rows have no expected outcome.


## Blocking, Scheduled, and Ephemeral Tests


Not every test belongs in the same gate.


Test class When to use Storage policy Run cadence


Blocking Critical account, payment, compliance, booking, or safety flows Persist test definition and evidence Every relevant PR


Scheduled Broader regression, language, persona, and coverage suites Persist definition, retain sampled evidence Nightly or weekly


Ephemeral One-off investigation, vendor trial, support reproduction Store run metadata and result, not permanent suite entry Manual or temporary CI job


Ephemeral is a lifecycle choice, not a logging shortcut. Otherwise the team cannot tell whether it was a real test or just a dashboard click.


> **Ephemeral run rule:** temporary voice agent tests can skip permanent suite registration, but they should not skip evidence. The minimum record is the run ID, agent version, guardrail result, trace link, and cleanup status.


## How to Run Ephemeral Tests Without Permanent Vendor Storage


Ephemeral voice agent tests are useful when you need a short-lived reproduction: a vendor bakeoff, a support escalation, a preview-environment smoke test, or a one-off prompt investigation. The trap is treating "temporary" as "unreviewable." Even if the test disappears tomorrow, the team still needs enough evidence to debug the failure, prove cleanup, and decide whether the case belongs in the permanent suite.


> **Temporary test contract:** delete the throwaway test definition when the investigation ends, but keep a redacted evidence envelope in customer-controlled storage. It should answer who ran the check, which agent version ran, what failed, where the trace lives, and when the test data expires.


Split retention by artifact instead of giving the whole run one broad expiration date. The values below are example operating defaults, not vendor limits or compliance advice:


Artifact Keep for ephemeral tests? Why Delete or promote when


Test definition Usually no, unless repeated Avoids turning a one-off reproduction into suite clutter Delete after the investigation, or promote when the failure is repeatable or high-risk


Run metadata Yes Engineers need run ID, agent version, environment, and owner Start with 7-30 days, then shorten or extend to match policy and incident-response needs


Redacted transcript Yes, if text is needed to debug Lets reviewers inspect the failure without raw customer data Delete after triage, or attach to a promoted regression


Raw audio Only when voice quality matters Needed for ASR, interruption, silence, or latency debugging Prefer short retention; disable when not needed


Tool trace Yes for workflow tests Proves the agent called the right tool with safe fixture data Keep until the issue is closed


Fixture cleanup proof Yes Shows test records, calendar holds, tickets, or sandbox writes were removed Keep with the run record


Security, legal, contractual, and incident-response requirements own the final retention window. If those requirements conflict with this example, use the stricter policy.


Provider settings matter here.[ElevenLabs documents separate retention controls](https://elevenlabs.io/docs/eleven-agents/customization/privacy/retention) for conversation transcripts and audio recordings, including scheduled deletion by setting retention to 0 days.[Retell documents per-agent retention](https://docs.retellai.com/accounts/data-retention) from 1 to 730 days; after expiry it deletes recordings, transcripts, logs, retrieval logs, dynamic variables, and metadata on a daily schedule, while retaining some basic internal metadata. That is delayed payload deletion, not zero retention. If a vendor cannot separate test definitions, run results, audio, transcripts, and tool traces, treat that as a security-review question before using production-derived test material.


For Google CX Agent Studio, the[evaluation model separates scenario tests from golden tests](https://docs.cloud.google.com/customer-engagement-ai/conversational-agents/ps/evaluation) . That distinction is useful outside Google too: scenario-style checks are good for exploration, while golden tests are the cases you want to keep stable. The[batch-upload format](https://docs.cloud.google.com/customer-engagement-ai/conversational-agents/ps/evaluation-batch-upload) shows what the promoted version needs: a named evaluation row, ordered turns, action types, and explicit expectations.


Use this promotion rule:


Signal Keep ephemeral Promote to permanent tests-as-code


One-off vendor trial Yes Only if it exposes a repeatable platform limitation


Support reproduction with synthetic data Yes Promote when the same failure appears in production or staging


Preview-environment smoke test Yes Promote when it protects a launch-critical workflow


Compliance or payment path Rarely Usually promote immediately after privacy review


Repeated failing tool-call sequence No Promote with fixture data, expected tool order, and cleanup guardrail


We used to think every useful test should become a permanent test. That created noisy suites. The better rule is narrower: every useful failure needs evidence, and every repeated useful failure needs a reviewed test.


## GitHub Actions Gate


Here is a minimal GitHub Actions shape. Use your own runner and command names.


```text
name  :     voice  -  agent  -  tests      on  :        pull_request  :          paths  :            -     agents  /**         - prompts/**         - tests/voice-agents/**     jobs:     voice-agent-tests:       runs-on: ubuntu-latest       steps:         - uses: actions/checkout@v4         - uses: actions/setup-node@v4           with:             node-version: "20"         - run: npm ci         - name: Validate voice test YAML           run: npm run voice-tests:validate -- tests/voice-agents         - name: Run blocking voice tests           run: npm run voice-tests:run -- --risk=blocking --env=staging         - name: Upload voice test evidence           if: always()           run: npm run voice-tests:evidence -- --format=summary
```


The gate should fail when a blocking test fails, when a required fixture is missing, when a test tries to write outside its allowed mode, or when the evidence upload fails. A test result that nobody can inspect is not a CI gate. It is a guess with a status icon.


## Common Mistakes


Mistake Why it hurts Better pattern


Storing tests only in a vendor dashboard Prompt changes and test changes cannot be reviewed together Keep critical tests in Git and sync to the runner


Making every test blocking CI becomes slow and noisy Use blocking, scheduled, and manual risk classes


Checking only transcript text The agent can say the right thing while tools fail Add tool, side-effect, latency, and handoff guardrails


Importing spreadsheets without schema validation Rows drift into inconsistent formats Convert columns into typed YAML fields


Keeping no evidence for failed runs Developers cannot debug the failure Retain audio, transcript, trace, tool output, and cleanup status


Letting tests write to shared systems Synthetic calls pollute calendars, CRMs, and support queues Use mock or sandbox modes by default


The honest limitation: tests as code will not replace exploratory QA. Voice agents still need humans listening for weird pacing, awkward recovery, and caller frustration. But once a failure matters enough to block a release, it should graduate into a file someone can review.
