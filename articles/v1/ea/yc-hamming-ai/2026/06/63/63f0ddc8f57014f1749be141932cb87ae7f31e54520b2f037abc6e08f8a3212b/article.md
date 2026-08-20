---
schema_version: "1.0.0"
document_id: "63f0ddc8f57014f1749be141932cb87ae7f31e54520b2f037abc6e08f8a3212b"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/voice-agent-dead-air-detection-guide"
published_at: "2026-06-27T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:197eafcdb3e73417fdabee2fa4b757c60914f1b42f2c0c79bfd29c4ad5a09643"
---

# Voice Agent Dead Air Detection: Root Causes and Fixes

Most teams notice dead air only after a caller complains or a reviewer listens to the recording. The call felt broken in the moment, but the dashboard still looked green, and the transcript only shows a blank stretch where the failure happened.


The trap is treating "silence" as one metric. In production, the same dead-air symptom can come from a missing RTP stream, endpointing that waits too long, an STT provider that has not finalized speech, a slow tool call, delayed TTS, or audio that was generated but never played to the caller.


If you run fewer than 100 calls a week, manual review may be enough. Once the agent is handling production volume across support, sales, healthcare, financial services, QSR, or booking workflows, "there was silence" is not actionable. The event has to tell an engineer which layer should be checked first.


> **TL;DR:** Detect voice agent dead air as a turn-level event:
>
>
> - **Measure the caller-visible gap:** silence start, silence end, phase of call, preceding speaker, expected next action, and duration.
> - **Join component timings:** telephony, VAD, STT finalization, LLM first token, tool execution, TTS first audio, and playback.
> - **Route by owner:** telephony owns missing media, VAD/STT owns endpointing, backend owns slow tools, speech owns TTS/playback, product owns confusing flows.
> - **Save evidence:** audio pointer, transcript pointer, trace ID, provider call IDs, event JSON, review outcome, and regression-test link.
>
>
> A useful dead-air alert should answer four questions before anyone opens a log: what went silent, who heard it, how long it lasted, and which component is the best first place to look.


> **Methodology Note:** This dead-air detection workflow is based on Hamming's analysis of production voice agent calls and QA review workflows across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> The thresholds below are starter thresholds, not universal SLA promises. Tune them by workflow, language, channel, and caller population.


**Last Updated:** June 2026


**Related Guides:**


- [Voice Agent Troubleshooting](https://hamming.ai/resources/voice-agent-troubleshooting) - broader symptom-to-layer debugging across ASR, LLM, TTS, and telephony
- [Voice AI Latency Guide](https://hamming.ai/resources/voice-ai-latency-whats-fast-whats-slow-how-to-fix-it) - response-time benchmarks and latency fixes
- [OpenTelemetry for Voice Agents](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) - trace model for STT, LLM, tools, TTS, and playback spans
- [Slack Alerts for Voice Agents](https://hamming.ai/resources/slack-alerts-voice-agents-monitoring-guide) - alert routing, severity, and noise control
- [Voice Agent Call Evidence Export Runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - reviewer-safe transcript, audio, trace, and QA packets
- [Voice Agent Drop-Off Analysis](https://hamming.ai/resources/voice-agent-drop-off-analysis-abandonment-framework) - connect dead air to abandonment and funnel loss
- [Voice Agent Interruption Handling](https://hamming.ai/resources/voice-agent-interruption-handling-runbook) - distinguish silence problems from barge-in and backchannel issues
- [Debug WebRTC Voice Agents](https://hamming.ai/resources/debug-webrtc-voice-agents-troubleshooting-guide) - RTP, WebRTC, audio, and media-path troubleshooting


## What Counts as Dead Air in a Voice Agent Call?


Dead air is a caller-visible pause where the caller reasonably expects the voice agent to speak, listen, or advance the workflow, but no useful audio or progress happens.


> **Dead-air event:** a timestamped voice-agent event where the call enters an unexpected no-progress state for longer than the allowed threshold. The event should include phase, duration, preceding speaker, expected next action, component timing, and evidence pointers.


Do not reduce dead air to "audio level equals zero."[Twilio Voice Insights](https://www.twilio.com/docs/voice/voice-insights/call-summary) can flag silence when there is a missing RTP stream or total silence from one party; Twilio also notes that silent calls may be reported as dead air or dropped calls. That signal is useful, but by itself it cannot tell you whether the agent was stuck in VAD, waiting on a tool, generating speech, or failing to play audio.


Use this scope table before alerting:


Event Count as dead air? Why


Caller pauses mid-thought for 1.2 seconds No Natural hesitation; the agent should not interrupt too early


Agent waits 3.8 seconds after caller stops speaking Yes Caller expects a response; route to VAD/STT/LLM/tool/TTS timing


RTP stream is missing from one side Yes Telephony or media-path issue, even if the agent pipeline is healthy


Tool call takes 4 seconds before the agent says anything Yes Caller experiences silence; backend/tool owner must see the event


Agent speaks but caller cannot hear playback Yes Audio output or media delivery failure


Caller hears hold music or an explicit wait message Usually no The call has audio and expectation-setting; track separately as hold time


## How to Measure Dead-Air Rate and Duration


Measure dead air at the turn level. Call-level averages hide the exact exchange that broke.


Use these definitions:


```text
Dead  -  Air     Event     =     unexpected     no  -  progress     interval     above     threshold    Dead  -  Air     Rate     =     calls     with     at     least     one     dead  -  air     event     /     total     connected     calls    Dead  -  Air     Density     =     dead  -  air     events     /     100     connected     calls    Dead  -  Air     Duration     P95     =     p95     duration     of     dead  -  air     events     in     milliseconds
```


Start with separate thresholds by phase:


Phase Starter threshold Alert when First owner


Before greeting greater than 2s after connect spike by carrier, region, or agent version Telephony / runtime


After caller stops speaking greater than 2s repeated across turns or workflows VAD / STT / LLM


During tool execution greater than 3s with no filler or progress cue critical workflow affected Backend / tools


Before TTS playback greater than 1.5s after final text is ready provider or voice-specific spike TTS / playback


After interruption greater than 2s after agent stops occurs after barge-in or correction Turn handling


Before disconnect greater than 3s before hang-up abandonment rises Product / QA


One global silence threshold sounds tidy, but it creates bad alerts. A 2-second pause before a greeting feels broken. The same pause while a caller searches for an account number may be exactly right. The threshold needs both phase and expectation.


## Dead-Air Root-Cause Matrix


The fastest way to waste a dead-air incident is to send every example to the LLM owner. Some will be model latency. Many will not. The silence is often hiding a pipeline problem.


Symptom First evidence to check Likely cause Owner Fix


No greeting after call connects provider call summary, RTP/audio-in, room events missing RTP, one-way audio, room dispatch delay Telephony / runtime verify media path, dispatch, region, SIP/WebRTC path


Agent waits too long after caller finishes VAD events, STT finalization, endpointing config silence duration too long, noisy audio blocking endpointing VAD / STT tune endpointing, add semantic turn detection, test noisy fixtures


Transcript appears late STT partial/final timestamps STT provider delay or finalization gap STT compare partial vs final latency, add fallback or timeout


LLM starts late after transcript finalizes LLM first-token timestamp, prompt version slow model, prompt bloat, provider issue LLM / orchestration reduce prompt payload, stream earlier, add timeout/fallback


Silence during backend action tool span, webhook latency, retry count slow CRM/POS/calendar/API call Backend / tools cache, timeout, async handoff, progress cue


Text is ready but caller hears nothing TTS first-audio, audio playout, output track events TTS delay, playback failure, muted output TTS / media preconnect, stream TTS, validate output track


Silence after interruption interruption events, agent speech state, VAD state machine stuck after barge-in Conversation runtime fix turn state, replay interruption cases


Dead air before hang-up event sequence, abandonment stage, recording caller gave up after repeated latency or confusion Product / QA route to failure cluster and regression test


[Deepgram's endpointing docs](https://developers.deepgram.com/docs/understanding-end-of-speech-detection) explain why noisy environments can keep an audio-based VAD from detecting silence, especially in places like a fast food drive-thru.[OpenAI's Realtime VAD docs](https://developers.openai.com/api/docs/guides/realtime-vad) expose configurable fields such as threshold, prefix padding, and silence duration.[LiveKit's turn-detection docs](https://docs.livekit.io/agents/logic/turns/) and adaptive interruption handling docs show the same principle at the runtime layer: turn-taking needs more context than volume alone.


## Build a Component Timestamp Waterfall


A dead-air event should not depend on a reviewer adding a label after the fact. Review is still valuable, but the first event should come from timestamps your system already emits.


Minimum waterfall:


Timestamp Example field Question answered


Call connected` call


.


connected_at


` Did the caller reach the voice runtime?


User speech started` vad


.


speech_started_at


` Did the system hear the caller?


User speech stopped` vad


.


speech_stopped_at


` Did endpointing fire?


STT partial received` stt


.


partial_at


` Did transcription start quickly?


STT final received` stt


.


final_at


` Did finalization stall?


LLM request sent` llm


.


request_at


` Did orchestration start?


LLM first token` llm


.


first_token_at


` Did the model stall?


Tool request sent` tool


.


request_at


` Did a dependency block the turn?


Tool response received` tool


.


response_at


` Did the backend recover?


TTS request sent` tts


.


request_at


` Did speech generation start?


TTS first audio` tts


.


first_audio_at


` Did synthesis stall?


Audio playback started` audio


.


playout_started_at


` Did generated audio reach the caller path?


Pair this with[OpenTelemetry voice-agent tracing](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) . The trace should let an engineer answer: did the silence come from input audio, endpointing, STT, reasoning, tools, TTS, playback, or evaluation?


## Save Dead-Air Events as Evidence, Not Just Alerts


Alerting tells the team something happened. Evidence tells them what to fix.


Use a compact event schema:


```text
{        "  eventType  "  :     "  voice_agent.dead_air  "  ,        "  severity  "  :     "  warning  "  ,        "  callId  "  :     "  call_2026_06_27_1842  "  ,        "  turnIndex  "  :     6  ,        "  phase  "  :     "  after_user_speech  "  ,        "  startedAtMs  "  :     42180  ,        "  endedAtMs  "  :     45940  ,        "  durationMs  "  :     3760  ,        "  precedingSpeaker  "  :     "  caller  "  ,        "  expectedNextAction  "  :     "  agent_response  "  ,        "  suspectedOwner  "  :     "  stt_endpointing  "  ,        "  componentTimingMs  "  :     {          "  vadStopToSttFinal  "  :     2840  ,          "  sttFinalToLlmFirstToken  "  :     240  ,          "  llmFirstTokenToTtsFirstAudio  "  :     310  ,          "  ttsFirstAudioToPlayout  "  :     90        }  ,        "  evidence  "  :     {          "  audioUri  "  :     "  s3://review-packets/call_1842/audio.wav  "  ,          "  transcriptUri  "  :     "  s3://review-packets/call_1842/transcript.json  "  ,          "  traceId  "  :     "  4bf92f3577b34da6a3ce929d0e0e4736  "        }  ,        "  reviewOutcome  "  :     "  needs_regression_test  "    }
```


For reviewer workflows, attach the event to a[call evidence export packet](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) . A QA reviewer should be able to replay the gap, read the surrounding transcript, inspect the trace, and see the component timing without broad production access.


## How Should Teams Alert on Dead Air?


Do not page on every pause. That turns the alert into background noise.


Use this starter policy:


Condition Action Channel Why


One active-turn gap greater than 3s create QA event review queue needs inspection, not panic


More than 2 dead-air events in one call Slack warning voice-alerts caller likely noticed


Dead-air rate doubles from 7-day baseline incident ticket voice-oncall regression or provider drift


Any critical workflow has dead air before task completion urgent alert workflow owner + on-call booking, payment, healthcare, account access


Dead air followed by hang-up promote to review QA + product likely abandonment


Dead air cluster after deploy block rollout release owner regression risk


The[Slack alerts guide](https://hamming.ai/resources/slack-alerts-voice-agents-monitoring-guide) includes a broader alerting table. For dead air, the alert payload should include the timestamp, preceding context, suspected owner, call link, trace link, and the first 2 nearby calls with the same signature.


## Fixes by Owner


Once the event has an owner, the fix is usually obvious.


Owner Checks Common fix


Telephony missing RTP, one-way audio, carrier/SIP region, WebRTC ICE state fix routing, TURN/SIP config, media region, provider incident


VAD / endpointing` speech_started


` ,` speech_stopped


` , silence duration, background noise tune threshold, use semantic endpointing, add noisy fixtures


STT partial vs final timing, confidence, provider fallback add timeout/fallback, use interim transcripts, adjust endpointing


LLM orchestration time to first token, prompt size, model latency, rate limits reduce context, stream earlier, timeout slow providers


Tools/backend webhook latency, retries, circuit breakers, cache misses cache pre-call data, add progress cue, shorten timeout


TTS synthesis time, first-audio time, voice/provider, preconnect state stream TTS, preconnect, fallback voice/provider


Playback/media output track, playout timestamp, mute state, egress recording validate output path, room participant state, audio device


Product / QA flow step, caller intent, abandonment stage, repeat attempts simplify prompt, add recovery line, create regression tests


For calls that end in abandonment, pair this with[drop-off analysis](https://hamming.ai/resources/voice-agent-drop-off-analysis-abandonment-framework) . Dead air explains one mechanism; the drop-off page explains where abandonment concentrates across the flow.


## What Dead-Air Detection Cannot Prove


Dead-air detection is a symptom detector. It does not prove the agent gave the right answer, followed policy, completed the task, or satisfied the caller.


Three limitations matter:


- **Silence is not always bad.** Callers pause to think, find information, or wait for a human transfer. Use phase and expected action before labeling it as failure.
- **Audio silence is not human-speech detection.** Twilio's Voice Insights FAQ notes that background noise can prevent a silent call from being marked silent, and that silence detection is not equivalent to speech detection.
- **A detected gap still needs root cause.** A 4-second gap could be endpointing, STT, LLM, tool, TTS, or playback. Without component timing, you only know the caller waited.


The useful standard is simple: every dead-air event should be reviewable and reproducible. If the team cannot replay the call, inspect the waterfall, and write the smallest regression test from the failure, the event is still only a complaint with a timestamp.
