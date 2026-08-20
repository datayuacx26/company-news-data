---
schema_version: "1.0.0"
document_id: "d54a184b52efe961b3d5bf0affa7a3f054a1bc2684bfce372f5da4b8dc3a9e2b"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/long-call-voice-agent-testing-runbook"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-22T10:23:57.949033+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:fa87dc0ddda82e468215c5602f996ab5ef254bd5a8695122bb8ae535f3f2c26d"
---

# Long-Call Voice Agent Testing: How to Test 70+ Conversation Turns

**Long-call voice agent testing** verifies that one stateful conversation stays correct through 70 or more turns. The test should catch forgotten facts, duplicated questions, state corruption, late tool errors, growing latency, failed interruptions, and an incorrect final outcome.


> **Definition:** Long-call voice agent testing is an endurance test for conversational state. It verifies that facts, corrections, workflow state, tool behavior, audio timing, and final outcomes remain consistent as one call accumulates 70 or more turns.


Short FAQ agent with calls that end in six turns? You probably do not need this runbook. A normal regression suite plus production monitoring is enough.


This is for clinical intake, interviews, collections, complex troubleshooting, claims, sales qualification, and other workflows where a caller may still be talking 30 minutes later. A 10-turn demo cannot prove that those calls work.


> **TL;DR:** Build a 70-turn scenario with checkpoints at turns 7, 21, 37, 53, and 70. At each checkpoint, assert retained facts, workflow state, tool results, response latency, interruption recovery, and evidence completeness. Then rerun semantic variants so the test protects behavior instead of one memorized transcript.


> **Methodology Note:** This runbook is based on Hamming's analysis of production voice agent calls and testing workflows across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> The checkpoint positions are a starter template, not universal benchmarks. Adjust them to the normal duration, risk, and task structure of your own calls.


**Last Updated:** July 2026


**Related Guides:**


- [Voice Agent Load Testing](https://hamming.ai/resources/voice-agent-load-testing-guide) - test simultaneous calls and infrastructure capacity
- [Conversational Flow Measurement](https://hamming.ai/resources/conversational-flow-measurement-voice-agents) - score repetition, silence, interruptions, and context retention
- [Voice Agent Tests as Code](https://hamming.ai/resources/voice-agent-tests-as-code-template) - keep scenarios and assertions reviewable in Git
- [Voice Agent Workflow Testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - assert tool order, state transitions, and side effects
- [OpenTelemetry for Voice Agents](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) - trace turn-level latency and tool execution
- [Voice Agent Interruption Handling](https://hamming.ai/resources/voice-agent-interruption-handling-runbook) - test barge-in and recovery behavior
- [Call Evidence Export Runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - preserve reviewer-safe audio, transcript, trace, and tool evidence
- [Voice Agent CI/CD Testing](https://hamming.ai/resources/voice-agent-testing-ci-cd-regression-load-security) - decide which regressions block a release


## What Does a 70-Turn Voice Agent Test Prove?


A 70-turn voice agent test proves that behavior remains correct as conversation state accumulates. Connection duration alone proves almost nothing. The call can stay open while the agent forgets an address, repeats an authentication question, overwrites a preference, or reports success after a tool failed.


[Microsoft's multi-turn evaluation guidance](https://learn.microsoft.com/en-us/microsoft-365/copilot/extensibility/evaluation-multi-turn) separates single-turn checks from complete conversations because later turns depend on earlier information. It calls out knowledge retention, role adherence, conversation relevancy, context consistency, and task completeness. Those are the right starting categories, but voice adds audio timing, interruptions, and live tool state.


We used to treat long calls as a small version of load testing. That framing is incomplete. Load testing asks how many sessions survive at once. Long-call testing asks whether one session survives its own history.


Test type Primary question Typical failure


Short functional test Can the agent complete one path? wrong answer or tool call


Long-call endurance test Does state remain correct across 70+ turns? context drift, repeated questions, stale tool state


Concurrency load test Can many sessions run at once? queues, throttling, latency cliffs


Soak test Does the service remain healthy for hours? memory leaks, orphaned sessions, evidence backlog


Run all four when the workflow is both long and high-volume. Do not collapse them into one green checkmark.


## How Do You Build a 70-Turn Scenario Around Checkpoints?


Do not write 70 independent lines of dialogue. Write a stateful scenario with facts that enter, change, disappear, and return later.


The most useful long-call scenario contains:


- a stable caller goal that should survive the whole call
- facts introduced early and referenced much later
- at least one correction that invalidates an earlier fact
- tool reads and writes with observable state
- an interruption or side question that should not reset the task
- a temporary tool or audio failure followed by recovery
- a final summary whose fields can be reconciled against system state


Use this starter checkpoint matrix:


Checkpoint Scenario event What to assert Failure it exposes


Turn 7 Caller supplies identity and primary goal identity stored once; goal is correct early extraction failure


Turn 21 Caller adds a preference and side constraint both facts remain available partial context loss


Turn 37 Caller corrects an earlier detail new value replaces old value everywhere stale-state persistence


Turn 53 Tool times out, caller interrupts, agent retries no duplicate write; recovery stays in flow retry and interruption corruption


Turn 70 Agent summarizes and commits final action summary, tool state, and confirmation agree late-call reconciliation failure


Why those turns? They force the test to look between the opening and the final answer. A single assertion at turn 70 tells you the call failed, but not when it started drifting.


> **Checkpoint rule:** A long-call test should identify the first turn where expected state diverges from observed state. “Final answer wrong” is an outcome. “Corrected address reverted after the retry at turn 53” is a diagnosis.


## What Does a Long-Call Test Template Look Like?


Keep the test definition compact enough to review. Put the full generated dialogue in run evidence, not in the test file.


```text
version  :     1    id  :     long_call_reschedule_70_turns    owner  :     scheduling  -  eng    target  :        agent  :     appointment  -  agent        environment  :     staging        minimum_turns  :     70      caller  :        goal  :     reschedule     two     appointments     and     preserve     accessibility     preferences        language  :     en  -  US        audio_conditions  :          -     speakerphone          -     intermittent_office_noise      state  :        initial  :          identity_status  :     unverified          preferred_time  :     morning          accessibility_note  :     null        mutations  :          -     at_turn  :     21            set  :     {     accessibility_note  :     "  allow extra response time  "     }          -     at_turn  :     37            replace  :     {     preferred_time  :     afternoon     }      events  :        -     at_turn  :     29          type  :     topic_detour        -     at_turn  :     53          type  :     tool_timeout_then_barge_in      checkpoints  :        -     turn  :     7          assert  :     [  identity_verified  ,     goal_retained  ]        -     turn  :     21          assert  :     [  accessibility_note_retained  ]        -     turn  :     37          assert  :     [  old_preference_removed  ,     new_preference_retained  ]        -     turn  :     53          assert  :     [  retry_is_idempotent  ,     interruption_recovered  ]        -     turn  :     70          assert  :     [  task_complete  ,     final_summary_matches_state  ]      evidence  :        retain  :     [  audio  ,     transcript  ,     trace  ,     tool_calls  ,     state_snapshots  ]
```


If your team already keeps[voice agent tests as code](https://hamming.ai/resources/voice-agent-tests-as-code-template) , add this as a separate long-call suite. Do not make every smoke test wait for 70 turns.


## What Should You Assert During a Long Voice Call?


Assert state and outcomes first. Exact wording is usually the wrong contract for a nondeterministic voice agent.


Assertion family Check Evidence to retain Fail when


Context facts, corrections, constraints, unresolved questions state snapshots plus transcript spans old fact returns or required fact disappears


Workflow current step and allowed transitions state-transition log agent skips, repeats, or reopens a completed step


Tools name, arguments, order, result, idempotency tool trace and sandbox state wrong call, stale argument, duplicate write


Audio silence, overlap, barge-in, playback channel audio and turn timestamps late-turn audio path degrades


Latency per-turn and component timing trace spans by turn tail latency grows beyond the early-call baseline


Role and policy scope, required language, prohibited action assertion result with evidence span agent drifts out of role late in the call


Completion final summary and backend state final transcript, tool state, receipt spoken confirmation disagrees with the system


The distinction between transcript and system state matters most near the end. An agent can say, “Both appointments are moved,” while one tool write failed 17 turns earlier.[Workflow testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) covers the tool and side-effect contract in more depth.


For model-based assertions, grade the smallest behavior possible: whether the corrected preference is used, whether the agent recovered, whether the final summary matches the record. Broad “quality” scores hide where a long conversation broke.


## How Do You Measure Degradation Across a Long Call?


Universal thresholds are tempting and often wrong. Provider, language, workflow, and model choices change the numbers. Start by comparing the later call with its own early turns.


```text
late_turn_latency_growth     =        (  median     latency     for     turns     51  -  70     -     median     latency     for     turns     1  -  20  )        /     median     latency     for     turns     1  -  20
```


Track the same early-versus-late split for:


- tool error rate
- repeated questions
- failed context references
- silence duration
- interruption recovery
- token or context size


If early median latency is 1.37 seconds and late median latency is 2.11 seconds, growth is 54%. That is a diagnostic signal, not an automatic failure threshold. Pair it with caller-visible behavior and component traces.


[OpenTelemetry voice-agent tracing](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) lets you compare speech-to-text, model, tool, text-to-speech, and playback spans by turn. Without turn-indexed evidence, teams tend to blame “context length” when a tool retry or recording backlog is the real cause.


## Why Should You Run Variants Instead of One 70-Turn Script?


One perfectly scripted conversation can become a 20-minute unit test for the script itself. That is the long-call theater problem: the run is impressive, but its coverage is brittle.


Run at least these variants:


1. Change the wording and order of non-critical facts.
2. Move the correction earlier or later.
3. Interrupt before, during, and after a tool call.
4. Inject one temporary tool failure and one audio condition.
5. Swap a valid preference while keeping the business outcome constant.
6. End with a caller request for a complete summary.


The[AWS Nova Sonic test harness](https://aws.amazon.com/blogs/machine-learning/evaluate-your-amazon-nova-sonic-voice-agent-at-scale-no-microphone-required/) describes why speech-to-speech evaluation needs persistent bidirectional sessions, multi-turn context, and tolerance for nondeterministic responses. It also notes that long sessions may need reconnection and history replay. Test that boundary deliberately when your runtime uses it.


For systems that group evaluation turns into one session, Microsoft's Voice Live harness uses a conversation ID to process related turns sequentially. It is currently a preview feature, so treat the documentation as a useful dataset pattern rather than a universal production recommendation.


## How Do You Diagnose the First Long-Call Drift?


When a long call fails, start at the first bad checkpoint.


First bad checkpoint Likely cause Next diagnostic


Turn 7 extraction or setup inspect transcript, entity parsing, initial tool read


Turn 21 memory write or retrieval compare stored state with model context


Turn 37 correction semantics find references to the superseded value


Turn 53 retry, idempotency, or turn handling inspect tool IDs, write receipts, interruption events


Turn 70 summary or commit reconciliation compare spoken summary with final backend state


Preserve audio, transcript, trace, tool calls, and state snapshots for the failed window. The[call evidence export runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) shows how to package those artifacts without giving every reviewer broad production access.


Once diagnosed, keep the smallest failing section as a normal regression test and retain the full long-call scenario as scheduled coverage.[Voice agent response coverage](https://hamming.ai/resources/voice-agent-response-coverage) explains how production failures should expand the durable suite.


## What Can Long-Call Simulation Not Prove?


**Synthetic patience is not human patience.** A simulated caller will happily continue after five awkward recoveries. A real caller may hang up after one.


**A 70-turn pass does not prove all durations.** Session reconnection, provider limits, context compaction, and resource growth may appear later. Test the longest important production workflow, not a ceremonial turn count.


**Long tests are slow and expensive.** Keep a small blocking subset around high-risk checkpoints. Run complete variants on a schedule or before changes to memory, tools, orchestration, and realtime providers.


If you are not ready for automation, run one structured manual call with a shared state sheet. Record the five checkpoints, mark the first divergence, and save the audio plus tool evidence. That simple process is more useful than an unstructured hour-long call followed by “it sounded okay.”


## What Should Pass Before a Long-Call Release?


- Scenario reaches the longest important production workflow, not an arbitrary duration.
- Early facts, corrections, preferences, and constraints have explicit checkpoint assertions.
- At least one detour, interruption, and recoverable dependency failure occurs after turn 20.
- Tool reads and writes use sandbox records, mocks, dry-run endpoints, or allowlisted fixtures.
- Tool retries are idempotent and cannot create duplicate side effects.
- Per-turn latency and component traces can be compared from early to late call.
- Final spoken summary reconciles with backend state.
- Failed runs retain audio, transcript, trace, tool evidence, and state snapshots.
- Semantic variants change wording and event placement.
- Short blocking regressions and complete scheduled long-call runs are separated.


Hamming helps teams run voice-native scenarios, inspect long-call evidence, score stateful behavior, and turn diagnosed failures into regression coverage. The useful outcome is not a 70-turn recording. It is knowing exactly where the agent stopped being correct.
