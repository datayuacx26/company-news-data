---
schema_version: "1.0.0"
document_id: "435e862783a9cc62ea38aed0a66e8144f515373cb7d2f0427e4dbac6bf319b17"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/llm-grader-voice-agent-call-scoring-rubric"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:dfb57382784bfa7f1e941ca469181a39883ed5ebde0e048cf09103681bf924bc"
---

# LLM Grader for Voice Agent Calls: Scoring Rubric and Calibration Template

An **LLM grader for voice agent calls** should not answer one vague question: "Was this a good call?" That prompt is too broad. It hides the difference between a call that completed the task, a call that sounded empathetic, and a call that quietly violated policy.


This template is for teams that already have call transcripts, audio links, tool traces, and production metadata. If you only run a few demo calls each week, start with the[voice agent evaluation metrics guide](https://hamming.ai/resources/voice-agent-evaluation-metrics-guide) and manual review. A full LLM grader is useful once you need to score hundreds or thousands of calls consistently.


We used to think the hard part was the judge model. It is not. The hard part is deciding what the judge is allowed to infer, what software verifies exactly, and which calls still need human review.


The dangerous case is boring: the agent sounds calm, the caller leaves politely, and the dashboard shows a pass. Then someone opens the tool trace and sees that the booking failed, the disclosure was partial, or the answer came from the wrong policy. A useful grader catches that kind of miss.


> **TL;DR:** Build a voice-agent LLM grader as a structured scorecard:
>
>
> - Use deterministic checks for exact facts: required disclosures, tool calls, IDs, durations, transfers, and write outcomes.
> - Use LLM judgment for semantic outcome, policy interpretation, empathy, conversation flow, and hallucination risk.
> - Require evidence spans, confidence, grader version, and human escalation for high-risk or ambiguous calls.
> - Calibrate the rubric against human QA before trusting dashboards, alerts, or release gates.


> **Methodology Note:** This rubric is based on Hamming's analysis of production voice agent calls and evaluation workflows across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> The goal is not to automate human judgment away. The goal is to make automated call scoring specific enough that humans can trust, audit, and improve it.


**Last Updated:** July 2026


**Related Guides:**


- [Voice Agent Evaluation Metrics](https://hamming.ai/resources/voice-agent-evaluation-metrics-guide) - definitions and benchmarks for the metrics behind the rubric
- [Voice Agent Call Review Triage](https://hamming.ai/resources/voice-agent-production-call-review-triage) - how to decide which scored calls deserve human review
- [Voice Agent Daily Failure Report Template](https://hamming.ai/resources/voice-agent-daily-failure-report-template) - how to summarize failed-call clusters and owners
- [Structured Output Validation Checklist](https://hamming.ai/resources/voice-agent-structured-output-validation-checklist) - how to prove extracted fields are supported by caller evidence
- [Voice Agent Call Evidence Export Runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - how to package transcript, audio, trace, and tool evidence
- [Voice Agent Compliance Analytics](https://hamming.ai/resources/voice-agent-compliance-analytics-audit-trails) - how to keep policy scoring auditable
- [Failed Production Call Regression Tests](https://hamming.ai/resources/failed-production-call-regression-test-runbook) - how to turn confirmed misses into tests
- [Voice Agent CI/CD Testing](https://hamming.ai/resources/voice-agent-testing-ci-cd-regression-load-security) - how to block releases when quality drops


## What Should an LLM Grader Score in a Voice Agent Call?


An LLM grader should score voice calls across independent dimensions, not collapse everything into one "quality" number. A good call can fail compliance. A friendly call can fail the task. A technically correct call can still frustrate the caller because of silence, interruptions, or a bad escalation.


Here is the baseline rubric I would start from:


Dimension Weight What the Grader Checks Evidence Required


Task completion 25% Did the caller's goal finish with the correct final state? transcript span, outcome event, tool result


Factual accuracy 20% Were facts, prices, eligibility, dates, and policy statements correct? transcript span, source/tool reference


Policy and compliance 20% Were required disclosures, refusals, consent statements, and restricted topics handled correctly? transcript span, policy ID, reviewer route


Conversation flow 10% Did the agent avoid loops, missed context, overtalk, and unnecessary turns? turn sequence, interruption/dead-air markers


Empathy and professionalism 10% Was tone appropriate for the caller's situation? transcript span and audio pointer when available


Escalation judgment 10% Did the agent transfer, hand off, or refuse at the right time? escalation event, queue/owner, rationale


Evidence quality 5% Is there enough call evidence to trust the score? call ID, grader version, confidence, packet link


For regulated workflows, increase policy and compliance to 30-40% and reduce softer dimensions. For sales or support coaching, keep empathy and conversation flow higher. The exact weights will change by business, but the tradeoff needs to be visible.


> **Definition:** A voice-agent LLM grader is a rubric-based evaluator that reviews transcript, audio, tool, and metadata evidence for one call and returns structured scores with evidence and confidence. It should not be a single model opinion about whether the call was "good."


## What Should Be Deterministic vs LLM-Judged?


Do not spend model judgment on things software can prove exactly. Deterministic checks are cheaper, repeatable, and easier to audit.


If the transcript contains a required disclosure, match it. If the refund tool returned` failed


` , trust the tool result over the agent's reassuring sentence. Save the judge model for calls where meaning actually matters.


Question Best Evaluator Why


Was the required disclosure spoken? Deterministic first, LLM fallback Exact phrase or accepted variant can be checked against transcript


Did the refund, booking, or ticket tool succeed? Deterministic Tool result and final state are source of truth


Did the caller ask for an unsupported action? LLM with evidence span Requires semantic interpretation


Was the agent empathetic enough? LLM plus human calibration Requires context, tone, and domain judgment


Did the agent hallucinate a policy? LLM plus source/tool reference Needs grounding against allowed knowledge


Was a human escalation required? LLM plus rule table Depends on policy, severity, and caller state


Was the call too slow? Deterministic Latency and silence are measurable


Is the score safe to use for compliance reporting? Human review for high-risk cases Legal and regulatory interpretation needs a final owner


[NVIDIA's LLM-as-a-judge documentation](https://docs.nvidia.com/nemo/microservices/latest/evaluator/metrics/llm-as-a-judge.html) frames judge models as useful when tasks need flexible, domain-specific scoring criteria. That is exactly the voice-call case. But flexible does not mean vague.


[Google Vertex AI evaluation docs](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/determine-eval) also separate rubric types, tool-use quality, hallucination, instruction-following, and multi-turn quality. Voice calls need the same separation, plus audio and tool evidence.


## Copy-Ready Voice Call Grader Prompt


Use this as a starting point. Replace the bracketed policy and workflow sections with your actual agent rules. Keep the prompt plain enough that a QA lead can argue with it in review.


```text
You     are     grading     one     production     voice     agent     call  .      You     must     grade     only     from     the     provided     evidence  :    -     transcript     with     speaker     labels     and     timestamps    -     call     metadata    -     tool     call     log    -     expected     workflow     policy    -     escalation     policy    -     known     product     or     compliance     rules      Do     not     infer     facts     that     are     not     in     the     evidence  .    If     evidence     is     missing  ,     lower     evidence_quality     and     explain     what     is     missing  .      Grade     these     dimensions     independently  :    1  .     task_completion    2  .     factual_accuracy    3  .     policy_compliance    4  .     conversation_flow    5  .     empathy_professionalism    6  .     escalation_judgment    7  .     evidence_quality      For     each     dimension  ,     return  :    -     score  :     0  ,     1  ,     2  ,     3  ,     4  ,     or     5    -     pass  :     true     or     false    -     evidence_spans  :     timestamped     transcript     spans     or     tool     events    -     rationale  :     one     short     sentence    -     confidence  :     low  ,     medium  ,     or     high      Critical     failure     rules  :    -     If     the     agent     gave     unsafe     advice  ,     set     critical_failure  =  true  .    -     If     the     required     disclosure     was     missing  ,     set     critical_failure  =  true  .    -     If     the     tool     result     contradicts     the     agent  '  s statement, set critical_failure=true.   - If the transcript is too incomplete to judge, set needs_human_review=true.     Return JSON only. Do not include markdown.
```


This prompt asks for evidence before explanation. In review, that changes the conversation. The reviewer can click the transcript span or tool result that caused the score instead of debating a polished paragraph. A beautiful rationale with no evidence is just another note to audit.


## Structured JSON Output Schema


Use a fixed output shape so dashboards, alerts, and regression queues can trust the result.


```text
{        "  call_id  "  :     "  call_123  "  ,        "  grader_version  "  :     "  voice-call-rubric-2026-07-01  "  ,        "  overall_score  "  :     82  ,        "  critical_failure  "  :     false  ,        "  needs_human_review  "  :     true  ,        "  dimensions  "  :     {          "  task_completion  "  :     {            "  score  "  :     4  ,            "  pass  "  :     true  ,            "  confidence  "  :     "  high  "  ,            "  evidence_spans  "  :     [  "  00:02:14-00:02:39  "  ,     "  tool:booking.create.success  "  ]  ,            "  rationale  "  :     "  The appointment was created and confirmed to the caller.  "          }  ,          "  factual_accuracy  "  :     {            "  score  "  :     3  ,            "  pass  "  :     true  ,            "  confidence  "  :     "  medium  "  ,            "  evidence_spans  "  :     [  "  00:01:01-00:01:19  "  ]  ,            "  rationale  "  :     "  The agent gave correct availability but did not cite the cancellation rule.  "          }  ,          "  policy_compliance  "  :     {            "  score  "  :     2  ,            "  pass  "  :     false  ,            "  confidence  "  :     "  high  "  ,            "  evidence_spans  "  :     [  "  00:00:22-00:00:31  "  ]  ,            "  rationale  "  :     "  The required consent disclosure was incomplete.  "          }        }  ,        "  selection_reason  "  :     "  policy_compliance_failure  "  ,        "  recommended_outcome  "  :     "  policy_risk  "  ,        "  next_action  "  :     "  Send to compliance reviewer and add disclosure regression test.  "    }
```


Pair this with the[call evidence export runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) . The grader output should point to the evidence packet, not replace it.


## How Do You Calibrate an LLM Grader Against Human QA?


Calibrate before you automate decisions. The first version of any call grader is usually overconfident, and the errors are rarely spread evenly. Policy may be strict while empathy is generous, or the grader may punish long calls even when the caller needed extra help.


Step Sample Size What to Do Pass Criteria


1. Seed examples 50 calls Human QA labels each dimension and notes evidence spans Rubric terms are clear enough for two humans to apply


2. First grader run same 50 calls Run the LLM grader without showing human labels Identify disagreement by dimension


3. Rewrite rubric 20-40 disagreements Clarify criteria, add examples, split overloaded dimensions Fewer ambiguous failures


4. Holdout check 100 calls Compare grader and human labels on unseen calls High-risk false passes are near zero


5. Production shadow 1-2 weeks Score all calls, but keep humans in the loop Review queue quality improves


6. Ongoing drift check weekly Re-label random and high-risk samples Score distribution and disagreement stay stable


[LangSmith's LLM-as-judge guide](https://docs.langchain.com/langsmith/llm-as-judge) describes evaluator configuration, variable mapping, feedback fields, and human corrections. The operational lesson is simple: a grader is a product surface. Version it, test it, and inspect disagreements.


If you use scores in a[daily failure report](https://hamming.ai/resources/voice-agent-daily-failure-report-template) , include the grader version. Without it, week-over-week quality changes may only mean the rubric changed.


## How Should You Route Low-Confidence or High-Risk Scores?


An automated grader creates a review queue. It does not bypass one.


Trigger Route To Why


` critical_failure


=


true


` Compliance, safety, or incident owner The call may create customer or regulatory risk


` needs_human_review


=


true


` QA reviewer Evidence is missing or ambiguous


Low confidence on policy/compliance Compliance reviewer The model may not understand policy nuance


Tool result contradicts agent statement Engineering owner The issue may be an integration or state bug


Repeat low score in same cohort Product + QA One bad call may be noise; a cluster is a product problem


Grader-human disagreement Calibration queue The rubric or examples need work


[Microsoft's Quality Evaluation Agent docs](https://learn.microsoft.com/en-us/dynamics365/contact-center/administer/manage-quality-evaluation-agent) describe supervisor-defined evaluation criteria, scoring logic, evaluation plans, and compliance cautions for customer interactions. Voice-agent grading needs the same discipline. A raw model score is not an employment, compensation, medical, legal, or compliance decision.


For production triage, connect these routes to your[voice agent call review queue](https://hamming.ai/resources/voice-agent-production-call-review-triage) . The LLM grader makes the queue smaller and clearer. It does not make the humans disappear.


## What Are Common LLM Grader Failure Modes?


The grader can fail even when the model is strong.


Failure Mode What It Looks Like Fix


Transcript-only tunnel vision The text looks fine, but audio has long silence, overtalk, or bad tone Add audio pointers, latency, barge-in, and silence features


Vague quality score Every call lands between 70 and 85 Split the score into independent dimensions


Policy paraphrase drift The grader accepts "close enough" disclosures that legal would reject Add exact deterministic checks and policy IDs


Tool-blind grading The agent says the appointment was booked, but the tool failed Include tool logs and final state in the evidence


Empathy overreward Friendly language masks wrong action Cap empathy's contribution when task or policy fails


Calibration decay Scores shift after prompt, model, or rubric changes Version grader, run weekly human spot checks


Missing N/A handling Calls are penalized for criteria that do not apply Add N/A and exclude it from the denominator


There is a pattern here: the "friendly wrong call" problem. The agent sounds polished, the transcript reads well, and a broad judge gives it a passing score. Then you inspect the tool result or policy rule and realize the call failed.


That is why the rubric weights task completion, accuracy, and policy above tone.


## What Should Hamming Do in This Workflow?


Hamming helps teams grade voice calls with the evidence needed to trust the result:


- full-call transcript and audio context
- tool-call and trace evidence
- prompt, agent, model, and evaluator versions
- custom evaluation criteria and pass/fail rules
- production monitoring that scores every call, not just a sample
- review queues that route low-confidence, high-risk, or novel failures
- regression promotion when a failed call should become a test


Use the LLM grader result as one layer in the[voice agent analytics metrics stack](https://hamming.ai/resources/voice-agent-analytics-post-call-metrics-definitions-formulas-dashboards) . For fields extracted from the call, pair it with the[structured output validation checklist](https://hamming.ai/resources/voice-agent-structured-output-validation-checklist) . For regulated workflows, pair it with[compliance analytics](https://hamming.ai/resources/voice-agent-compliance-analytics-audit-trails) and your legal review process.


## Launch Checklist


Before using an LLM grader for production call decisions, verify:


- Each score dimension has a plain-English definition and examples.
- Deterministic checks run before LLM judgment.
- The grader receives transcript, audio pointer, metadata, tool logs, and policy context.
- The output JSON includes evidence spans, confidence, grader version, and next action.
- High-risk and low-confidence calls route to humans.
- Human QA labels at least 50 seed calls before production use.
- A holdout sample checks false passes before release gates depend on the grader.
- Weekly calibration samples compare human and model scores.
- Compliance, medical, legal, and employment-impacting decisions stay human-owned.
- Confirmed grader misses create new regression tests or rubric updates.


No measurement approach is free. This rubric adds review discipline, but it also adds maintenance. If nobody owns calibration, the score will drift. If nobody reviews false passes, the dashboard will look cleaner than reality.


The payoff is worth it once call volume grows. Reviewers stop listening blindly and spend more time on the calls where their judgment changes the system.
