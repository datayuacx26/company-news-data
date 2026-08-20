---
schema_version: "1.0.0"
document_id: "d314345953e87c06bdef73c899cb04eff90545d3108c0bbb7cb0b02982c1ac7d"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/multilingual-voice-agent-transcript-repository"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:23:18.688239+00:00"
content_hash: "sha256:dd1c434db81ba6165aeba251e9156b7e6827268cb2249ba6f6d8260f2860e0d4"
---

# Multilingual Voice Agent Transcript Repository: Architecture and Schema

**A multilingual voice agent transcript repository** is the system of record that centralizes native transcripts, optional translations, language confidence, audio replay pointers, QA labels, evaluation results, and analytics fields across every language and voice bot your team runs.


Most teams do not fail because they forgot to save transcripts. They fail because Spanish support calls, Hindi-English code-switching, German consent flows, and English regression tests all land in slightly different shapes. The warehouse has text, but not the language confidence. The QA dashboard has scores, but not the native transcript. The audio is in another system. Nobody can tell whether a bad score came from the agent, the speech-to-text model, or a translation layer.


If you run one English-only agent and review 10 calls a week, this architecture is probably too much. Use your provider dashboard and keep moving. This guide is for teams operating multiple agents, regions, languages, queues, or compliance programs where multilingual transcript aggregation becomes the front door for QA and analytics.


> **TL;DR:** Build the repository around six linked records: call, turn, language, artifact, label, and evaluation.
>
>
> Store native transcript text and translated review text separately. Keep language code, language confidence, audio offset, redaction state, region, agent version, trace ID, QA label, and evaluation result as typed fields. Do not hide multilingual evidence in one transcript blob.
>
>
> **Repository rule:** a reviewer should be able to answer one question from a search result: what happened in the original language, how confident was the language pipeline, where is the matching audio, and what action should we take next?


> **Methodology Note:** This repository schema is based on Hamming's analysis of production voice agent calls, QA review workflows, and multilingual testing patterns across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> Treat this as an engineering architecture guide, not legal advice. Data residency, retention, consent, and deletion policies still need owner-approved controls for each market.


**Last Updated:** June 2026


**Related Guides:**


- [Voice Agent Transcript Search Schema](https://hamming.ai/resources/voice-agent-transcript-search-schema) - searchable turn, label, artifact, and evaluation records
- [Call Logging for AI Voice Agents](https://hamming.ai/resources/call-logging-voice-agents-taxonomy-compliance) - event taxonomy and compliance fields
- [Voice Agent Call Evidence Export Runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - reviewer-safe export packets
- [Voice Agent Log Retention Compliance Checklist](https://hamming.ai/resources/voice-agent-log-retention-compliance-checklist) - retention classes and legal holds
- [PII Redaction for Voice Agents](https://hamming.ai/resources/pii-redaction-voice-agents) - safe transcript and audio handling
- [Multilingual Voice Agent Testing](https://hamming.ai/resources/multilingual-voice-agent-testing) - language coverage and localized scenario testing
- [Multi-Tenant Voice Agent Analytics Dashboards](https://hamming.ai/resources/multi-tenant-voice-agent-analytics-dashboards-bpo) - tenant-safe reporting for outsourced operations
- [OpenTelemetry for AI Voice Agents](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) - trace IDs and span models


## What the Repository Must Prove


A multilingual repository is not just storage. It has to prove that five systems agree on the same call:


System What It Owns What the Repository Must Keep


Telephony or CCaaS call identity, recording, route, region provider aliases, canonical call ID, audio pointer


Speech-to-text native transcript, language code, confidence, speaker turns turn text, language confidence, model version, timing


Translation or localization translated review text and glossary decisions translation text, translation provider, review status


QA and evaluation labels, scores, pass/fail, reviewer decisions evaluation result, rubric version, failure reason


Analytics warehouse cohorts, trends, dashboards, exports safe aggregate fields and access policy


The mistake is letting one layer pretend to be the system of record. A transcript service can identify language. It cannot decide retention. A warehouse can aggregate calls. It cannot prove which audio segment matched the failed turn unless the repository carries the pointer.


We found that multilingual review breaks at the handoff. A team can find the call, but not the native turn, translated text, audio offset, STT confidence, and evaluation that explain the issue.


I used to think this was mostly a warehouse design problem. It is not. The hard part is deciding which record is allowed to answer the question when the native transcript, translated review text, audio, and score disagree.


## The Repository Architecture


Use a two-zone architecture: evidence storage for controlled artifacts, and query storage for searchable fields.


Layer Stores Default Access Do Not Store Here


Capture layer provider call ID, room ID, audio URI, timestamps, consent state platform and compliance owners long-term QA decisions


Transcript layer native turns, speaker, language code, language confidence, model version QA and engineering, after redaction unrestricted raw audio


Translation layer translated text, glossary version, reviewer language, translation confidence reviewers who need cross-language triage source-of-truth decisions without native text


Artifact layer audio pointers, trace links, redaction reports, transcript JSON role-scoped users broad searchable PII


Analytics layer aggregate metrics, labels, cohorts, trends product, QA, operations raw transcripts or unrestricted recordings


[Google Cloud Speech-to-Text](https://cloud.google.com/speech-to-text/v2/docs/multiple-languages) can transcribe from a configured set of possible languages and label results with a predicted language code.[Amazon Transcribe](https://docs.aws.amazon.com/transcribe/latest/dg/lang-id-stream.html) supports streaming language identification and multi-language identification, but its docs also call out constraints around language options, dialects, custom language models, and redaction. Those constraints are the reason the repository should preserve the language decision, not just the final text.


## Required Fields for Every Multilingual Turn


Start with the turn record because QA reviewers work at the turn level.


Field Type Why It Matters


` canonicalCallId


` string Joins every language, audio, trace, label, and evaluation record


` turnId


` string Makes one utterance addressable


` speaker


` enum Separates caller, voice agent, IVR, human agent, and system output


` turnStartMs


` /` turnEndMs


` integer Lets reviewers replay the matching audio segment


` nativeText


` text Source transcript in the spoken language


` translatedText


` text or null Review aid for global QA and leadership


` languageCode


` BCP 47 string Enables language cohorts and model routing analysis


` languageConfidence


` number Flags low-confidence language detection before scoring


` sttProvider


` /` sttModel


` string Explains behavior changes after provider or model updates


` translationProvider


` /` glossaryVersion


` string or null Makes translated review text auditable


` redactionState


` enum Prevents broad search over raw sensitive content


` region


` /` residencyClass


` string Keeps storage and access aligned with market rules


` agentVersion


` string Connects failures to prompt, workflow, and model changes


` traceId


` /` spanId


` string Links the turn to logs, tool calls, and latency spans


` audioArtifactId


` string Points to controlled replay


` qaLabel


` /` evaluationResult


` object Connects text to review and regression decisions


This is a repository schema, not a product feature list. You can implement it in relational tables, search documents, a lakehouse, or a hybrid index. The invariant is simpler: every multilingual search result needs text, language, confidence, replay, redaction, and action.


```text
{        "  turnId  "  :     "  turn_0017  "  ,        "  canonicalCallId  "  :     "  call_2026_06_19_0942  "  ,        "  speaker  "  :     "  caller  "  ,        "  turnStartMs  "  :     84210  ,        "  turnEndMs  "  :     91780  ,        "  nativeText  "  :     "  Ya verifiqué mi cuenta, ¿por qué me preguntas otra vez?  "  ,        "  translatedText  "  :     "  I already verified my account. Why are you asking again?  "  ,        "  languageCode  "  :     "  es-US  "  ,        "  languageConfidence  "  :     0  .  82  ,        "  sttProvider  "  :     "  provider_name  "  ,        "  sttModel  "  :     "  multilingual-prod-2026-06  "  ,        "  translationProvider  "  :     "  translation_provider  "  ,        "  glossaryVersion  "  :     "  support_terms_v4  "  ,        "  redactionState  "  :     "  redacted  "  ,        "  region  "  :     "  us  "  ,        "  residencyClass  "  :     "  customer_support_us  "  ,        "  agentVersion  "  :     "  billing-agent@2026-06-19.2  "  ,        "  traceId  "  :     "  4bf92f3577b34da6a3ce929d0e0e4736  "  ,        "  spanId  "  :     "  span_asr_turn_0017  "  ,        "  audioArtifactId  "  :     "  audio_0942_redacted  "  ,        "  qaLabel  "  :     {          "  type  "  :     "  identity_verification_confusion  "  ,          "  source  "  :     "  reviewer  "        }  ,        "  evaluationResult  "  :     {          "  rubricId  "  :     "  billing_identity_v5  "  ,          "  passed  "  :     false  ,          "  failureReason  "  :     "  repeated_identity_check  "        }    }
```


The sample record uses plain text for readability. In production, run redaction before broad indexing and keep raw native text behind stricter access controls when policy requires it.


## Native Text, Translations, and Language Confidence


Do not make English translation the source of truth for every market.


Data Choice Use It For Failure If Misused


Native transcript QA, dispute review, language-specific evaluation, model debugging Reviewers miss language-specific errors if they only inspect translations


Translated transcript global triage, leadership review, cross-region issue clustering Translation can hide tone, entity mistakes, and policy wording


Language confidence routing decisions, review flags, score eligibility Low-confidence calls get scored as if the transcript were reliable


Native audio pointer ASR disputes, accent review, interruption analysis, consent checks Text-only review cannot explain noise, barge-in, or pronunciation failures


Aggregate language metrics dashboards and trend analysis Averages hide one failing language or region


[Azure AI Video Indexer](https://learn.microsoft.com/en-us/azure/azure-video-indexer/transcription-translation-lid-insight) describes language identification, multi-language identification, translation, diarization, and JSON insight output with language fields.[AssemblyAI's language detection docs](https://www.assemblyai.com/docs/pre-recorded-audio/language-detection.md) call out expected-language lists, fallback language, confidence scores, confidence thresholds, and misdetection troubleshooting. The product details differ, but the repository rule is the same: keep the language decision observable.


> **Language confidence rule:** if language confidence is low, the repository should flag the turn before it feeds automated scoring, dashboards, or regression promotion.


That flag matters in code-switching. A caller can start in English, switch to Spanish for an account detail, then return to English. If the transcript pipeline collapses that into one English record, QA may blame the voice agent for a failure that started in language detection.


## Query Cookbook for QA and Analytics


Design the repository around review questions, not storage tables.


Review Question Required Filters Result Should Show


Which Spanish calls failed identity verification after the latest prompt change? language code, agent version, QA label, date range native turns, translated text, audio offsets, evaluation result


Which calls switched languages mid-conversation? per-turn language code, call ID, sequence turn sequence, confidence, replay offsets


Which low-confidence transcripts were still auto-scored? language confidence, evaluation status score, rubric, owner, blocked-report flag


Which translated summaries disagree with native reviewer labels? translation status, reviewer label, language native text, translated text, reviewer decision


Which regions have restricted raw audio access? region, residency class, artifact type audio policy, redaction state, access role


Which production failures should become multilingual regression tests? QA label, evaluation failure, review status source turn, expected behavior, test persona, fixture


This is where the repository connects to[failed production call regression tests](https://hamming.ai/resources/failed-production-call-regression-test-runbook) . A multilingual failure should not become a test case until the team can preserve the native turn, translated aid, language confidence, prompt version, expected behavior, and audio pointer.


For search-index details, use the[voice agent transcript search schema](https://hamming.ai/resources/voice-agent-transcript-search-schema) . This page answers the multilingual repository question. The search schema answers how to index, search, and highlight turns once the records exist.


## Access, Retention, and Residency Gates


Multilingual repositories cross markets, languages, and customer data classes. One global access policy will not survive contact with enterprise review.


Use separate controls for each evidence class:


Evidence Class Default Policy Common Exception


Raw audio controlled pointer, narrow playback role dispute, consent review, ASR investigation


Native transcript redacted search by default native-language QA or compliance review


Translated transcript reviewer aid, not source of truth executive summary or global triage


Language metadata broadly queryable remove user identifiers before export


QA labels and evaluations QA and product analytics customer-specific contract restrictions


Aggregate analytics broadest access after de-identification low-volume cohorts that could re-identify callers


Pair this with the[voice agent log retention checklist](https://hamming.ai/resources/voice-agent-log-retention-compliance-checklist) before launch. The repository should carry region, retention class, redaction state, deletion status, and legal-hold state so the analytics layer does not become an accidental archive.


The honest limitation: zero-retention and full longitudinal analytics fight each other. If policy says a vendor cannot store raw transcripts or recordings, the architecture needs customer-owned storage, push-based ingestion, scoped pointers, and aggregate-only analytics. Do not pretend those tradeoffs disappear.


## Implementation Checklist


Build the repository in this order:


Step Action Evidence to Keep


1. Normalize identity Create one canonical call ID and store provider aliases provider IDs, room IDs, trace IDs


2. Capture language fields Store language code, confidence, provider, model, and fallback behavior per turn language decision record


3. Split native and translated text Keep native text as source; store translation as review aid translation provider and glossary version


4. Attach audio pointers Store replay offsets and controlled artifact IDs audio URI, redaction state, access role


5. Join labels and evaluations Link QA labels, scores, reviewer decisions, and rubric versions evaluation record


6. Apply residency and retention Add region, retention class, deletion status, and legal-hold state policy metadata


7. Gate analytics Block low-confidence or unredacted records from broad dashboards blocked-report reason


8. Promote regressions Turn selected failures into multilingual test cases source turn, persona, expected behavior


Start smaller than feels satisfying: one agent, two languages, one region, and one QA workflow. If the joins work there, expand.


If the first cohort cannot answer "what did the caller say, in which language, with what confidence, and what should we do next," adding more markets will not create clarity. It will just make the repository harder to trust.


## What Not to Centralize


More centralization is not always better.


Avoid putting these into broad query storage:


- Raw audio files.
- Unredacted transcript text.
- Full tool payloads with customer data.
- Secrets, auth headers, or webhook bodies.
- Translations with no native-text pointer.
- Aggregate metrics for cohorts so small they identify a caller.
- Low-confidence language-detection output with no review flag.


Use the[call evidence export runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) when reviewers need portable packets. Use the[PII redaction guide](https://hamming.ai/resources/pii-redaction-voice-agents) before transcript text becomes broadly searchable. Use[multi-tenant dashboard requirements](https://hamming.ai/resources/multi-tenant-voice-agent-analytics-dashboards-bpo) when the same repository feeds client-facing reports.


The repository is supposed to reduce ambiguity. If it makes raw multilingual call data easier to over-share, the architecture is moving in the wrong direction.
