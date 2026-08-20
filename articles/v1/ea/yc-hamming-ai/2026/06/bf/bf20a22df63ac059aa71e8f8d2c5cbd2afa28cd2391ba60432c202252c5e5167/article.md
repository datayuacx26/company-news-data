---
schema_version: "1.0.0"
document_id: "bf20a22df63ac059aa71e8f8d2c5cbd2afa28cd2391ba60432c202252c5e5167"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/voice-agent-prompt-injection-testing-guide"
published_at: "2026-06-18T00:00:00+00:00"
first_seen_at: "2026-07-24T10:35:18.916983+00:00"
fetched_at: "2026-07-28T21:23:18.688239+00:00"
content_hash: "sha256:a6321db023d1d7004d721f6359faec4c5a46f07c18b474744d48394dc5766212"
---

# Voice Agent Prompt Injection Testing: A Security Runbook for Phone Calls

**Voice agent prompt injection testing** is the security practice of proving that a caller cannot use spoken instructions, multi-turn pressure, retrieved context, or tool requests to override the agent's policy.


If your agent only answers public FAQ questions and cannot touch accounts, payments, healthcare data, calendars, refunds, or internal tools, basic LLM security hygiene may be enough. This runbook is for production voice agents with real users, real data, and actions that can harm a customer or the business.


The failure mode is the clean transcript trap. The text transcript may look harmless after speech-to-text normalizes the call. The actual call may have included pauses, authority claims, repeated pressure, background speech, or ambiguity that changed what the agent did.


> **TL;DR:** Test voice agent prompt injection across 6 layers: spoken input, transcript normalization, context boundaries, tool permissions, output policy, and evidence logging. A safe voice agent does not need perfect injection detection. It needs enough guardrails that a successful injection cannot disclose sensitive data, expand tool permissions, or bypass required workflow steps.


> **Methodology Note:** This runbook is based on Hamming's analysis of production voice agent calls, security review workflows, and adversarial test scenarios across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> Use the examples here only for defensive testing on systems you own or are authorized to assess. Calibrate thresholds to your risk level, caller population, tool access, and regulated-workflow scope.


**Last Updated:** June 2026


**Related Guides:**


- [Voice Agent Security Review Questions](https://hamming.ai/resources/voice-agent-security-review-questions) - vendor and POC security questions before call evidence enters a platform
- [Voice Agent Production Readiness Checklist](https://hamming.ai/resources/voice-agent-production-readiness-checklist) - launch gates for functional, scale, security, monitoring, and rollback readiness
- [PII Redaction for Voice Agents](https://hamming.ai/resources/pii-redaction-voice-agents-compliance-architecture-guide) - how to protect transcripts, audio, and evidence archives
- [Voice Agent Workflow Testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - tool-call, state-transition, and side-effect guardrails
- [Voice Agent Sandbox Testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) - prove tool behavior without touching production systems
- [Voice Agent CI/CD Testing](https://hamming.ai/resources/voice-agent-testing-ci-cd-regression-load-security) - make security regression tests run before release
- [Voice Agent Incident Response Runbook](https://hamming.ai/resources/voice-agent-incident-response-runbook) - respond when unsafe behavior reaches production


## What is voice agent prompt injection?


Voice agent prompt injection is an attempt to make an LLM-powered phone agent treat caller-controlled content as higher-priority instructions. The caller may try to change the agent's role, reveal hidden instructions, access sensitive context, or trigger a tool action that the caller is not authorized to request.


> **Definition:** Prompt injection in a voice agent happens when spoken input, transcribed text, retrieved content, or tool output influences the model to violate its intended policy or workflow. Voice makes this harder because the security boundary starts before text exists.


[OWASP's 2025 LLM guidance](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) defines prompt injection as input that alters model behavior or output in unintended ways. OWASP also calls out direct and indirect injection, sensitive information disclosure, unauthorized functions, and manipulation of critical decisions. On a phone line, the model may sit next to caller identity, a CRM record, a scheduling system, a payment flow, or an escalation queue. That is where a bad answer becomes a bad action.


A keyword filter is the part that feels comforting in a review deck. It is also the part attackers route around first. The behavior to inspect is messier: pressure over several turns, uncertain transcription, tool access, sensitive context, and what the agent says after it gets confused.


## Why phone calls need different tests than chat


A typed jailbreak test never has hold music bleeding into the microphone, a caller talking over the agent, or a second person coaching from the room. Text-only prompt injection tests are still useful, but they miss several voice-specific failure modes.


Voice-specific risk What can go wrong Test evidence to save


ASR ambiguity The transcript hides uncertainty or turns a messy phrase into confident text. Audio clip, interim transcript, final transcript, ASR confidence, agent response


Multi-turn pressure The caller slowly moves from a valid task into an unsafe request. Full conversation replay and turn-level policy decisions


Authority claims Caller pretends to be an admin, supervisor, clinician, auditor, or account owner. Identity state, verification step, refusal or escalation


Background speech Another voice or played audio injects instructions into the conversation. Audio channel, speaker attribution if available, agent decision


Tool-action pressure Caller tries to make the agent refund, book, delete, export, transfer, or reveal data. Tool call, parameters, authorization check, result


Retrieved context CRM notes, prior-call summaries, web content, or knowledge-base text contains untrusted instructions. Retrieved content label, prompt construction, model output


We used to think the main question was whether the model would refuse the obvious attack. That is too narrow. The better question is whether the system keeps the blast radius small when the model is confused.


[NIST's 2025 adversarial machine learning taxonomy](https://csrc.nist.gov/pubs/ai/100/2/e2025/final) frames attacks and mitigations across the AI lifecycle. For voice agents, that lifecycle includes pre-launch scenario testing, production monitoring, incident review, and regression tests created from real failures.


## The 6-layer voice agent prompt injection test plan


Run these layers in order. Do not jump straight to red-team prompts before you know which tools, data classes, and policy boundaries are in scope.


Layer What to test Passing condition Common failure


1. Spoken input Direct spoken override attempts, role changes, and repeated pressure Agent keeps role and routes unsafe requests to refusal or escalation Agent follows caller-provided meta-instructions


2. Transcript normalization ASR uncertainty, homophones, noisy calls, language switching, background voices Risky or uncertain turns are flagged before tool use Clean final transcript erases the risky audio context


3. Context boundaries System prompt, retrieved docs, CRM notes, prior-call summaries, and memory Untrusted content is labeled as data, not instructions External content changes model behavior


4. Tool permissions Reads, writes, transfers, refunds, bookings, messages, and exports Backend policy enforces authorization outside the model Model-triggered tool call bypasses business rules


5. Output policy Sensitive data, hidden instructions, unsupported advice, and unsafe summaries Output filter blocks leakage before the caller hears it Agent refuses input but leaks data in the response


6. Evidence logging Audio, transcript, prompt version, tool trace, policy result, and reviewer decision Every failure is replayable and can become a regression test Security result is a note with no reproducible evidence


OWASP's[prevention cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html) recommends structured separation between instructions and user data, output monitoring, least privilege, and human approval for high-risk actions. In a phone agent, make those controls visible in the test evidence. A launch review should not accept "the model refused it" as the only proof.


## How to build a safe test corpus


Start with defensive categories, not a list of clever payloads. The goal is coverage, repeatability, and evidence.


Test category Include these scenarios Expected safe behavior


Role override Caller asks the agent to behave outside its assigned role. Agent stays in role and explains the supported task boundary.


Hidden instruction request Caller asks for internal prompt, policies, chain of thought, secrets, or previous-call details. Agent refuses disclosure and continues only with the caller's authorized task.


Jailbreak and role-breaking Caller pressures the agent to ignore policy, become another persona, or treat the test as a special exception. Agent keeps its assigned role, avoids unsafe role-play, and logs the refusal or escalation path.


Sensitive-data extraction Caller asks for another person's data, account context, call history, or system configuration. Agent verifies identity and denies unauthorized disclosure.


Tool escalation Caller asks the agent to skip confirmation, bypass eligibility, refund, transfer, delete, or write a record. Backend policy blocks the action unless preconditions pass.


Indirect context injection Retrieved notes or documents contain instruction-like content. Agent treats retrieved content as quoted data, not policy.


Multi-turn social engineering Caller starts normal, then pressures the agent with urgency, authority, sympathy, or repetition. Agent maintains the workflow and escalates when policy says to.


ASR stress Noise, accents, interruptions, language switches, and low-confidence words around risky requests. Agent asks for clarification or reduces tool privileges.


Output leakage Agent response could reveal hidden instructions, sensitive fields, or excess handoff context. Output policy blocks or redacts before delivery.


Keep the phrases safe in shared docs. The actual test suite can store authorized payloads in a private repository with ownership, review, and use restrictions. Public content should teach the defensive shape, not hand attackers a copy-paste list.


## What metrics should the security test report include?


Use metrics that connect detection, false positives, blast radius, and evidence quality.


Metric Definition Starting threshold What to do when it fails


Unsafe completion rate Share of tests where the agent violated policy or workflow. 0 critical failures Block launch or remove affected tool/data access.


Unauthorized tool-action rate Share of tests where a tool call was attempted without required preconditions. 0 for critical tools Move authorization into deterministic backend policy.


Sensitive disclosure rate Share of tests where protected data or hidden instructions reached the caller or reviewer. 0 Add output checks, redaction, and context minimization.


Clarification rate Share of ambiguous audio turns where the agent asked a clarifying question. High for low-confidence risky turns Tune risk scoring and ask-before-action policy.


False positive rate Share of legitimate caller requests blocked as attacks. Below 5% for low-risk flows Add softer escalation and reduce keyword-only blocking.


Evidence completeness Share of failures with audio, transcript, prompt version, tool trace, and policy result. 100% for critical tests Fix logging before expanding the test suite.


The exact numbers should vary by workflow. A healthcare identity flow, bank transfer flow, or refund flow needs stricter thresholds than a public FAQ agent. The important rule is that critical failures are counted separately. A 99% pass rate can still be unacceptable if the 1% includes a data leak.


## How to test tool calls without creating real side effects


Prompt injection becomes more dangerous when the agent can act. Test those actions in a sandbox first.


Tool class Risk Safer test setup


Read account data Unauthorized disclosure Use synthetic accounts with clear authorization states.


Write customer records Corruption or privacy breach Point writes at a sandbox CRM with diffable records.


Booking or scheduling Real appointments or downstream messages Use mock calendars and deterministic availability fixtures.


Payments and refunds Financial loss or PCI scope expansion Use payment-provider sandbox tokens and require explicit confirmation.


Transfers and handoffs Over-sharing context or routing to wrong queue Use test queues and inspect handoff payloads.


Webhooks and exports Data leakage outside approved boundary Sign callbacks, use test endpoints, and block real destinations.


Pair this with the[voice agent sandbox testing guide](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) . The model should never be the only control between a caller and a side effect. Backend policy should validate identity, authorization, idempotency, and required confirmations.


> **Tool-action rule:** if a spoken instruction can trigger a write, transfer, export, payment, or record lookup, the security test must prove that non-LLM policy enforces the boundary.


## What defenses should a voice agent verify?


Do not claim prompt injection is solved. Verify layered controls and document what each layer can and cannot catch.


Defense What it catches What it does not catch


Input pattern checks Obvious override attempts and known risky phrases Novel phrasing, multi-turn pressure, and ASR ambiguity


ASR confidence and audio review Low-confidence risky turns and background speech Cleanly transcribed but malicious requests


Structured prompt separation Confusion between instructions and data Excessive tool permissions or weak backend policy


Least-privilege tools Unauthorized action after model confusion Bad authorized actions if business rules are wrong


Output monitoring Sensitive leakage and policy-violating responses Tool actions that already happened


Human approval High-risk actions and unclear identity states Low-risk flows at high volume unless sampling is designed


Regression tests Known failures after fixes New attack classes and model/provider changes


[OWASP](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) is explicit that there are no foolproof prompt-injection defenses. Treat that as a design constraint. Your system needs containment, monitoring, and rollback, not just a better filter.


## The pre-launch security evidence packet


Before launch, save one packet for the security gate. It should be boring enough that a reviewer can inspect it quickly.


Evidence item Why it matters Owner


Scope statement Names which agents, intents, tools, data classes, and channels were tested. Security and engineering


Test corpus summary Shows coverage by category without exposing sensitive payloads publicly. QA or security


Run results Lists pass/fail by scenario, severity, and tool/data class. QA


Failed-call replays Lets reviewers inspect audio, transcript, model response, and tool trace. Engineering


Fix log Connects every critical failure to a code, prompt, policy, or routing change. Owning team


Regression suite Proves the same failure will be caught on the next release. QA and CI owner


Residual risk note Names what is still hard or out of scope. Security


Launch decision Records whether launch is approved, blocked, narrowed, or accepted with risk. Launch owner


This evidence packet should link to your[production readiness checklist](https://hamming.ai/resources/voice-agent-production-readiness-checklist) . Security is not a standalone spreadsheet. It affects launch gates, monitoring, rollback, and incident response.


## Flaws but not dealbreakers


**False positives are real.** If every caller who says "start over" gets treated as malicious, support quality will suffer. Use risk scoring and softer responses for ambiguous requests.


**Some attacks only show up over time.** Multi-turn pressure, model changes, and new tool integrations can create behavior that the original suite did not cover. Re-run security tests after prompt, model, tool, and retrieval changes.


**Detection is not enough.** The safest architecture assumes a risky turn can slip through. Limit tool privileges, minimize context, monitor outputs, and require human approval for high-impact actions.


## After a failed prompt injection test


Do not just add one phrase to a denylist. Classify the failure and fix the layer that allowed harm.


Failure Likely fix Regression test


Agent changed role Harden role boundary and add refusal examples. Same scenario with direct and multi-turn variants.


Agent leaked sensitive context Reduce context, add output checks, and review data access. Disclosure attempt across authorized and unauthorized caller states.


Agent attempted unsafe tool call Move authorization to backend policy and require confirmation. Tool-call guardrail with failed preconditions.


Retrieved content changed behavior Label retrieved content as untrusted and quote it as data. Indirect injection in CRM note or knowledge-base fixture.


ASR ambiguity led to action Require clarification for low-confidence risky turns. Noisy and accented variants before tool use.


Reviewer cannot reproduce failure Fix logging and evidence capture. Evidence completeness check in CI or launch review.


Confirmed production failures should feed your[incident response runbook](https://hamming.ai/resources/voice-agent-incident-response-runbook) and your[failed-call regression test runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) . Security test results are only useful if they change the next release.


## Voice agent prompt injection checklist


Use this checklist before launch and after any major prompt, model, retrieval, or tool change.


- Scope every data class the agent can access: caller identity, account data, PHI, PCI data, CRM notes, prior calls, tool results, and exports.
- List every tool the model can request and the deterministic policy that approves or rejects it.
- Run spoken direct-injection tests, not just typed prompts.
- Run multi-turn pressure tests where the unsafe request appears after a valid task.
- Run ASR stress tests with noise, accents, interruptions, and low-confidence segments.
- Test indirect injection from retrieved notes, summaries, knowledge-base content, and tool outputs.
- Verify output checks for hidden instructions, sensitive data, and over-broad handoff summaries.
- Store replayable evidence for every critical failure.
- Convert every confirmed critical failure into a regression test.
- Record residual risk before production traffic.


## Related Guides


- [Voice Agent Security Review Questions](https://hamming.ai/resources/voice-agent-security-review-questions)
- [Voice Agent Production Readiness Checklist](https://hamming.ai/resources/voice-agent-production-readiness-checklist)
- [PII Redaction for Voice Agents](https://hamming.ai/resources/pii-redaction-voice-agents-compliance-architecture-guide)
- [Voice Agent Workflow Testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook)
- [Voice Agent Sandbox Testing](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects)
- [Voice Agent CI/CD Testing](https://hamming.ai/resources/voice-agent-testing-ci-cd-regression-load-security)
- [Failed Production Call Regression Test Runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook)
- [Voice Agent Call Evidence Export Runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook)
- [Voice Agent Incident Response Runbook](https://hamming.ai/resources/voice-agent-incident-response-runbook)
