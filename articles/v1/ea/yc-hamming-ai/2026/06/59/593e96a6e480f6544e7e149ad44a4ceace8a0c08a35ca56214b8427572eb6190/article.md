---
schema_version: "1.0.0"
document_id: "593e96a6e480f6544e7e149ad44a4ceace8a0c08a35ca56214b8427572eb6190"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/genesys-asterisk-voice-agent-testing-runbook"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:23:18.688239+00:00"
content_hash: "sha256:8696be5d19088ca5abcbb72a453a4b743033652ffcab3133f8ec812cd9127978"
---

# Genesys and Asterisk Voice Agent Testing: Enterprise Telephony QA Runbook

Genesys and Asterisk voice agent testing breaks when teams treat the voice agent like a standalone chatbot. The AI response can look right while the real call path drops DTMF, loses caller context, records the wrong channel, or hands off to the wrong queue.


If your agent only runs through a WebSocket endpoint, start with[WebSocket voice agent testing](https://hamming.ai/resources/websocket-voice-agent-testing-guide) . If your agent runs through Genesys, BYOC trunks, SIP routing, or on-prem Asterisk middleware, the phone path is part of the product. Test it as one system.


> **Genesys and Asterisk voice agent testing** is the process of validating the full enterprise call path: SIP setup, IVR routing, Asterisk channel control, media quality, AI agent behavior, transfers, recordings, transcripts, traces, and QA outcomes.


**Quick filter:** if a failed test cannot answer "which Genesys interaction, SIP Call-ID, Asterisk channel, transcript, recording, and trace belong to this call?", the test is not production-grade yet.


> **TL;DR:** Build the test runbook around 5 layers:
>
>
> - **Telephony:** SIP trunk, BYOC, carrier, codec, DTMF, media, and failover behavior.
> - **Contact center:** Genesys route, IVR flow, queue, transfer, and interaction metadata.
> - **Asterisk middleware:** dialplan, ARI/AMI events, channel IDs, recordings, and simulator behavior.
> - **Voice agent:** ASR, prompt version, tool calls, TTS, interruptions, and fallback handling.
> - **QA evidence:** recording pointer, transcript, trace ID, evaluation result, and regression decision.
>
>
> Do not release on "the call sounded fine." Release when the evidence proves the call path, agent behavior, and business outcome all matched the test.


> **Methodology Note:** This runbook is based on Hamming's analysis of production voice agent testing and enterprise call-review workflows across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> It also uses public Genesys, Asterisk, and SIP trunking documentation to ground the telephony-specific evidence requirements.


Across Hamming's 10M+ mins protected and 10K+ voice agents, we found that enterprise failures are rarely isolated to one layer. One artifact says the caller asked for billing. Another says the call transferred. A third says the task failed. The test has to join those facts before anyone can trust the result.


**Last Updated:** June 2026


**Related Guides:**


- [Call Center Voice Agent Testing](https://hamming.ai/resources/call-center-voice-agent-testing-guide) - broader call-center QA program design
- [IVR and Voice Agent Log Correlation](https://hamming.ai/resources/ivr-voice-agent-log-correlation-runbook) - canonical call IDs across IVR, telephony, transcripts, and outcomes
- [Voice Agent Workflow Testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - tool-call, state, and side-effect guardrails
- [Voice Agent Call Evidence Export](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - reviewer-safe packets for transcripts, audio, traces, and QA results
- [OpenTelemetry for Voice Agents](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) - span and event modeling for ASR, LLM, tools, and TTS
- [Voice Agent Incident Response Runbook](https://hamming.ai/resources/voice-agent-incident-response-runbook) - production triage when calls start failing
- [Voice Agent Log Retention Compliance Checklist](https://hamming.ai/resources/voice-agent-log-retention-compliance-checklist) - retention policy for recordings and transcripts
- [Testing Voice Agents for Production Reliability](https://hamming.ai/resources/testing-voice-agents-production-reliability) - release gates and regression policy


## What Makes Genesys and Asterisk Voice Agent Testing Different?


In a Genesys or Asterisk deployment, the AI agent is only one participant in a longer telephony system. A test can pass at the model layer and still fail in production because the route, channel, recording, transfer, or media path behaved differently.


The most common mistake is testing a prompt in isolation, then assuming the same behavior will hold inside the contact-center call path.


Layer What Can Fail Evidence You Need


SIP and BYOC INVITE failures, bad allowlists, retry/failover mismatch, codec negotiation, one-way audio SIP Call-ID, response codes, PCAP, codec, packet loss, jitter


Genesys wrong route, missing contact attributes, queue mismatch, transfer loop, bot flow branch drift interaction ID, flow name, queue, DNIS/ANI policy, transfer reason


Asterisk wrong dialplan branch, channel not entering ARI, recording missing, DTMF mismatch channel unique ID, Stasis events, AMI/ARI events, recording file


Voice agent ASR miss, prompt regression, tool-call failure, latency, hallucination, bad fallback transcript, prompt version, tool trace, latency, evaluation score


QA workflow reviewer cannot replay the call or prove the failure canonical call ID, recording pointer, trace ID, packet manifest


[Genesys SIP Diagnostics](https://help.mypurecloud.com/articles/sip-diagnostics/) documents why SIP evidence matters for BYOC Cloud and BYOC Premises: PCAPs can show signaling history for a specific call across external trunks such as BYOC Cloud Carrier, PBX trunks, and premises Edge external trunks, but PCAP generation is best-effort and the files are available only for a limited window. BYOC Premises also needs conversation headers enabled on the trunk so captures can be queried by conversation ID. These are operational constraints, not footnotes. If the test runner waits too long to collect evidence, the call may no longer be debuggable.


[Asterisk ARI channel docs](https://docs.asterisk.org/Configuration/Interfaces/Asterisk-REST-Interface-ARI/Introduction-to-ARI-and-Channels/) describe channels as the path between an endpoint and Asterisk, with a channel unique ID and events such as` StasisStart


` and` StasisEnd


` . Those IDs are not marketing details. They are the keys that let a test connect Asterisk behavior to Genesys and voice-agent evidence.


> **Enterprise voice-agent test:** a call scenario that proves the caller path, contact-center route, middleware behavior, AI decision, media artifact, and final outcome agree under one canonical call record.


## Which Topology Are You Testing?


Do not start with test scripts. Start with topology. The same phrase, "Genesys plus Asterisk," can mean at least 5 different systems.


Topology Asterisk Role Genesys Role Best First Test Watch For


Genesys Cloud voice only none telephony, routing, bot or handoff Genesys test route and interaction evidence voice-agent layer hidden behind platform abstractions


Genesys BYOC to carrier or PBX downstream SIP target routing and media edge inbound and outbound trunk call with PCAP and route evidence allowlist, retry codes, media region, TLS/UDP/TCP mismatch


Asterisk as test caller controlled simulator system under test deterministic inbound calls through test DNIS simulator masking real carrier behavior


Asterisk as middleware bridge media/control bridge upstream or downstream route bridge call with recording and channel correlation lost IDs, wrong channel recording, DTMF conversion


Hybrid lab simulator plus limited production mirror isolated test route staged happy path, fallback, transfer, and load slice test route drifting from production route


[Genesys BYOC documentation](https://help.genesys.cloud/faqs/category/byoc-cloud/) says BYOC Cloud trunks can connect Genesys Cloud to SIP-compliant carriers or devices reachable over the public internet, and supports UDP, TCP, and TLS. The same docs call out default retry and failover behavior for certain SIP codes. Your test plan needs to prove those rules match your environment, not just that one phone call connected.


For carrier-side references,[Twilio Elastic SIP Trunking](https://www.twilio.com/docs/sip-trunking) shows the operational shape many teams need to test: origination URIs, multiple SIP URIs for failover, edge location, recording, caller ID, firewall allowlists, and SIP OPTIONS behavior. Even if Twilio is not in your stack, the test categories are useful.


**Topology rule:** name the system under test in every scenario. "Asterisk test" does not tell a reviewer enough. "Asterisk-originated inbound call through Genesys BYOC route to billing voice agent" gives the test owner something they can replay.


## What Scenarios Should the Test Harness Run?


Start with a small scenario matrix and make it boring before adding scale. A 200-call batch with unclear guardrails creates more noise than a 12-call suite with specific artifacts.


Scenario Setup Expected Result Release Blocker


Happy path Inbound caller reaches test route and completes one known task correct route, transcript, tool result, recording, QA pass any layer cannot be joined by call ID


DTMF path Caller presses expected menu digits during IVR or agent prompt digit captured and routed correctly DTMF lost, duplicated, or treated as speech only


Transfer to queue Agent transfers caller to human or queue transfer reason and destination match policy transfer loop, wrong queue, lost caller context


Fallback path Backend API fails or agent cannot answer safe fallback, escalation, or retry policy runs agent invents answer or hangs silently


Caller interruption Caller corrects the agent mid-response agent stops, accepts correction, preserves task state caller repeats or agent continues old path


Silence timeout Caller does not respond after prompt reprompt or escalation follows policy timeout hidden as latency or ignored


No-answer or busy downstream destination cannot answer route follows fallback policy call drops without useful reason


QoS degradation controlled jitter, packet loss, or codec mismatch in lab test records quality degradation and voice-agent impact transcript only shows failure with no media evidence


Recording and replay call should be reviewable after test recording pointer, transcript, trace, and score open together recording missing, wrong channel, or unsafe access


The Asterisk side should be explicit about whether it is generating calls, controlling channels, or only recording media.[Asterisk recording docs](https://docs.asterisk.org/Configuration/Interfaces/Asterisk-REST-Interface-ARI/Introduction-to-ARI-and-Media-Manipulation/ARI-and-Media-Part-1-Recording/) distinguish live recordings from stored recordings.[MixMonitor](https://docs.asterisk.org/Latest_API/API_Documentation/Dialplan_Applications/MixMonitor/) can record mixed or separated audio streams, but your QA result should know which channel was recorded.


Here is the minimum test case shape:


```text
{        "  testId  "  :     "  genesys_billing_dtmf_transfer_001  "  ,        "  topology  "  :     "  asterisk-originated-call-to-genesys-byoc  "  ,        "  callerFixture  "  :     "  billing_customer_known_account  "  ,        "  entryRoute  "  :     {          "  dialedNumber  "  :     "  +15551230000  "  ,          "  genesysFlow  "  :     "  billing-support-test  "  ,          "  asteriskContext  "  :     "  hamming_test_calls  "        }  ,        "  expectedPath  "  :     [          "  call.answered  "  ,          "  ivr.prompt_played  "  ,          "  dtmf.received  "  ,          "  agent.session_started  "  ,          "  tool.called  "  ,          "  call.transferred  "        ]  ,        "  guardrails  "  :     {          "  dtmfDigit  "  :     "  2  "  ,          "  targetQueue  "  :     "  billing_escalation_test  "  ,          "  taskOutcome  "  :     "  transferred_with_context  "  ,          "  maxP95TurnLatencyMs  "  :     3000        }    }
```


The important part is not the particular field names. The important part is that the test names the route, caller fixture, expected events, and evidence required to prove the result.


## What Evidence Should Every Test Capture?


Every test needs an evidence contract. Without it, QA ends up with a call recording, engineering ends up with logs, telephony ends up with PCAP, and nobody can prove they are looking at the same call.


Evidence Required? Source Why It Matters


canonical call ID yes your test runner or orchestration layer joins every artifact under one identity


Genesys interaction or conversation ID yes Genesys proves route, flow, queue, and transfer behavior


SIP Call-ID and response codes yes for SIP/BYOC tests SIP trace or PCAP proves setup, retry, failover, and disconnect path


Asterisk channel unique ID yes for Asterisk tests ARI/AMI/channel logs joins dialplan, bridge, recording, and channel events


recording pointer usually Genesys, Asterisk, carrier, or storage lets reviewers hear DTMF, silence, clipping, and interruptions


transcript and turn IDs yes voice-agent runtime or ASR proves what the agent heard and said


prompt and agent version yes voice-agent runtime ties behavior to shipped configuration


trace ID yes for engineering QA OpenTelemetry or app tracing joins ASR, LLM, tools, TTS, and storage spans


evaluation result yes Hamming or QA system gives pass/fail reason and regression label


redaction state yes evidence/export layer protects recordings, transcripts, and account data


For the full identity model, use the[IVR and voice agent log correlation runbook](https://hamming.ai/resources/ivr-voice-agent-log-correlation-runbook) . For reviewer packaging, use the[call evidence export runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) .


> **Evidence contract:** a test is not complete until it can produce the IDs, recordings, transcripts, traces, and QA result needed to replay the call without a manual hunt through every production system.


Use a normalized event envelope so QA and engineering can search across providers:


```text
{        "  canonicalCallId  "  :     "  call_2026_06_17_001  "  ,        "  eventName  "  :     "  telephony.transfer.completed  "  ,        "  occurredAt  "  :     "  2026-06-17T15:42:18.231Z  "  ,        "  sourceSystem  "  :     "  genesys  "  ,        "  providerAliases  "  :     {          "  genesysInteractionId  "  :     "  interaction_test_001  "  ,          "  sipCallId  "  :     "  a84b4c76e66710@sample.org  "  ,          "  asteriskChannelId  "  :     "  asterisk01-1781710938.12  "  ,          "  traceId  "  :     "  4bf92f3577b34da6a3ce929d0e0e4736  "        }  ,        "  payload  "  :     {          "  fromRoute  "  :     "  billing_voice_agent_test  "  ,          "  toQueue  "  :     "  billing_escalation_test  "  ,          "  transferReason  "  :     "  caller_requested_human  "  ,          "  recordingPointer  "  :     "  s3://qa-evidence/call_001/audio.wav  "        }  ,        "  qa  "  :     {          "  evaluationId  "  :     "  eval_001  "  ,          "  result  "  :     "  pass  "  ,          "  regressionCandidate  "  :     false        }    }
```


Keep the payload small enough to search and safe enough to share. Put raw audio, full transcripts, and account-specific data behind the right access controls.


## How Do You Automate Asterisk and Genesys Test Runs?


Automation should make the test repeatable, not hide the call path. Treat Asterisk as a controlled participant and Genesys as the routing system under test.


Step Action Output Common Mistake


1. Freeze topology name trunk, route, test number, Asterisk context, and agent version topology record "testing Genesys" without a concrete route


2. Generate call originate a controlled call or replay a known fixture call attempt ID using production caller identity by accident


3. Drive scenario play audio, send DTMF, pause, interrupt, transfer, or hang up event sequence only testing one happy path


4. Capture artifacts pull Genesys, SIP, Asterisk, transcript, recording, and trace evidence evidence packet collecting artifacts after retention window


5. Score behavior evaluate route, transcript, latency, tool result, and outcome QA result pass/fail based only on transcript


6. Promote failures turn repeatable failures into regression tests test fixture leaving incident evidence as a one-off note


Asterisk can be a useful simulator because it can originate calls, expose channel events, manipulate media, and record audio. That does not make it a complete substitute for real carrier traffic. We used to think a lab route was enough if the agent logic passed. After watching voice-agent failures move across telephony, IVR, and model layers, I now treat lab calls as the first gate, not the last gate.


For enterprise launches, use 3 rings of confidence:


1. **Transport gate:** WebSocket or direct endpoint test proves the voice-agent runtime can accept audio and emit events.
2. **Telephony lab gate:** Asterisk or SIP simulator proves the Genesys route, IVR, DTMF, transfer, recording, and agent behavior.
3. **Production mirror gate:** limited test numbers or controlled cohorts prove the same evidence contract survives real routing and media constraints.


The[voice agent workflow testing runbook](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) covers business guardrails. This runbook adds the enterprise telephony boundary: the workflow is not proven unless the route and evidence are proven too.


## What Release Gates Should Block Launch?


Block launch on missing evidence, not just bad outcomes. A test that fails with good evidence is fixable. A test that passes without replayable evidence is fragile.


Gate Pass Condition Block Launch When


Route gate test DNIS/ANI, flow, queue, and transfer match expected path wrong queue, hidden fallback, or transfer loop


SIP gate setup, retry/failover, and disconnect reason are visible SIP Call-ID, response code, or PCAP evidence missing for a SIP failure


Media gate audio is bidirectional and recording matches policy one-way audio, wrong channel, clipping, or missing recording


DTMF gate expected digits are captured and routed correctly DTMF lost, duplicated, delayed, or interpreted as speech only


Agent gate transcript, prompt version, tool calls, and TTS are traceable model response cannot be connected to call evidence


QA gate evaluation result names the failing guardrail pass/fail has no reason, rubric, or artifact pointer


Privacy gate transcript and audio redaction state is explicit raw call artifacts leak into broad review paths


Regression gate repeatable failures become fixtures production failure remains a screenshot or anecdote


This is where[production reliability testing](https://hamming.ai/resources/testing-voice-agents-production-reliability) and[incident response](https://hamming.ai/resources/voice-agent-incident-response-runbook) connect. The release gate should tell the on-call engineer what evidence will exist when the same failure appears at 3am.


One unresolved tension: production-like telephony tests cost more than endpoint tests. They use real routes, numbers, SIP infrastructure, recordings, and sometimes carrier minutes. Do not run them for every tiny prompt edit. Use fast endpoint tests first, then run the enterprise telephony suite for changes that touch routing, caller identity, DTMF, transfers, recordings, latency, or regulated workflows.


## How Hamming Fits Into the Enterprise Telephony QA Loop


Hamming does not need to be your Genesys administrator or your Asterisk PBX. Hamming fits as the independent QA and evidence layer around the voice-agent behavior.


Use Hamming to:


- Generate or replay realistic caller scenarios before a route goes live.
- Evaluate transcripts, audio, latency, tool calls, and policy adherence after each test call.
- Attach QA results to call IDs, recordings, traces, and workflow evidence.
- Find the few production calls worth reviewing instead of sampling blindly.
- Promote failed Genesys, SIP, or Asterisk call patterns into repeatable regression tests.
- Compare agent versions after prompt, model, ASR, TTS, route, or middleware changes.


The practical loop is:


```text
define     topology    run     controlled     call    capture     telephony     +     contact  -  center     +     agent     evidence    score     the     call    review     failures    promote     repeatable     failures     into     regression     tests    rerun     before     the     next     route  ,     prompt  ,     or     provider     change
```


If you already have a Genesys or Asterisk stack, keep it. The goal is not to replace your telephony system. The goal is to make sure your AI voice agent is tested with the same call path, evidence, and failure modes that real callers use.


## Launch Checklist


Before shipping a Genesys or Asterisk voice agent path, verify:


- The topology under test is named and versioned.
- Test numbers, routes, queues, and Asterisk contexts are isolated from production callers.
- Every test creates one canonical call ID.
- Genesys interaction IDs, SIP Call-IDs, Asterisk channel IDs, recordings, transcripts, and traces are joined.
- DTMF, transfer, silence, interruption, fallback, and no-answer scenarios are covered.
- Recordings identify channel policy: mixed, caller-only, agent-only, or dual channel.
- The QA result names the failing guardrail and links to evidence.
- Raw audio and transcripts follow the retention and redaction policy.
- At least 1 production-mirror call validates the route before launch.
- Repeatable failures become regression tests before the next release.


Do the boring part first. Make the call path replayable. Once the evidence is joined, the AI failures become much easier to fix.
