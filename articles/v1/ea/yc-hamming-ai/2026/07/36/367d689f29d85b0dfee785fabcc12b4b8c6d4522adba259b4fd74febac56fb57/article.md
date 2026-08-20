---
schema_version: "1.0.0"
document_id: "367d689f29d85b0dfee785fabcc12b4b8c6d4522adba259b4fd74febac56fb57"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/voice-agent-call-replay-qa-checklist"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-23T10:24:10.607271+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:395385bca73389100ed35827b96b2f51fc4004d2312d941c06cfa778c2ba1646"
---

# Voice Agent Call Replay

**Voice agent call replay** is the QA workflow for reviewing a call's audio, transcript, traces, tool calls, and evaluation results on one synchronized timeline. A reviewer should be able to click a transcript turn, hear the matching audio, inspect what the agent did next, and record a defensible decision without opening five tools.


> **Definition:** A voice agent call replay is a synchronized review of the recording, speaker-labeled transcript, turn timestamps, agent and tool events, and QA findings for one completed call.


If your team only needs a recording player and a plain transcript, most call platforms already provide them. This checklist is for teams that need to explain *why* a voice agent failed, decide who owns the fix, and preserve enough evidence to verify the correction.


> **TL;DR:** Keep audio, transcript, traces, and tool events on one clock. Verify call identity and permissions first, jump to the selected failure window, inspect the surrounding turns, classify the defect, and end every review with an owner and next action. Do not approve a replay workflow when timestamps drift, speakers are mislabeled, or the transcript cannot be tied to the actual recording.


> **Methodology:** This checklist synthesizes Hamming's production QA experience with the official AWS and LiveKit workflows cited below. The six-minute target, timing tolerances, 12 checks, and 20-call gate are starter policies, not benchmarks. Set stricter gates for regulated, safety-critical, or high-value workflows.


**Last Updated:** July 2026


**Related Guides:**


- [Production Call Review Triage](https://hamming.ai/resources/voice-agent-production-call-review-triage) - select which calls deserve human review
- [Call Evidence Export Runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - package audio, transcripts, traces, and evaluation evidence
- [Voice Agent Interruption Handling](https://hamming.ai/resources/voice-agent-interruption-handling-runbook) - diagnose barge-in, overlap, and recovery failures
- [Dead Air Detection Guide](https://hamming.ai/resources/voice-agent-dead-air-detection-guide) - investigate silence windows and media failures
- [Voice Agent Observability](https://hamming.ai/resources/voice-agent-observability-tracing-guide) - connect call behavior to cross-service traces
- [Failed Call Regression Tests](https://hamming.ai/resources/failed-production-call-regression-test-runbook) - turn diagnosed failures into repeatable coverage
- [Daily Failure Report Template](https://hamming.ai/resources/voice-agent-daily-failure-report-template) - summarize clusters, owners, and next actions


## What Type of Voice Agent Call Replay Tool Do You Need?


Choose the tool category from the decision you need to make, not from the quality of its transcript viewer.


Tool category Best fit What it must preserve Main tradeoff


Voice-agent QA and observability diagnosing agent behavior and turning failures into regression coverage audio, turns, agent state, tools, traces, scores, and reviewer decisions requires reliable joins across the voice stack


Native voice-platform logs debugging one agent runtime or media session platform recording, transcript, session events, and runtime identifiers evidence may stop at the platform boundary


Contact-center conversation intelligence supervisor review, coaching, compliance, and queue analytics recordings, transcripts, participants, evaluation forms, and access policy may not expose agent prompts, tool results, or model traces


Generic transcription searchable recordings and lightweight review recording, speaker labels, timestamps, and export insufficient for root-causing orchestration or tool failures


Custom replay viewer workflows with proprietary evidence or controls one canonical call ID and a versioned evidence contract the team owns synchronization, permissions, and reviewer UX


A dedicated voice-agent QA workflow is the right fit when the reviewer must connect a spoken symptom to an ASR result, model decision, tool side effect, or media event.[Hamming's session replay](https://hamming.ai/product/voice-agent-monitoring#end-to-end-observability) fits this category by keeping production call evidence connected to evaluation and debugging workflows. If the immediate problem is deciding which calls deserve review, start with[production call review triage](https://hamming.ai/resources/voice-agent-production-call-review-triage) before adding another replay surface.


## What Must Stay Synchronized During Voice Agent Call Replay?


At minimum, keep five evidence lanes aligned to the same call clock:


Evidence lane What the reviewer sees What it proves


Audio caller and agent recording with seek controls what was captured on each recorded channel, including silence, overlap, noise, and tone


Transcript speaker-labeled turns with start and end timestamps what the speech recognizer produced and which turn triggered the next action


Agent events listening, thinking, speaking, interrupted, error whether turn-taking and orchestration matched the audio


Tool and trace events tool name, arguments summary, result, retry, latency whether backend behavior matched the conversation


QA findings reason selected, rubric result, note, owner, disposition why the call matters and what happens next


[Amazon Connect documents](https://docs.aws.amazon.com/connect/latest/adminguide/turn-by-turn-transcript.html) a useful baseline: audio and transcript remain synchronized, and selecting a turn timestamp jumps to that part of the recording.[LiveKit Agent Observability](https://livekit.com/products/agent-observability) extends the same idea to voice agents with synchronized recordings, transcripts, and traces alongside session logs.


Ben Rigby, then Talkdesk's SVP of AI, Automation, and Workforce, described the operational goal as extracting["actionable conversation insights"](https://aws.amazon.com/blogs/aws/extract-insights-from-customer-conversations-with-amazon-transcribe-call-analytics/) from customer-service calls. For voice-agent QA, an insight is only actionable when the reviewer can trace it back to the matching audio and system event.


The hard part is not rendering five panels. It is keeping them on one clock. Recording time, session time, provider event time, and trace time may start at different moments. Normalize every event to` offsetMs


` from one canonical call start, while retaining the source timestamp for audit and debugging.


> **Replay clock:** the canonical call-relative timeline used to align audio samples, transcript turns, state changes, tool calls, traces, and reviewer annotations.


[LiveKit's session report reference](https://docs.livekit.io/reference/python/livekit/agents/voice/report.html) exposes recording start time, session start time, chat history, events, room identifiers, and job identifiers. Your provider may use different names, but the join requirement is the same.


## The 12-Point Voice Agent Call Replay Checklist


Use this checklist before treating a replay surface as QA-ready.


# Check Pass condition Failure signal


1 Canonical identity recording, transcript, trace, and evaluation share one internal call ID reviewer matches artifacts by caller or approximate time


2 Recording availability authorized reviewer can play the expected channel or mixed recording missing file, wrong call, silent channel, or expired URL


3 Speaker labels caller, agent, IVR, and human-transfer speakers are distinguishable turns switch speakers or merge two participants


4 Timestamp alignment clicking a turn lands on the matching spoken phrase audio leads or trails the transcript enough to confuse diagnosis


5 Turn boundaries interruptions, partial speech, and resumed turns remain visible overlap is flattened into a clean but false sequence


6 Agent state listening, thinking, speaking, and interrupted states align with playback state says “speaking” during dead air or misses an interruption


7 Tool evidence each important tool call shows trigger, result, latency, and side-effect status transcript implies success without backend evidence


8 Trace linkage reviewer can open the relevant span without searching by time call and trace cannot be joined reliably


9 Evaluation context score, rubric version, and failed criterion are visible only a red badge appears with no reason


10 Privacy controls playback, raw text, exports, and notes respect separate permissions transcript is redacted but raw audio is broadly accessible


11 Reviewer decision finding uses a shared defect category and disposition free-text note cannot be aggregated or routed


12 Next action owner, due state, and regression-test decision are recorded reviewed call disappears into a notes field


A technically playable call can still fail this checklist. If clicking “booking completed” opens the correct audio but the tool result is missing, the reviewer cannot tell whether the agent completed the task or merely said it did.


That distinction is why the replay should link directly into[voice-agent observability](https://hamming.ai/resources/voice-agent-observability-tracing-guide) , not leave the reviewer searching for a trace by approximate time.


## How Do You Review a Voice Agent Call in Six Minutes?


Do not listen from second zero unless the selection reason says the whole call matters. Start with the reason the call entered review.


1. **Verify identity and access.** Confirm call ID, agent version, environment, recording consent state, and reviewer permission.
2. **Scan the outcome.** Read the summary, evaluation result, and selection reason. Treat them as hypotheses, not truth.
3. **Jump to the failure window.** Open 15 to 30 seconds before the flagged turn so you hear the setup, not only the symptom.
4. **Replay the surrounding turns.** Listen for overlap, silence, ASR errors, repeated questions, unsafe wording, and whether the caller corrected prior information.
5. **Inspect trace and tool evidence.** Check the event that should explain the behavior: ASR output, model span, tool result, TTS start, transfer attempt, or media state.
6. **Classify and route.** Choose one primary defect, add supporting evidence, assign an owner, and decide whether the call becomes a regression test.


The six-minute target is an operating constraint, not a benchmark. Complex compliance or incident reviews should take longer. We found the replay UI stops being useful when a routine failed-tool-call review needs 20 minutes of copying IDs across tabs. The interface should carry the reviewer from the spoken symptom to the supporting system event.


> **Review-window rule:** preserve at least one complete caller-agent exchange before and after the suspected failure. A single isolated turn often hides the correction, interruption, or tool response that caused it.


## A Copyable Call Replay Packet


The replay UI should be reconstructable from a stable evidence contract. Start with this shape:


```text
{        "  callId  "  :     "  call_01K0Q8R7D4  "  ,        "  agentVersion  "  :     "  claims-intake@2026-07-22.3  "  ,        "  environment  "  :     "  production  "  ,        "  startedAt  "  :     "  2026-07-22T15:04:12.301Z  "  ,        "  recording  "  :     {          "  uri  "  :     "  controlled://recordings/call_01K0Q8R7D4  "  ,          "  offsetMs  "  :     0  ,          "  channels  "  :     [  "  caller  "  ,     "  agent  "  ]  ,          "  redactionState  "  :     "  restricted_raw  "        }  ,        "  turns  "  :     [          {            "  turnId  "  :     "  turn_018  "  ,            "  speaker  "  :     "  caller  "  ,            "  startOffsetMs  "  :     94210  ,            "  endOffsetMs  "  :     98140  ,            "  text  "  :     "  I need the Tuesday appointment, not Thursday.  "  ,            "  asrConfidence  "  :     0  .  87          }        ]  ,        "  events  "  :     [          {            "  eventId  "  :     "  evt_066  "  ,            "  type  "  :     "  tool_result  "  ,            "  startOffsetMs  "  :     101420  ,            "  turnId  "  :     "  turn_018  "  ,            "  toolCallId  "  :     "  tool_93  "  ,            "  status  "  :     "  failed  "  ,            "  traceId  "  :     "  4bf92f3577b34da6a3ce929d0e0e4736  "          }        ]  ,        "  review  "  :     {          "  selectionReason  "  :     "  agent_claimed_success_after_tool_failure  "  ,          "  primaryDefect  "  :     "  tool_result_handling  "  ,          "  disposition  "  :     "  promote_to_regression_test  "  ,          "  owner  "  :     "  scheduling-platform  "        }    }
```


Keep raw tool arguments and sensitive transcript text behind tighter permissions than the summary. The replay packet should carry pointers and redaction states, not become an uncontrolled copy of every production artifact.


## How Should Reviewers Classify Voice Agent Replay Findings?


Use a small taxonomy that maps findings to an owner and next diagnostic step.


Primary defect Replay evidence Likely owner Next action


audio transport clipping, one-way audio, packet gap, wrong channel realtime/telephony inspect media events and provider metrics


turn detection false interruption, missed barge-in, early cutoff voice runtime reproduce with the same timing pattern


ASR recognition transcript disagrees with clear recording speech/agent team add audio variant and expected semantic content


orchestration agent state or prompt path is wrong agent engineering inspect state transition and prompt version


tool execution wrong call, failed result, retry, duplicate side effect integration owner verify request, response, idempotency, and final state


policy or content unsafe, noncompliant, or incorrect response product/policy update Guardrail or knowledge and add coverage


evidence pipeline missing recording, timestamp drift, broken trace link observability/data repair joins before judging agent behavior


Do not force every call into a product defect. Sometimes the right result is` expected_behavior


` ,` insufficient_evidence


` ,` caller_environment


` , or` duplicate_cluster


` . Those dispositions prevent noisy reports from turning into invented engineering work.


## What Should Never Be Hidden by a Clean Transcript?


A clean transcript is convenient, but it can erase the reason the call felt broken. Preserve these conditions in replay:


- overlapping caller and agent speech
- partial utterances cut off by endpointing
- silence between a tool result and the next spoken response
- TTS audio that started but was never heard
- a correction that invalidated an earlier value
- repeated or retried tool calls
- transfer audio, hold music, IVR prompts, and participant changes
- low-confidence transcription or unavailable words


This is why audio remains necessary even when transcript quality is high. The transcript is an interpretation of the recording, not a replacement for it. A server-side recording still does not prove what reached a participant's device; use client or media-path telemetry when that distinction matters.


## Privacy and Sampling Boundaries


Call replay concentrates sensitive artifacts in one place. Treat that convenience as a security boundary.


- Separate permission to view redacted text, raw transcript, and raw audio.
- Log playback, export, annotation, and share actions.
- Use expiring access for recordings and traces.
- Keep reviewer notes free of copied payment, health, or identity data.
- Sample routine calls; do not duplicate every recording into a second QA store.
- Route legal retention, consent, and deletion rules by jurisdiction and contract.


[Amazon Connect Contact Lens](https://docs.aws.amazon.com/connect/latest/adminguide/contact-lens.html) describes reviewing recordings and transcripts alongside evaluation criteria, and it supports redaction of sensitive transcript and audio data. Redaction is still probabilistic: a “redacted” label should not grant broad access without an appropriate review policy.


## The Call Replay Acceptance Gate


Before rolling the workflow out to QA reviewers, test 20 calls that include normal turns, interruptions, silence, failed tools, transfers, and at least one restricted-data case.


Approve the replay workflow only when:


- all 20 calls join to the correct recording, transcript, trace, and evaluation
- transcript clicks land on the intended spoken turn
- speaker labels and participant changes remain understandable
- missing evidence is explicit rather than silently omitted
- tool failures cannot appear as verified success
- unauthorized reviewers cannot access restricted audio or raw text
- every finding can be classified, assigned, and exported to the next workflow


Then rerun every affected case plus at least one normal control call whenever the telephony provider, recorder, transcript merger, event schema, or review UI changes. Do not reuse the original sign-off when a changed component can alter timing, identity, permissions, or evidence joins.


Call replay is the diagnosis step. It does not prove the fix. When a reviewer finds a real defect, use the[failed production call regression test runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) to turn the relevant caller behavior and expected outcome into durable coverage.
