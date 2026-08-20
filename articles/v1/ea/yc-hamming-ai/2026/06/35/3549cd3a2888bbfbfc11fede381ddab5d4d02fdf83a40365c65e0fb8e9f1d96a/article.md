---
schema_version: "1.0.0"
document_id: "3549cd3a2888bbfbfc11fede381ddab5d4d02fdf83a40365c65e0fb8e9f1d96a"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/voice-agent-production-call-review-triage"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:70c74d7154f1fbee25ce3af6c9b64165bb721446fdfbb75d21f3b3c9c62e8ff1"
---

# Voice Agent Call Review Triage Runbook

**Voice agent call review triage** is the process of ranking production calls by review value so humans inspect the calls most likely to expose product bugs, unsafe behavior, compliance risk, customer-impacting failures, or missing regression coverage.


If you run fewer than 100 production voice agent calls per week, a simple manual review habit is probably fine. Listen to failed calls, review a few random successes, and keep notes.


This runbook is for teams past that point. Once the system handles hundreds, thousands, or tens of thousands of calls per day, "pick 20 recordings at random" stops being a quality program. It becomes a lottery.


We used to think the main question was, "How many calls should QA review?" The better question is, "Which calls can change what we ship, block, escalate, or test tomorrow?"


> **TL;DR:** Build a production voice call review queue with 5 parts: selection reason, triage score, evidence packet, reviewer owner, and outcome. Keep random sampling for calibration, but prioritize calls with unsafe responses, failed tool calls, abandonment, repeat callers, low-confidence turns, compliance risk, new failure clusters, and high-impact cohorts.


> **Methodology Note:** This triage runbook is based on Hamming's analysis of production voice agent calls and QA review workflows across 10K+ voice agents (2025-2026). Hamming's platform has 10M+ mins protected. We've tested agents built on LiveKit, Pipecat, ElevenLabs, Retell, Vapi, and custom-built solutions.
>
>
> The goal is not to review more calls. The goal is to review the calls that can change reliability, safety, compliance, or regression coverage.


Across Hamming's 10M+ mins protected and 10K+ voice agents, we found that review systems break when every call has the same priority. The useful calls are not evenly distributed.


**Last Updated:** June 2026


**Related Guides:**


- [Voice Agent Call Replay QA Checklist](https://hamming.ai/resources/voice-agent-call-replay-qa-checklist) - review selected calls on one synchronized evidence timeline
- [Voice Agent Call Evidence Export Runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) - package transcripts, audio, traces, and tool evidence after selecting calls
- [Voice Agent Response Coverage](https://hamming.ai/resources/voice-agent-response-coverage) - turn unresolved production demand into coverage work
- [Failed Production Call Regression Tests](https://hamming.ai/resources/failed-production-call-regression-test-runbook) - promote selected failures into repeatable tests
- [Post-Call Analytics for Voice Agents](https://hamming.ai/resources/voice-agent-analytics-post-call-metrics-definitions-formulas-dashboards) - define metrics that feed the review queue
- [Slack Alerts for Voice Agents](https://hamming.ai/resources/slack-alerts-voice-agents-monitoring-guide) - route urgent production failures to the right channel
- [Voice Agent Workflow Testing](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) - validate tool calls and side effects
- [Voice Agent SLOs and Error Budgets](https://hamming.ai/resources/voice-agent-slos-error-budgets) - decide when quality issues should block releases
- [Voice Agent Transcript Search Schema](https://hamming.ai/resources/voice-agent-transcript-search-schema) - make review evidence searchable


## Random Sampling Is Calibration, Not Triage


Random sampling still has a job. It catches drift in your scoring rubric, gives QA a baseline view, and prevents the team from only looking at obvious failures.


It should not be the only way calls enter review.


[Salesforce describes contact center quality management](https://www.salesforce.com/service/contact-center/quality-management/) as a continuous loop of monitoring, evaluation, scoring, coaching, and improvement. That loop gets weaker when selection is blind. The call that caused a duplicate refund, unsafe healthcare answer, or 12-minute abandonment should not compete equally with a clean happy-path call.


> **Triage rule:** random samples protect calibration. Risk-based samples protect customers.


Use both:


Selection Mode Best For Weakness


Random sample Calibration, representative baseline, evaluator drift checks Wastes review capacity on low-signal calls


Stratified sample Comparing agents, queues, languages, locations, or cohorts Can miss rare but severe failures


Risk-based queue Unsafe responses, failed tools, abandonments, repeat callers, new clusters Needs guardrails to avoid overfitting to obvious failures


Customer-reported queue Fast support follow-up and incident reconstruction Depends on external reports and usually arrives late


Calibration sample Keeping human and automated scoring aligned Does not replace production incident review


The mistake is pretending one mode can do every job.


## Select Calls by Reason First


Every call in the review queue should carry a selection reason. Without it, reviewers waste time rediscovering why the call mattered.


Selection Reason Add the Call When Default Queue Reviewer


` unsafe_response


` The agent may have given unsafe, prohibited, or policy-violating advice P0 Compliance + product


` failed_tool_call


` A tool timed out, returned bad data, duplicated a write, or used unsupported arguments P0 Engineering


` customer_reported


` A customer, support rep, or account team named a bad call P0 Support + engineering


` abandoned_before_resolution


` Caller hung up before success, transfer, or safe fallback P1 QA + product


` repeat_caller


` Same caller or account came back within the review window for the same task P1 QA + product


` negative_sentiment_shift


` Caller frustration rose during the call P1 QA


` low_confidence_turn


` ASR, intent, policy, or evaluator confidence fell below threshold P2 QA


` new_failure_cluster


` Several calls share a previously unseen failure pattern P1 Product + QA


` cohort_regression


` A language, queue, location, provider, or agent version degraded P1 Engineering + product


` random_calibration


` The call was sampled to keep scoring honest P2 QA


The[call evidence export runbook](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) starts after this decision. It explains how to package transcripts, audio, traces, and tool evidence. Triage decides which calls deserve that packet.


## Use a Triage Score, But Keep It Explainable


Do not build a black-box score that nobody trusts. A review score should be simple enough for an ops lead to inspect and override.


Use this starter formula:


```text
review_priority     =        severity        +     evidence_quality        +     novelty        +     cohort_impact        +     freshness        -     reviewer_load_penalty
```


Component Score How to Assign It


Severity 0-5 Customer harm, compliance risk, money movement, safety risk, or blocked task


Evidence quality 0-3 Transcript, audio, trace, tool result, and evaluator rationale are available


Novelty 0-3 New failure cluster, new agent version, new provider, or unseen user behavior


Cohort impact 0-3 Affects many calls, high-value customers, regulated workflows, or a vulnerable segment


Freshness 0-2 Happened recently enough to debug while logs and context are still useful


Reviewer load penalty 0-3 Deduct when the queue is saturated with duplicates from the same issue


Then map the score to queues:


Score Queue SLA Action


12+ P0 Same day Assign owner, inspect packet, decide block/escalate/fix


8-11 P1 2 business days Review for product bug, prompt bug, workflow gap, or regression candidate


4-7 P2 Weekly Use for pattern review, coaching, coverage expansion, or calibration


0-3 Archive No human review by default Keep aggregate label unless it joins a larger cluster


This is intentionally simple. The score is a routing tool, not a truth machine.


> **Review-value definition:** a call is worth human review when the likely next action could change a prompt, policy, tool, product workflow, compliance queue, regression suite, or customer follow-up.


## Build the Daily Review Queue


The daily queue should be small enough to finish and structured enough to trust.


Queue Daily Contents Cap Exit Criteria


P0 safety and incidents Unsafe responses, failed writes, customer reports, compliance flags No artificial cap Owner assigned and decision recorded


P1 product failures Abandonment, repeat caller, new cluster, cohort regression 10-30 calls Issue labeled and pattern counted


P2 QA signal Low confidence, sentiment shift, unusual fallback, long silence 20-50 calls Either dismissed, clustered, or promoted


Calibration sample Random calls across agents, cohorts, and outcomes 5-10% of review volume Human/automated scoring compared


The right volume depends on reviewer capacity. A queue of 500 calls that nobody finishes is worse than a queue of 40 calls with clean outcomes.


[Qualtrics' quality management documentation](https://www.qualtrics.com/support/xm-discover/ccqm/qualtrics-contact-center-quality-management/) describes rubrics, scorecards, alerts, and tickets that route coaching opportunities. Voice agents need the same control loop, but with voice-specific evidence: transcript turn, audio segment, trace, tool result, prompt or agent version, and evaluator rationale.


For urgent production failures, route the P0 item to the same operating surface you use for[voice agent Slack alerts](https://hamming.ai/resources/slack-alerts-voice-agents-monitoring-guide) . For slow quality decay, keep the item in the QA queue and aggregate it into weekly coverage work.


## Give Reviewers a Packet, Not a Dashboard Hunt


A reviewer should not need five tabs and tribal knowledge to judge one call.


Minimum packet:


Field Required? Why


Canonical call ID Yes Joins transcript, audio, traces, evaluations, provider IDs, and review outcome


Selection reason Yes Explains why the call is in the queue


Triage score Yes Shows priority and routing logic


Agent version Yes Connects behavior to prompt, model, tool, or deployment changes


Transcript excerpt Yes Lets reviewer inspect the relevant turn quickly


Audio pointer Usually Captures silence, interruption, tone, background noise, and ASR ambiguity


Trace or tool evidence For workflow calls Proves what happened outside the conversation text


Evaluation result Yes Includes rubric, score, evaluator version, and failure label


Privacy state Yes Shows whether the packet is redacted, restricted, or aggregate-only


Allowed outcomes Yes Prevents free-text review notes from becoming a dead end


This should reuse your[call evidence export](https://hamming.ai/resources/voice-agent-call-evidence-export-runbook) format. The triage queue should store the` selectionReason


` and` reviewPriority


` ; the evidence packet should store the proof.


[The CTAA quality assurance guide](https://ctaa.org/wp-content/uploads/2019/02/OCOC_Quality_Assurance.pdf) emphasizes documented QA plans, call monitoring, customer satisfaction tools, reporting, and escalation. That old operational lesson still applies: the review process has to create a record that managers and teams can act on later.


## Force Every Review Into an Outcome


The review is not done when someone listens to the call. It is done when the next action is recorded.


Use a closed outcome taxonomy:


Outcome Use When Next Step


` no_issue


` The call was correctly handled or the signal was a false positive Feed calibration data back to scoring


` prompt_bug


` Agent wording, instruction following, or policy interpretation caused the failure Update prompt and add regression case


` tool_bug


` Backend action, integration, timeout, retry, or argument handling failed File engineering issue and keep tool evidence


` product_gap


` Caller asked for a supported-but-missing workflow Add roadmap or coverage item


` unsupported_request


` Caller asked for something the agent should not handle Improve graceful fallback


` policy_risk


` Agent may have violated safety, compliance, legal, or brand policy Route to compliance or policy owner


` regression_candidate


` The failure should never recur Convert to a test using safe fixtures


` coaching_signal


` Human operations or handoff process needs change Route to training or ops lead


Pair` regression_candidate


` with the[failed production call regression runbook](https://hamming.ai/resources/failed-production-call-regression-test-runbook) . A call review that finds a bug but does not create a durable test is only a one-time inspection.


For coverage gaps, pair` product_gap


` and` unsupported_request


` with[response coverage](https://hamming.ai/resources/voice-agent-response-coverage) . The goal is not to make the agent handle every long-tail request. The goal is to know which gaps deserve workflow coverage and which deserve a clean handoff.


## Protect Review Quality


Review queues drift without guardrails.


Risk Symptom Guardrail


Duplicate flooding One incident fills the whole queue with near-identical calls Cluster first, review representative examples, keep aggregate count


Severity inflation Everything becomes P0 because teams want attention Require severity definition and owner override reason


Reviewer fatigue Queue grows faster than humans can resolve Cap P1/P2, preserve P0, archive low-score calls into clusters


Privacy leakage Reviewers see raw audio or sensitive transcript text by default Use redacted packets and role-gated raw access


Calibration drift Reviewers disagree on the same call Keep random calibration samples and compare decisions


Dead-end notes Reviewers write comments but no action happens Use closed outcomes and required next owner


Dashboard-only evidence Nobody can reconstruct why the call was selected Store selection reason, score, packet pointer, and review outcome


Privacy deserves special attention. Production calls may include names, account numbers, health details, addresses, payment information, and background speech. Use the[log retention compliance checklist](https://hamming.ai/resources/voice-agent-log-retention-compliance-checklist) and[security review questions](https://hamming.ai/resources/voice-agent-security-review-questions) before giving broad teams raw recordings or unredacted transcripts.


## What Hamming Does in This Loop


Hamming helps teams move from "we have a dashboard" to "we know which calls need action."


Use Hamming to:


- Analyze production voice calls across transcripts, audio, metadata, and evaluation results.
- Rank calls by failure type, severity, confidence, cohort, and review value.
- Give reviewers the evidence needed to decide whether the issue is a prompt bug, tool bug, policy risk, product gap, or regression candidate.
- Turn selected failures into[workflow tests](https://hamming.ai/resources/voice-agent-workflow-testing-runbook) ,[sandbox tests](https://hamming.ai/resources/voice-agent-sandbox-testing-tool-calls-side-effects) , or CI regression cases.
- Measure whether fixes actually reduce repeat failures, abandonment, unsafe responses, and unresolved coverage gaps.


The practical loop is:


```text
monitor     production     calls        -  >     score     review     priority        -  >     package     evidence        -  >     review     with     closed     outcomes        -  >     fix     or     escalate        -  >     promote     durable     failures     into     tests        -  >     watch     whether     the     cluster     disappears
```


This is where call review becomes a product-quality system instead of a recording audit.


## Final Checklist


Before you trust a production voice call review queue, check:


- Every reviewed call has a selection reason.
- P0 calls bypass normal queue caps.
- Random samples are used for calibration, not as the whole QA strategy.
- The triage score is explainable and overrideable.
- Duplicate calls are clustered before they consume review capacity.
- Reviewers get transcript, audio pointer, trace/tool evidence, evaluation result, and privacy state.
- Every review ends in a closed outcome.
- Regression candidates become tests with source labels.
- Compliance and safety issues route to a named owner.
- Weekly reporting shows which clusters disappeared, persisted, or got worse.
