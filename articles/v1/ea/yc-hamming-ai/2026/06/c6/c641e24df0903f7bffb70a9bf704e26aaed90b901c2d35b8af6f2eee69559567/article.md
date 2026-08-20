---
schema_version: "1.0.0"
document_id: "c641e24df0903f7bffb70a9bf704e26aaed90b901c2d35b8af6f2eee69559567"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/voice-agent-structured-output-validation-checklist"
published_at: "2026-06-07T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:15910587dfe1e96176bdddbe5a4c27e39ab2a96cd2eeacdbba1e32cb3a71abb2"
---

# Voice Agent Structured Output Validation Checklist

**Voice agent structured output validation** is the process of proving that extracted JSON fields, tool arguments, intent labels, and post-call analysis results are supported by what the caller actually said.


If the output is only used for a rough internal summary, a lightweight review may be enough. This checklist is for teams that send structured call data into CRMs, QA dashboards, compliance queues, test cases, routing decisions, or workflow automations.


The failure mode is subtle: the JSON parses, the schema passes, and the dashboard looks clean. Then a date, account ID, medication name, refund amount, or callback intent is wrong because the extractor normalized the wrong phrase.


We found this shows up most often when teams treat the extractor as the source of truth instead of treating it as one more system that needs evidence.


> **TL;DR:** Validate voice agent structured outputs in 4 layers: schema, evidence, normalization, and action safety. A field should not drive a tool call, customer record, compliance decision, or metric until it is tied to transcript or audio evidence and the ambiguity policy is clear.


> **Methodology Note:** This checklist is based on Hamming's analysis of production voice agent calls where extracted fields, tool arguments, and post-call labels changed downstream workflows across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> Treat structured output validation as a workflow control, not a formatting check. The highest-risk failures are usually schema-valid and semantically wrong.


**Last Updated:** June 2026


**Related Guides:**


- [Voice Agent Workflow Testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - assert tool calls, state transitions, and side effects
- [Voice Agent Sandbox Testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) - prove tool calls without touching production data
- [Voice Agent Transcript Search Schema](https://hamming.ai/resources/voice-agent-transcript-search-schema) - store searchable transcript evidence
- [Voice Agent Call Evidence Export Runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - package call evidence for review
- [Voice Agent Tests as Code](https://hamming.ai/resources/voice-agent-tests-as-code-template) - put guardrails and fixtures in Git
- [Voice Agent Caller Identity Testing](https://hamming.ai/resources/voice-agent-caller-identity-testing-checklist) - decide which identity fields are trusted
- [Voice Agent CI/CD Testing](https://hamming.ai/resources/voice-agent-testing-ci-cd-regression-load-security) - gate risky changes before release
- [OpenTelemetry for Voice Agents](https://hamming.ai/resources/opentelemetry-voice-agents-tracing-guide) - connect extraction failures to traces
- [Voice Agent Response Coverage](https://hamming.ai/resources/voice-agent-response-coverage) - convert production gaps into tests
- [WebSocket Voice Agent Testing](https://hamming.ai/resources/websocket-voice-agent-testing-guide) - validate event streams and tool calls without PSTN


## Do Not Confuse Schema Validity With Caller Truth


[OpenAI's Structured Outputs documentation](https://developers.openai.com/api/docs/guides/structured-outputs) explains that Structured Outputs can make model responses adhere to a supplied JSON Schema. That is useful. It does not prove the fields are true.


Valid JSON means the parser can read it. Schema-valid output means the fields match the contract. Caller-supported output means each meaningful field can be traced back to the call evidence.


Level What It Proves What It Does Not Prove Sample Failure


Valid JSON The output parses. Required fields, enums, and types are correct.` "


appointment_date


"


:


"


next Friday


"


` parses but is not normalized.


Schema-valid Required fields and types match the schema. The extracted value was said by the caller.` "


refund_amount


"


:


50


` fits the schema but the caller said $15.


Caller-supported The value is tied to transcript or audio evidence. The value is safe to write downstream. Caller mentioned "John" but identity lookup found 3 matching records.


Action-safe Evidence, confidence, policy, and destination are all acceptable. The whole workflow succeeded. Extracted callback time is safe, but SMS delivery fails later.


> **Structured output validation rule:** do not let a field cross into a tool call, CRM write, compliance queue, or KPI until it passes schema validation, caller-evidence validation, normalization validation, and action-safety validation.


This is the same reason[voice agent workflow testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) cannot stop at "the agent sounded correct." A model can produce a valid object that still misrepresents the caller.


## The Validation Contract


Start with a contract that describes how each field earns trust.


```text
structured_output_validation     =        schema_version        +     source_call_id        +     transcript_or_audio_span        +     extraction_prompt_or_tool_version        +     field_value        +     normalized_value        +     confidence        +     ambiguity_policy        +     downstream_action_policy
```


Use this as the minimum review table.


Field Required? Sample Why It Matters


` schema_version


` Yes` booking_intake_v3


` Prevents old extractors from feeding new workflows.


` source_call_id


` Yes` call_2026_06_07_1042


` Lets reviewers replay the evidence.


` evidence_span


` Yes` turn_12


:


chars


40


-


78


` or audio` 00


:


03


:


12


-


00


:


03


:


18


` Shows where the value came from.


` raw_utterance


` Usually "Friday after 2" Keeps the original caller words.


` normalized_value


` Conditional` 2026


-


06


-


12T14


:


00


:


00


-


05


:


00


` Makes downstream systems deterministic.


` confidence


` Yes` 0


.


82


` or` high


` Separates strong evidence from guesswork.


` review_status


` Yes for high-risk fields` auto_pass


` ,` needs_review


` ,` rejected


` Blocks writes when evidence is weak.


` action_policy


` Yes` may_write_sandbox


` ,` human_review_required


` Keeps extraction from becoming an unsafe side effect.


[Vapi's structured outputs documentation](https://docs.vapi.ai/assistants/structured-outputs-quickstart) describes processing complete call context, transcripts, messages, tool results, and metadata before delivering schema-shaped results. That is the right direction. The validation layer still has to decide whether each result should be trusted.


## Field-Level Checklist


Not every field deserves the same scrutiny. Tags and summaries can tolerate more ambiguity than dates, amounts, identity fields, and tool arguments.


Field Type Validate Against Block When Safer Fallback


Caller name Caller utterance, identity lookup, spelling confirmation Name conflicts with trusted profile or only appears in ASR noise Mark as unverified and ask for confirmation.


Phone or account ID Trusted lookup, masked transcript, caller confirmation Value came only from model inference Use backend identity record instead.


Date and time Transcript span, timezone, business calendar Relative date cannot be resolved unambiguously Store raw phrase and request confirmation.


Money amount Transcript span, currency, decimal normalization Amount differs between transcript and tool argument Route to review before writing.


Address Transcript span, geocoder result, required fields Missing unit, city, postal code, or country when required Ask a clarifying question.


Intent label Transcript evidence, allowed label set, confidence Label is inferred from agent summary only Keep as candidate label.


Sentiment or CSAT Caller words, explicit score, evaluator rationale The caller never provided enough evidence Mark as unavailable.


Tool arguments Tool schema, evidence span, preconditions Required argument was invented or copied from stale context Fail the workflow test.


Summary Transcript coverage, redaction policy Summary includes unsupported facts or sensitive details Regenerate from redacted evidence.


For transcript entity extraction,[AssemblyAI documents](https://www.assemblyai.com/docs/speech-understanding/detect-entities-in-transcript) entity records with text, entity type, and start/end timestamps. Timestamps are useful because they let a reviewer jump from the extracted field to the precise evidence span instead of reading the whole call.


## A Copyable Schema for Evidence-Aware Extraction


Use a schema that forces evidence, not just values.


```text
{        "  type  "  :     "  object  "  ,        "  additionalProperties  "  :     false  ,        "  required  "  :     [  "  callId  "  ,     "  schemaVersion  "  ,     "  fields  "  ,     "  overallStatus  "  ]  ,        "  properties  "  :     {          "  callId  "  :     {     "  type  "  :     "  string  "     }  ,          "  schemaVersion  "  :     {     "  type  "  :     "  string  "     }  ,          "  overallStatus  "  :     {            "  type  "  :     "  string  "  ,            "  enum  "  :     [  "  auto_pass  "  ,     "  needs_review  "  ,     "  rejected  "  ]          }  ,          "  fields  "  :     {            "  type  "  :     "  array  "  ,            "  items  "  :     {              "  type  "  :     "  object  "  ,              "  additionalProperties  "  :     false  ,              "  required  "  :     [                "  name  "  ,                "  rawValue  "  ,                "  normalizedValue  "  ,                "  evidenceSpan  "  ,                "  confidence  "  ,                "  actionPolicy  "              ]  ,              "  properties  "  :     {                "  name  "  :     {     "  type  "  :     "  string  "     }  ,                "  rawValue  "  :     {     "  type  "  :     "  string  "     }  ,                "  normalizedValue  "  :     {     "  type  "  :     [  "  string  "  ,     "  number  "  ,     "  boolean  "  ,     "  null  "  ]     }  ,                "  evidenceSpan  "  :     {     "  type  "  :     "  string  "     }  ,                "  confidence  "  :     {     "  type  "  :     "  number  "  ,     "  minimum  "  :     0  ,     "  maximum  "  :     1     }  ,                "  actionPolicy  "  :     {                  "  type  "  :     "  string  "  ,                  "  enum  "  :     [  "  read_only  "  ,     "  human_review_required  "  ,     "  may_write_sandbox  "  ,     "  may_write_production  "  ]                }              }            }          }        }    }
```


[OpenAI's function-calling guide](https://developers.openai.com/api/docs/guides/function-calling) recommends strict mode for function schemas and documents requirements such as` additionalProperties


:


false


` and required fields. Use those constraints for shape. Then add your own evidence requirements for truth.


> **Evidence-first schema rule:** every high-risk extracted field should carry its raw value, normalized value, evidence span, confidence, and action policy. A value without provenance is a note, not a workflow input.


## Normalize Only After Preserving What Was Said


Normalization is where many errors become permanent. Once "Friday after 2" becomes a timestamp, the original uncertainty can disappear.


Caller Phrase Bad Normalization Better Output


"Friday after 2"` 2026


-


06


-


12T14


:


00


:


00Z


` Raw phrase plus local timezone, candidate slots, and confirmation required.


"fifteen dollars"` 50


` Raw phrase, parsed amount` 15


.


00


` , currency, and transcript span.


"my son John"` caller_name


:


John


` Relationship field plus dependent name, not caller identity.


"cancel the second one"` cancel_all


:


true


` Referenced appointment ID only after listing candidates.


"same address as last time" Full address copied from stale profile Trusted profile pointer plus confirmation status.


I used to think structured outputs were mostly a schema problem. They are not. In voice, the hard part is preserving uncertainty long enough for the workflow to handle it safely.


That matters for[caller identity testing](https://hamming.ai/resources/voice-agent-caller-identity-testing-checklist) . A trusted backend field and a caller-spoken field are different evidence types. Do not merge them just because both can fit into one JSON object.


## Gate Tool Calls and Side Effects


Structured outputs often become tool arguments. That is where validation stops being editorial and starts being operational.


Before a field can drive a tool call or side effect, require these checks:


Gate Pass Condition Fail Action


Schema gate Output matches the schema version expected by the workflow. Reject and retry extraction or route to review.


Evidence gate Field has transcript or audio support. Mark field unavailable.


Ambiguity gate The field is unambiguous for the action. Ask a clarifying question.


Trust gate Identity and permission fields come from approved sources. Block sensitive action.


Destination gate The target system is sandbox, test, or explicitly allowlisted. Do not write.


Idempotency gate The action has a dedupe key. Block retries that can duplicate records.


Audit gate Call ID, field evidence, tool request, and result are stored. Treat as failed evidence.


Pair this with[voice agent sandbox testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) . A structured output can be correct and still unsafe to write to production if the environment, idempotency key, or cleanup rule is missing.


## CI Regression Sample


Put the risky cases in CI, not just in a dashboard.


```text
id  :     callback_time_extraction_ambiguous_date    risk  :     blocking    agent_ref  :        environment  :     staging        prompt_version  :     pr  -  842    input  :        call_fixture  :     fixtures  /  calls  /  callback_next_friday_after_two  .  wav    expected_structured_output  :        schema_version  :     callback_request_v2        fields  :          -     name  :     callback_window            raw_value_must_include  :     "  Friday after 2  "            normalized_value_status  :     needs_confirmation            evidence_span_required  :     true            action_policy  :     human_review_required    forbidden  :        -     create_production_callback        -     write_crm_task_without_confirmation    evidence  :        retain_audio  :     true        retain_transcript  :     true        retain_structured_output  :     true        retain_tool_trace  :     true
```


This belongs next to your[voice agent tests as code](https://hamming.ai/resources/voice-agent-tests-as-code-template) . If a prompt change makes the extractor confidently choose the wrong date, the PR should fail before a customer record changes.


## What This Checklist Cannot Prove


Structured output validation does not prove the whole voice agent worked.


It does not prove the caller was satisfied, the audio was clear, the tool completed, the downstream record synced, or the business policy was right. It proves a narrower thing: the structured fields you plan to trust are shaped correctly, supported by evidence, normalized safely, and gated before action.


Use[call evidence exports](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) when a reviewer needs the whole packet. Use[transcript search schema](https://hamming.ai/resources/voice-agent-transcript-search-schema) when teams need to find similar failures across calls. Use[response coverage](https://hamming.ai/resources/voice-agent-response-coverage) when repeated extraction misses should become new test cases.


## Final Review Checklist


Before trusting voice agent structured outputs, check:


- The output matches a versioned schema.
- Every high-risk field has transcript or audio evidence.
- Raw caller words are preserved before normalization.
- Dates, amounts, IDs, names, and addresses have ambiguity rules.
- Caller identity fields are separated from caller-spoken fields.
- Tool arguments are validated before tool execution.
- Production writes require stronger evidence than dashboard labels.
- Redaction and storage policy are set before exporting evidence.
- CI contains at least 5 regression cases for ambiguous fields.
- Failed extractions are added to the test suite, not just fixed once.
