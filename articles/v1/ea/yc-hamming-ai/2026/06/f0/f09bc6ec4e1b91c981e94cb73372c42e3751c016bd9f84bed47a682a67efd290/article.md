---
schema_version: "1.0.0"
document_id: "f09bc6ec4e1b91c981e94cb73372c42e3751c016bd9f84bed47a682a67efd290"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/voice-agent-transcript-search-schema"
published_at: "2026-06-06T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:47f745f4bd89e12a27222f0e011e0099b039b3c7f4e4758780efcfc88063ee7a"
---

# Voice Agent Transcript Search Schema for QA Teams

**Voice agent transcript search** is the ability to find the specific calls and turns that matter across transcripts, audio replay pointers, prompt versions, traces, QA labels, redaction state, and evaluation results.


Most teams do not fail because they forgot to store transcripts. They fail because the transcript is a blob. It cannot answer "show me Spanish calls where the billing agent missed identity verification after prompt v42," or "find every failed tool call where the user said cancel and the audio segment is reviewable."


If your team reviews a dozen calls manually each week, a provider dashboard may be enough. This schema is for teams operating multiple agents, queues, languages, prompt releases, or compliance workflows where transcript search becomes the front door for QA and incident review.


We found the recurring failure was review handoff: teams could find the call, but not the turn, replay offset, redaction state, or evaluation that made the call actionable.


> **TL;DR:** Build transcript search as five linked records: call, turn, artifact, label, and evaluation.
>
>
> Make redacted turn text searchable. Keep raw audio, unredacted transcript, tool payloads, and full traces as controlled pointers. Every query result should tell a reviewer what matched, where to replay it, whether it was redacted, which prompt version ran, and what action to take next.


> **Transcript search schema:** a voice-agent data model that lets teams query call turns by text, time, speaker, language, intent, sentiment, prompt version, redaction state, trace ID, audio offset, QA label, and evaluation result without granting broad access to raw recordings.


> **Methodology Note:** This schema is based on Hamming's analysis of production voice agent calls and QA review workflows across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected.
>
>
> Treat it as an implementation contract, not legal advice. Regulated archives, recording consent, and deletion windows still need counsel-approved policy.


**Last Updated:** July 2026


**Related Guides:**


- [Call Logging for AI Voice Agents](https://hamming.ai/resources/call-logging-voice-agents-taxonomy-compliance) - event taxonomy and compliance fields
- [Voice Agent Log Retention Compliance Checklist](https://hamming.ai/resources/voice-agent-log-retention-compliance-checklist) - retention classes and legal holds
- [Voice Agent Call Evidence Export Runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - reviewer-safe evidence packets
- [Multilingual Voice Agent Transcript Repository](https://hamming.ai/resources/multilingual-voice-agent-transcript-repository) - native text, translations, language confidence, and QA labels in one record model
- [IVR and Voice Agent Log Correlation](https://hamming.ai/resources/ivr-voice-agent-log-correlation-runbook) - canonical call identity and provider aliases
- [OpenTelemetry for AI Voice Agents](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) - trace IDs and span models
- [Voice Agent Analytics Grafana Dashboard](https://hamming.ai/resources/voice-agent-analytics-grafana-dashboard) - metrics views that point back to call evidence
- [Debugging Voice Agents](https://hamming.ai/resources/debugging-voice-agents-real-time-logs-missed-intents-error-dashboards) - missed-intent and error investigation
- [PII Redaction for Voice Agents](https://hamming.ai/resources/pii-redaction-voice-agents) - safe transcript and audio handling
- [Logging and Analytics Architecture](https://hamming.ai/blog/logging-and-analytics-architecture-for-voice-agents) - routing, storage, and retention patterns before search


## Why Transcript Blobs Break Search


A transcript blob is easy to store and hard to operate.


Blob Shortcut What Breaks Later


One JSON document per call with the full transcript Search can find a call, but not the specific turn, timestamp, or replay segment.


Raw transcript text in the analytics table Broad search becomes a privacy problem.


Audio URL stored beside the transcript only Reviewers cannot jump from a matching sentence to the right audio offset.


Prompt version stored in a deployment table only Teams cannot compare failures across prompt releases.


QA labels stored as comments Labels cannot drive dashboards, regression tests, or routing.


Provider call ID as the only key Transfers, LiveKit rooms, recordings, traces, and evaluations drift apart.


[Amazon Connect documentation](https://docs.aws.amazon.com/connect/latest/adminguide/about-recording-behavior.html) recommends using contact ID to find the correct recording because recording filenames may not reliably match the contact ID. That is the right instinct: search should depend on stable identity, not file names or dashboard URLs.


The same pattern applies to voice agents. Use one canonical call ID, store provider aliases under it, and make each transcript turn addressable.


## The Five Records to Model


Do not start with the search engine. Start with the records reviewers need.


Record Grain Searchable by Default? Purpose


` call


` one voice interaction Yes, metadata only Owns canonical identity, agent, queue, language, outcome, and retention class


` turn


` one speaker segment Yes, redacted text Owns transcript text, speaker, timestamps, language, sentiment, confidence, and replay offset


` artifact


` one external pointer No raw content Points to audio, transcript JSON, trace, tool evidence, redaction report, or provider record


` label


` one human or model tag Yes Stores intent, issue category, compliance tag, root cause, reviewer decision, and confidence


` evaluation


` one score or guardrail result Yes, summary only Stores rubric, evaluator version, pass/fail, score, failure reason, and suggested next action


This split keeps the common path fast and safe. QA can search redacted turns and labels. Engineering can jump from a result to a trace or tool summary. Compliance can restrict raw artifacts without breaking every dashboard.


> **Search rule:** broad transcript search should return redacted text, call identity, turn offsets, labels, scores, and replay pointers. It should not return raw audio, unredacted transcript text, secrets, or full backend payloads by default.


## Required Fields


Use this as the minimum warehouse or index contract.


Field Record Type Why It Matters


` canonicalCallId


` call, turn, artifact, label, evaluation string Joins every record to one call story


` providerAliases


` call object Stores CallSid, contact ID, room name, provider call ID, recording ID, and trace ID


` agentId


` call string Filters by deployed voice agent


` agentVersion


` call, turn string Compares behavior across prompt, model, workflow, or tool changes


` environment


` call enum Separates production, staging, sandbox, and test calls


` startedAt


` call timestamp Drives retention, cohorting, and incident windows


` routeOrQueue


` call string Groups calls by business flow


` language


` call, turn BCP 47 string Enables multilingual transcript search


` speaker


` turn enum Separates caller, agent, IVR, human agent, and system output


` turnStartMs


` /` turnEndMs


` turn integer Lets reviewers jump to the matching audio segment


` redactedText


` turn text Default searchable transcript field


` redactionState


` call, turn, artifact enum Prevents accidental raw-data exposure


` intentLabel


` label string Finds missed intents and flow gaps


` sentimentLabel


` label enum Supports QA triage and trend queries


` traceId


` /` spanId


` call, turn, artifact string Links search results to observability data


` audioArtifactId


` turn, artifact string Points to controlled audio replay


` rubricId


` /` score


` evaluation string / number Makes QA and model-judge results queryable


` reviewStatus


` evaluation enum Tracks whether a call needs action


` retentionClass


` call, artifact string Connects search to deletion and legal-hold policy


[Google's call session metadata docs](https://cloud.google.com/contact-center/ccai-platform/docs/call-session-metadata) show why call-level state, recording URLs, permissions, queues, and virtual-agent fields belong in structured metadata.[Azure Communication Services diagnostics logs](https://learn.microsoft.com/en-us/azure/communication-services/concepts/analytics/logs/call-diagnostics-log-schema) use correlation IDs, participant IDs, endpoint IDs, media type, stream IDs, jitter, packet loss, and related media fields. Voice-agent search needs those same join and quality concepts, plus transcript-turn, prompt-version, and QA fields.


## A Copyable JSON Shape


Here is a compact version. In production, you can store these as relational tables, search documents, lakehouse tables, or a hybrid index. The shape matters more than the storage product.


```text
{        "  call  "  :     {          "  canonicalCallId  "  :     "  call_2026_06_06_1842  "  ,          "  startedAt  "  :     "  2026-06-06T18:42:19.000Z  "  ,          "  environment  "  :     "  production  "  ,          "  agentId  "  :     "  billing-agent  "  ,          "  agentVersion  "  :     "  billing-agent@2026-06-06.3  "  ,          "  routeOrQueue  "  :     "  billing-support  "  ,          "  language  "  :     "  en-US  "  ,          "  providerAliases  "  :     {            "  telephonyCallId  "  :     "  CA...  "  ,            "  contactCenterContactId  "  :     "  contact-...  "  ,            "  livekitRoom  "  :     "  billing-prod-1842  "  ,            "  traceId  "  :     "  4bf92f3577b34da6a3ce929d0e0e4736  "          }  ,          "  retentionClass  "  :     "  support-investigation  "  ,          "  redactionState  "  :     "  redacted  "        }  ,        "  turn  "  :     {          "  turnId  "  :     "  turn_0007  "  ,          "  canonicalCallId  "  :     "  call_2026_06_06_1842  "  ,          "  speaker  "  :     "  caller  "  ,          "  turnStartMs  "  :     42840  ,          "  turnEndMs  "  :     49120  ,          "  language  "  :     "  en-US  "  ,          "  redactedText  "  :     "  I already verified my account. Why are you asking again?  "  ,          "  asrConfidence  "  :     0  .  86  ,          "  redactionState  "  :     "  redacted  "  ,          "  traceId  "  :     "  4bf92f3577b34da6a3ce929d0e0e4736  "  ,          "  spanId  "  :     "  span_asr_turn_0007  "  ,          "  audioArtifactId  "  :     "  audio_1842_redacted  "        }  ,        "  labels  "  :     [          {            "  labelType  "  :     "  intent  "  ,            "  labelValue  "  :     "  identity_verification_confusion  "  ,            "  source  "  :     "  reviewer  "  ,            "  confidence  "  :     1  .  0          }  ,          {            "  labelType  "  :     "  sentiment  "  ,            "  labelValue  "  :     "  negative  "  ,            "  source  "  :     "  model  "  ,            "  confidence  "  :     0  .  81          }        ]  ,        "  evaluation  "  :     {          "  rubricId  "  :     "  billing_identity_v4  "  ,          "  evaluatorVersion  "  :     "  2026-06-01  "  ,          "  score  "  :     0  .  42  ,          "  passed  "  :     false  ,          "  failureReason  "  :     "  agent_repeated_identity_check  "  ,          "  reviewStatus  "  :     "  promote_to_regression  "        }    }
```


The field names can change. The invariant should not: search results must join text, replay, trace, labels, scores, redaction, and version context without a human guessing which dashboard owns the truth.


## Query Cookbook


Design the search product around real review questions.


Question Required Filters Result Should Show


Which calls mentioned cancellation and ended with negative sentiment? transcript text, sentiment label, outcome matching turns, call outcome, replay offset, reviewer status


Did prompt v42 increase identity-verification confusion? agent version, label, date range count by version, sample calls, failing turns


Which Spanish calls had low ASR confidence and a failed task? language, ASR confidence, evaluation result turns, audio pointers, failure reasons


Which raw artifacts are not redacted yet? artifact type, redaction state artifact owner, retention class, blocked downstream views


Which tool failures are visible in the transcript? tool failure label, transcript phrase, trace ID call, turn, trace, tool summary


Which calls should become regression tests? review status, failure label, evaluation score evidence packet, owner, suggested test name


Which searches require restricted access? raw artifact request, unredacted field, user role denial reason or approval workflow


[Amazon Connect Contact Lens search](https://docs.aws.amazon.com/connect/latest/adminguide/search-conversations.html) supports search across analyzed conversations by transcript words, sentiment, non-talk time, and categories. That is a useful reference shape, but voice-agent teams usually need additional dimensions: prompt version, tool behavior, trace ID, evaluator version, and regression-promotion status.


## Search Index Architecture


There are two common approaches.


Approach Use When Watch Out For


One document per call with nested turns Call volume is moderate and each call has bounded turns Nested arrays can become expensive, and highlighting the matching turn may require special query handling


One document per turn plus call-level joins Call volume is high or reviewers need precise turn search You need reliable joins back to call metadata, artifacts, labels, and evaluations


For most production voice-agent QA workflows, one searchable record per turn is the cleaner default. Keep call metadata denormalized into the turn record where it is safe and stable: agent, version, language, route, date, and redaction state. Keep raw artifacts as IDs or URLs behind access checks.


[OpenTelemetry's logs data model](https://opentelemetry.io/docs/specs/otel/logs/data-model/) includes timestamp, trace ID, span ID, severity, body, resource, attributes, and event name. Use that as a mental model for events and logs, not as a reason to stuff full transcripts into ordinary application logs.


[OpenSearch mappings](https://docs.opensearch.org/latest/mappings/) recommend explicit mappings when consistency and performance matter. For transcript search, map structured filters such as` agentId


` ,` agentVersion


` ,` language


` ,` speaker


` ,` redactionState


` ,` routeOrQueue


` , and` reviewStatus


` as keyword-like fields, while` redactedText


` should be a full-text field. OpenSearch's[text field docs](https://docs.opensearch.org/latest/mappings/supported-field-types/text/) describe how analyzed text supports full-text search and highlighting. Its[text chunking docs](https://docs.opensearch.org/latest/vector-search/ingesting-data/text-chunking/) are useful when you add semantic search over longer passages, summaries, or call-level narratives.


## Open-Source Storage Patterns for Voice AI Conversation Logs


The honest answer is that there is no single open-source database that should own every voice AI conversation-log job.


Start with the review question. If QA needs to find the precise turn where a caller said "cancel," use a search index. If operations needs weekly failure-rate trends across 10 million turns, use a columnar store. If compliance needs raw audio, use controlled object storage. If product needs semantic similarity across failure descriptions, add a vector layer.


Pattern Good Fit What to Store Watch Out For


OpenSearch / Elasticsearch-style search Turn-level transcript search, highlighting, labels, reviewer queues Redacted turn text, call metadata, prompt version, QA labels, replay offsets Do not make it the raw-audio archive or the only compliance store


ClickHouse-style columnar analytics High-volume trends, latency cohorts, failure rates, dashboard aggregates Wide event rows, typed dimensions, materialized fields, safe derived metrics Full-text search may need explicit text indexes and careful schema design


Postgres plus pgvector Smaller repositories, product workflows, semantic search sidecars Relational call records, labels, review state, embeddings for summaries or failure reasons Approximate vector indexes trade recall for speed; keep original evidence links


Object storage plus manifests Raw audio, transcript JSON, redaction reports, export packets Audio files, raw artifacts, hashes, retention metadata, access policy Search should point here; broad search should not expose everything inside


OpenTelemetry-compatible log pipeline Cross-service correlation across ASR, LLM, tools, TTS, and evaluation Trace IDs, span IDs, event names, severity, attributes, artifact pointers Ordinary app logs are not a transcript repository


> **Open-source voice agent log repository:** a layered architecture that keeps searchable redacted turns, analytical event rows, raw artifact pointers, labels, evaluations, and trace context joined by one canonical call ID. The storage choice matters less than preserving the joins and access boundaries.


For a lean first version, do not build a lakehouse. Use one turn-level search index, one artifact manifest table, one controlled audio bucket, and one evaluation table. That is enough to answer the first operational questions: which calls failed, where is the matching audio, which version ran, and what should we do next?


As volume grows, split the read paths:


Query Best Primary Store Required Join


"Find calls where the caller said cancel and sentiment was negative" Search index` canonicalCallId


` to audio and evaluation


"Show failure rate by agent version for the last 30 days" Columnar analytics` agentVersion


` to label/evaluation rows


"Replay the failed turn with the tool trace" Artifact manifest + trace link` turnId


` to audio offset and trace ID


"Find similar failures to this reviewer note" Vector sidecar embedding result to canonical call evidence


"Prove this deleted call is no longer searchable" Source-of-truth retention table deletion state to every derived index


This is the part teams often overcomplicate. You do not need a perfect data platform before the first search box. You need stable call identity, redacted searchable turns, replay pointers, and a clear rule for what never enters broad search.


## Redaction and Access Boundaries


Transcript search is useful only if teams trust it.


Data Default Search Access Restricted Access


Redacted turn text QA, support, product, engineering Usually not needed


Unredacted turn text No broad search compliance-approved users


Raw audio Pointer only playback by approved roles


Redacted audio Pointer or clipped replay QA and engineering, depending on policy


Tool payloads summary only engineering or incident owner


Prompt/system instructions version pointer only owner-approved debugging


Trace data trace ID and key spans engineering detail view


Aggregate labels broad dashboards row-level drilldown by role


[Amazon Connect recording docs](https://docs.aws.amazon.com/connect/latest/adminguide/about-recording-behavior.html) describe where recordings and transcripts are stored and how contact IDs help locate the right recording.[Amazon Transcribe post-call analytics](https://docs.aws.amazon.com/transcribe/latest/dg/tca-post-call.html) can produce redacted and unredacted transcript/audio outputs depending on settings. The product lesson is simple: keep raw and redacted artifacts separate, and make the default search surface the safer copy.


Tie this to your[log retention checklist](https://hamming.ai/resources/voice-agent-log-retention-compliance-checklist) . A search index is not a legal archive. It is a review surface that should respect retention class, redaction state, deletion status, and legal hold.


## What to Validate Before Launch


Run these checks before you rely on transcript search for QA or incidents.


Gate Pass Condition Block When


Identity Every turn joins to one canonical call ID provider IDs conflict or calls split across records


Replay Matching turns have audio offsets and controlled artifact pointers reviewer cannot jump from text to audio


Redaction Broad search indexes only approved redacted text unredacted text appears in general results


Versioning Calls and turns include agent or prompt version failures cannot be compared across releases


Labels Intent, sentiment, issue category, and reviewer labels have source and confidence labels are comments or untyped strings only


Evaluation Scores include rubric ID, evaluator version, and failure reason score cannot be interpreted


Access User role controls raw playback, export, and unredacted fields search bypasses storage permissions


Deletion Removed calls disappear from search and derived indexes deleted source remains searchable


Observability Search result links to trace ID or evidence packet engineering cannot debug the matched call


This is where transcript search connects to the[call evidence export runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) . Search finds the calls and turns. Evidence packets preserve the bounded artifacts a reviewer needs. For multilingual fleets, the[multilingual transcript repository](https://hamming.ai/resources/multilingual-voice-agent-transcript-repository) keeps native text, translations, language confidence, and labels joined before search indexes flatten the evidence. The[failed production call regression runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) turns the right failures into durable tests.


## How Hamming Fits


Hamming helps teams search, score, review, and act on production voice-agent calls without treating transcripts as isolated text files. The important thing is not just finding a phrase. It is knowing what happened around that phrase: which prompt version ran, what the caller heard, what the model decided, which tool call fired, how the evaluator scored it, and whether the result should become a regression test.


Use Hamming when you need to:


- Search production calls by transcript, label, score, prompt version, language, and failure reason.
- Replay the relevant audio segment next to the redacted transcript and evaluation result.
- Connect call search to traces, tool evidence, QA findings, and incident workflows.
- Promote recurring failures into[response coverage](https://hamming.ai/resources/voice-agent-response-coverage) improvements and regression tests.
- Keep analytics dashboards pointed at evidence instead of turning dashboards into raw transcript archives.


The operating loop is straightforward: search the turns, replay the evidence, label the issue, fix the behavior, and preserve the pattern if it should never happen again.


## Launch Checklist


Before shipping transcript search, make sure:


- Calls, turns, artifacts, labels, and evaluations are separate records or separate logical views.
- Every record joins through one canonical call ID.
- Broad search indexes redacted turn text, not raw transcript text.
- Audio replay uses controlled pointers with turn-level offsets.
- Prompt, model, workflow, or agent version is stored on every call.
- Intent, sentiment, issue category, and reviewer labels are typed and source-attributed.
- Evaluation rows include rubric ID, evaluator version, score, pass/fail, and failure reason.
- Search results show redaction state, retention class, and access limitations.
- Deleted or legally restricted calls are removed from searchable surfaces.
- Search results link to trace IDs, evidence packets, or incident runbooks.


If you cannot check those boxes, do not scale the search surface yet. You may have a transcript archive, but you do not have a QA-ready transcript search system.
