---
schema_version: "1.0.0"
document_id: "181cd01e319a96f7cc2c642bebf890e4168fa9b0b3e07ccbccac4ceaaecc21ba"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/voice-agent-call-evidence-export-runbook"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-27T03:05:01.547836+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:641fdb6f6f30e4c2ce1f6d465ec22318128cdf9aac2a32d304f8694d443ed738"
---

# Voice Agent Call Evidence Export Runbook: Transcripts, Audio, Traces, and QA Packets

**Voice agent call evidence export** is the workflow for packaging transcripts, audio, traces, tool-call records, and evaluation results so QA, compliance, and engineering reviewers can inspect selected calls without searching five production systems.


The common mistake is exporting transcripts and calling the job done. A transcript may show what was said, but it will not prove which agent version ran, which provider call ID owns the recording, whether the audio was redacted, which tool call wrote to a backend, why the call was selected, or whether the reviewer saw the same artifact as everyone else.


If you only need to listen to 5 calls after a launch, this runbook is overkill. Use the dashboard. If you review hundreds or thousands of calls across agents, regions, queues, or compliance programs, you need an evidence packet.


> **Scope:** this runbook applies to production voice agents that already capture transcripts, recordings, traces, tool calls, and evaluation results. It does not replace retention policy, legal review, recording-consent rules, or provider-specific export permissions.
>
>
> **TL;DR:** Export voice agent calls as reviewer-safe evidence packets with 10 required fields: canonical call ID, provider aliases, selection reason, redacted transcript, audio pointer, trace ID, tool-call summary, evaluation result, redaction state, and manifest hash.
>
>
> Do not batch download everything. Export the calls with a reason: failure cluster, compliance sample, regression candidate, escalation spike, customer report, or scheduled QA cohort.
>
>
> **Call evidence packet:** a bounded bundle that lets a reviewer reconstruct one voice-agent call from transcript to audio to traces to tool behavior without broad production access.


> **Methodology Note:** This export workflow is based on Hamming's analysis of production voice agent calls and QA review workflows across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> The packet fields focus on the evidence families teams need for QA review, incident follow-up, compliance sampling, and regression-test promotion.


Across Hamming's 10M+ mins protected and 10K+ agents, we found that the painful part is rarely the download button. Review breaks when the transcript, audio, trace, tool result, and evaluation score point to different IDs.


**Last Updated:** June 2026


**Related Guides:**


- [Voice Agent Call Replay QA Checklist](https://hamming.ai/resources/voice-agent-call-replay-qa-checklist) - verify exported audio, transcripts, traces, and tool evidence stay synchronized
- [Call Logging for AI Voice Agents](https://hamming.ai/resources/call-logging-voice-agents-taxonomy-compliance) - define the events and metadata that feed exports
- [IVR and Voice Agent Log Correlation](https://hamming.ai/resources/ivr-voice-agent-log-correlation-runbook) - preserve call identity across IVR, telephony, transcripts, and outcomes
- [OpenTelemetry for Voice Agents](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) - propagate trace context across ASR, LLM, tools, and TTS
- [Failed Production Call Regression Tests](https://hamming.ai/resources/failed-production-call-regression-test-runbook) - promote selected failures into repeatable tests
- [Voice Agent Workflow Testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - assert tool calls, state transitions, and side effects
- [Multilingual Voice Agent Transcript Repository](https://hamming.ai/resources/multilingual-voice-agent-transcript-repository) - keep native text, translations, language confidence, and QA labels joined before export
- [Voice Agent Log Retention Compliance Checklist](https://hamming.ai/resources/voice-agent-log-retention-compliance-checklist) - decide how long exported artifacts should live
- [Voice Agent Analytics Grafana Dashboard](https://hamming.ai/resources/voice-agent-analytics-grafana-dashboard) - keep operational dashboards separate from evidence storage


## What Goes in a Call Evidence Packet


The packet is not one file. It is a manifest plus controlled references to the artifacts a reviewer needs.


Use this as the minimum schema:


Field Required? Sample Value Why It Matters


` canonicalCallId


` Yes` call_2026_06_03_1842


` Stable internal identity across providers and systems


` providerAliases


` Yes Twilio CallSid, LiveKit room, Retell call ID, Vapi call ID Lets reviewers find source artifacts


` selectionReason


` Yes` failed_tool_call


` ,` qa_sample


` ,` compliance_sample


` Explains why this call was exported


` agentVersion


` Yes` billing


-


agent


@


2026


-


06


-


03


.


2


` Connects behavior to a shipped prompt/model/tool config


` redactedTranscriptUri


` Yes` s3


:


//review-packets/.../transcript.json


` Reviewable conversation text


` audioUri


` Usually` s3


:


//review-packets/.../audio.wav


` Lets QA hear silence, interruption, noise, tone, and playback issues


` traceId


` Usually` 4bf92f3577b34da6a3ce929d0e0e4736


` Connects logs, spans, model calls, and tool calls


` toolEvidence


` For workflow agents tool name, call ID, argument summary, result summary Proves whether backend actions matched the conversation


` evaluationResult


` Yes` task_failed


` , score` 0


.


42


` , rubric` refund_policy_v3


` Shows how the call was judged


` redactionState


` Yes` redacted


` ,` raw_restricted


` ,` aggregate_only


` Prevents accidental broad access to sensitive content


` manifestHash


` Yes SHA-256 of manifest Proves the packet did not silently change after review


` reviewStatus


` Yes` queued


` ,` reviewed


` ,` promoted_to_test


` ,` ignored


` Keeps offline review from becoming a dead archive


> **Export rule:** if the transcript, audio, trace, and tool evidence cannot be joined by one call identity, the packet is not audit-ready.


This is why the[IVR log correlation runbook](https://hamming.ai/resources/ivr-voice-agent-log-correlation-runbook) matters before export. A reviewer should not need to know whether the source system calls the object a CallSid, contact ID, room name, test run, or trace. Store those as aliases under one internal call ID.


I used to think the transcript was the main export artifact. After reviewing production voice-agent failures, I now treat the transcript as one field in a packet; the manifest, selection reason, call identity, redaction state, and trace/tool pointers are what make the review repeatable.


## Choose Calls by Review Reason, Not by Volume


Batch export is dangerous when it starts with "give me everything from yesterday." That creates large sensitive datasets and gives reviewers too much noise.


Start with a selection taxonomy:


Selection Reason Include When Export Priority Reviewer


` customer_reported


` A customer or support team named a bad call, timestamp, account, or symptom Highest Engineering + support


` failed_tool_call


` Tool failed, timed out, returned invalid data, duplicated a write, or used wrong arguments Highest Engineering


` unsafe_or_noncompliant


` The agent gave unsafe advice, missed a disclosure, leaked sensitive data, or ignored policy Highest Compliance + QA


` regression_candidate


` A production failure should become a test case High Engineering + QA


` low_confidence_turn


` ASR or intent confidence fell below threshold Medium QA


` latency_or_silence


` The call had long pauses, slow response, dead air, or interruption failures Medium Engineering


` scheduled_sample


` Randomized or stratified QA sample by agent, queue, language, or cohort Medium QA


` executive_report_sample


` Representative calls for weekly quality review Low Ops leadership


For regression candidates, pair this with the[failed-call regression runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) . The evidence packet explains the original failure; the regression test recreates the smallest safe version of that failure.


## Export Provider Artifacts Without Losing Context


Most voice-agent stacks already expose the raw ingredients. The hard part is keeping them joined.


Source System Export Join Key Keep Out of Broad Exports


Telephony provider call metadata, recording ID, disconnect reason, call quality metrics provider call ID and canonical call ID phone numbers, raw SIP headers with customer data


Voice agent platform transcript, messages, recording URL, log URL, PCAP, call analysis, latency platform call ID and trace ID raw logs with secrets or unredacted variables


Media runtime room events, participant events, track IDs, egress recordings room name, participant ID, canonical call ID raw media for callers outside the selected cohort


Evaluation system rubric, score, guardrail results, evaluator version, failure label test run ID and call ID model prompts with sensitive customer data


Tool/backend logs tool name, request summary, result summary, retry status, side-effect proof tool-call ID, trace ID, call ID full payloads unless needed for restricted review


Object storage transcript JSON, audio file, manifest, redaction report object path and manifest hash raw unredacted archive outside approved role


[Twilio's Recording resource](https://www.twilio.com/docs/voice/api/recording) documents metadata and media retrieval for recordings, including call SID, recording SID, status, channels, duration, and recording URL callbacks. Its[Transcriptions resource](https://www.twilio.com/docs/voice/api/recording-transcription) represents text and metadata from transcribed recordings. Those are source artifacts, not the complete evidence packet.


[Amazon Connect](https://docs.aws.amazon.com/connect/latest/adminguide/about-recording-behavior.html) stores recordings and transcripts in S3-backed locations, and its Contact Lens output paths separate original transcript JSON, redacted transcript JSON, and redacted audio by contact ID. That separation is a useful pattern: raw audio, redacted text, and analytics output should not be treated as the same artifact.


[LiveKit Egress](https://docs.livekit.io/home/egress/composite-recording/) can record or export rooms and tracks. LiveKit's agent observability docs also describe recordings, transcripts, traces, and logs. For export packets, keep the room or participant IDs as aliases, then tie the media pointer back to the canonical call ID used by the rest of the evidence.


[OpenTelemetry context propagation](https://opentelemetry.io/docs/concepts/context-propagation/) is what keeps traces, metrics, and logs correlated across services. For voice agents, propagate the trace ID through ASR, LLM, tools, TTS, storage, and evaluation so the reviewer can move from a failed score to the span or tool call that explains it.


Voice-agent platforms expose similar artifact families.[Vapi artifact plans](https://docs.vapi.ai/assistants/call-recording) can include recordings, transcripts, logs, PCAP files, and API-accessible call artifacts.[Retell's call APIs](https://docs.retellai.com/api-references/get-call) expose call details, transcript, recording, latency, and function-call data, while its dynamic variables include a per-call ID. Treat those IDs as aliases in your manifest, not as the only identity in your system.


## Build the Daily Export Job


Do not let humans download files by hand from dashboards. Build a repeatable job.


Step Action Output Failure Mode to Block


1. Select calls Query by selection reason, cohort, severity, date, and owner candidate list no selection reason


2. Resolve identity Map provider aliases to one canonical call ID identity map duplicate or missing call ID


3. Fetch transcript Pull redacted transcript or generate redacted copy transcript artifact raw transcript exported to broad bucket


4. Fetch audio pointer Store controlled URI or scoped signed URL; copy raw audio only if required audio reference unavailable or wrong-channel recording


5. Fetch traces Attach trace ID and key span summary trace summary trace missing for engineering-review packet


6. Fetch tool evidence Attach tool-call summaries and side-effect proof tool evidence JSON transcript says success but tool proof missing


7. Attach evaluation Add rubric, score, guardrail result, evaluator version evaluation result score without rubric/version


8. Write manifest Hash packet manifest and object references manifest JSON manifest changed after review


9. Queue review Assign reviewer, SLA, and outcome choices review task orphaned export with no owner


For workflow agents, the export job should preserve enough tool evidence for the[workflow testing runbook](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) : tool selected, arguments, result, order, retry status, and side-effect proof. The transcript alone can say "I booked that appointment" while the backend shows no appointment or two duplicates.


## Use a Manifest as the Control Plane


The manifest is the part auditors and engineers can trust later.


> **Manifest rule:** the manifest should answer four questions without opening a dashboard: which call was exported, why it was selected, which artifacts were attached, and whether the packet changed after review.


```text
{        "  canonicalCallId  "  :     "  call_2026_06_03_1842  "  ,        "  selectionReason  "  :     "  failed_tool_call  "  ,        "  agentVersion  "  :     "  billing-agent@2026-06-03.2  "  ,        "  providerAliases  "  :     {          "  twilioCallSid  "  :     "  CA...  "  ,          "  livekitRoom  "  :     "  billing-prod-1842  "  ,          "  traceId  "  :     "  4bf92f3577b34da6a3ce929d0e0e4736  "        }  ,        "  artifacts  "  :     {          "  redactedTranscriptUri  "  :     "  s3://review-packets/2026-06-03/call_1842/transcript.json  "  ,          "  audioUri  "  :     "  s3://review-packets/2026-06-03/call_1842/audio.wav  "  ,          "  toolEvidenceUri  "  :     "  s3://review-packets/2026-06-03/call_1842/tools.json  "  ,          "  evaluationUri  "  :     "  s3://review-packets/2026-06-03/call_1842/evaluation.json  "        }  ,        "  redactionState  "  :     "  redacted  "  ,        "  review  "  :     {          "  owner  "  :     "  qa  "  ,          "  status  "  :     "  queued  "  ,          "  allowedOutcomes  "  :     [  "  no_issue  "  ,     "  bug  "  ,     "  promote_to_regression  "  ,     "  needs_compliance_review  "  ]        }    }
```


Hash the manifest after the export finishes. If a transcript, audio pointer, trace ID, or tool-evidence object changes, write a new manifest version instead of mutating the old one.


This is especially important for compliance review. The[log retention checklist](https://hamming.ai/resources/voice-agent-log-retention-compliance-checklist) covers retention classes and legal holds; this runbook covers the packet you hand to a reviewer inside those policies.


## Redact Before Broad Review


Raw audio and transcripts are not ordinary debug logs. They can contain account numbers, addresses, health details, payment information, caller names, employee names, and accidental background speech.


Use three export levels:


Export Level Who Gets It Contents Use Case


` aggregate_only


` Leadership, reporting counts, cohorts, failure labels, samples without call artifacts weekly quality report


` redacted_packet


` QA, support, product, engineering redacted transcript, audio pointer if allowed, trace/tool summaries, evaluation results normal call review


` raw_restricted


` approved security/compliance role raw audio/transcript and full payloads legal, dispute, incident, or deep RCA


Default to` redacted_packet


` . Use[PII redaction for voice agents](https://hamming.ai/resources/pii-redaction-voice-agents) before broad review, and keep access to raw audio narrower than access to metrics. A reviewer usually needs to hear the relevant segment, not export every second of every call forever.


> **Reviewer-safe export:** if a broad QA reviewer can see raw audio, unredacted transcript text, secrets, or full backend payloads by default, the export boundary is too wide.


## Run Quality Gates Before Sharing the Packet


Block the export when the packet cannot be trusted.


Gate Check Block When


Identity canonical call ID maps to every provider alias duplicate, missing, or conflicting IDs


Transcript redacted transcript exists and has ordered speaker turns missing turns or unknown redaction state


Audio audio pointer exists and matches call duration/channel policy broken link, wrong channel, or restricted audio copied broadly


Trace trace ID opens the relevant call or turn missing trace for engineering packet


Tool evidence tool calls include request summary, result summary, latency, retries transcript implies action but no backend proof


Evaluation rubric, score, evaluator version, and failure label are present score cannot be interpreted


Manifest manifest hash is written after artifacts are finalized mutable packet with no version


Review owner and outcome choices are assigned export has no reviewer or next action


The[voice agent analytics Grafana dashboard](https://hamming.ai/resources/voice-agent-analytics-grafana-dashboard) should help find cohorts and anomalies. It should not become the system of record for raw call replay or evidence review. Metrics dashboards are for detection; evidence packets are for inspection.


## What Not to Export


More data does not make a better review packet.


Avoid exporting:


- Full raw provider logs unless the reviewer needs them for a restricted incident review.
- Secrets, API keys, auth headers, or webhook payloads with credentials.
- Unredacted phone numbers, addresses, payment fields, or health information in broad QA packets.
- Every trace span when a summary plus trace ID is enough.
- Every call from a high-volume day when the selection reason is unclear.
- Prompt chains or hidden system instructions unless the reviewer has explicit need and access.
- Metric labels with call IDs, user IDs, phone numbers, or transcript text.


The[Grafana dashboard guide](https://hamming.ai/resources/voice-agent-analytics-grafana-dashboard) covers why high-cardinality identifiers do not belong in metrics labels. Use replay pointers and trace IDs instead.


## How Hamming Fits


Hamming helps teams connect production call analysis, transcripts, audio, traces, tool evidence, and evaluation results into one review workflow. The value is not another export button. The value is deciding which calls deserve review, preserving the evidence that explains why, and turning the right failures into lasting coverage.


Use Hamming when you need to:


- Review only the calls that need attention instead of sampling blindly.
- Attach evaluation results to transcripts, audio, traces, and tool behavior.
- Preserve evidence packets for QA, compliance, incident response, and regression promotion.
- Turn production failure clusters into[response coverage](https://hamming.ai/resources/voice-agent-response-coverage) improvements and[sandbox tests](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) .
- Keep reviewers focused on decision-ready packets rather than dashboard archaeology.


The operating loop is simple: detect the call, export the packet, review the evidence, fix the issue, and promote the pattern when it should never return.


## Reviewer Handoff Checklist


Before a packet leaves the export job, verify:


- Every packet has one canonical call ID and provider aliases.
- Every packet has a selection reason and reviewer owner.
- Transcript redaction state is explicit.
- Audio is a controlled pointer unless raw export is approved.
- Trace ID and key span summary are attached for engineering packets.
- Tool-call summaries include request, result, latency, retry, and side-effect proof.
- Evaluation result includes rubric, score, evaluator version, and failure label.
- Manifest hash is written and immutable for the review version.
- Review outcomes include` no_issue


` ,` bug


` ,` promote_to_regression


` , and` needs_compliance_review


` .
- Retention class matches the[log retention checklist](https://hamming.ai/resources/voice-agent-log-retention-compliance-checklist) .


If a customer-reported incident is involved, pair the packet with the[voice agent incident response runbook](https://hamming.ai/resources/voice-agent-incident-response-runbook) . If the packet includes multiple languages, use the[multilingual transcript repository](https://hamming.ai/resources/multilingual-voice-agent-transcript-repository) to keep native transcript, translation, confidence, and reviewer labels under the same canonical call ID. If a failure should become a test, promote it with the[failed-call regression runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) .
