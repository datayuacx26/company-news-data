---
schema_version: "1.0.0"
document_id: "cded1dadc73fa32dad339b8fb910df796a3601ea63cfa7ad890cbcd4dd723fef"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-380bdc997b57"
canonical_url: "https://hamming.ai/resources/how-to-evaluate-voice-agents"
published_at: "2025-12-23T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:39.163404+00:00"
fetched_at: "2026-07-28T22:24:50.551214+00:00"
content_hash: "sha256:d352fd4c15f5044344250c7f26c02215f9bd690afa4042cd38cc4fb79ba971d0"
---

# How to Evaluate Voice Agents: Framework, Metrics, Checklists, and Tooling (2026)

## TL;DR: Voice Agent Evaluation in 5 Minutes


**What "good" looks like:**


- Task completion >85%, First Call Resolution >75%, Containment >70%
- P50 latency 1.5-1.7s, P95 <5s end-to-end (based on Hamming production data)
- WER <10% in normal conditions, <15% with background noise
- Barge-in recovery >90%, Reprompt rate <10%


**The 5-step evaluation loop:**


1. **Define** → Success criteria, task constraints, acceptable failure modes
2. **Build** → Representative test set (golden paths + edge cases + adversarial)
3. **Run** → Automated evals at scale (100% coverage, not 1-5% sampling)
4. **Triage** → Quantitative metrics + qualitative review of failures
5. **Monitor** → Regression tests on every change + production alerting


**10 metrics to track:**


Category Metrics


**Task & Outcome** Task Success Rate, Containment Rate, First Call Resolution


**Conversation Quality** Barge-in Recovery, Reprompt Rate, Sentiment Trajectory


**Reliability** Tool-call Success Rate, Fallback Rate, Error Rate


**Latency** Turn Latency (P50/P95/P99), Time to First Word


**Speech** Word Error Rate (with noise/accent breakdowns)


**What to automate vs review manually:**


- ✅ **Automate** : Latency percentiles, WER calculation, task completion detection, regression testing
- 👁️ **Human review** : Edge case calibration, prompt tuning decisions, new failure mode discovery


**Quick filter:** If you're pre-production, focus on latency + task completion. Add the rest once you're live with real calls.


## Evaluation at a Glance


Before diving deep, here's the complete evaluation lifecycle:


Stage What You Do Output Automate?


**1. Define Success** Set task completion criteria, latency thresholds, acceptable error rates Success criteria document ❌ Manual


**2. Build Test Set** Create scenarios: happy paths, edge cases, adversarial, acoustic variations 100+ test scenarios ⚠️ Partially


**3. Run Automated Evals** Execute synthetic calls, collect metrics across all dimensions Metrics dashboard ✅ Fully


**4. Triage Failures** Review failed calls, categorize by failure mode, identify patterns Failure analysis report ⚠️ Partially


**5. Regression Test** Run test suite on every change, block deploys on degradation Pass/fail gate ✅ Fully


**6. Production Monitor** Track live calls 24/7, alert on anomalies, detect drift Real-time dashboards ✅ Fully


**Related Frameworks:**


- [The 4-Layer Voice Agent Quality Framework](https://hamming.ai/resources/guide-to-ai-voice-agents-quality-assurance) - Deep-dive on Infrastructure → Execution → User Reaction → Business Outcome
- [Testing Voice Agents for Production Reliability](https://hamming.ai/resources/testing-voice-agents-production-reliability) - 3-Pillar Framework (Load, Regression, A/B)
- [Call Center Voice Agent Testing](https://hamming.ai/resources/call-center-voice-agent-testing-guide) - 4-Layer Framework for contact center deployments
- [Voice Agent Monitoring KPIs](https://hamming.ai/resources/voice-agent-monitoring-kpis-production-guide) - Production monitoring metrics


---


## The 5-Step Voice Agent Evaluation Loop


Most evaluation failures come from skipping steps or doing them out of order. Here's the loop that actually works:


### Step 1: Define Tasks and Constraints


Before measuring anything, define what success looks like:


Question Example Answer


What is the agent's primary task? Schedule medical appointments


What task completion rate is acceptable? >90% for standard appointments


What latency is acceptable? P95 <5s end-to-end


What escalation rate is acceptable? <15%


What failure modes are acceptable? Edge cases can fail gracefully to human


What compliance requirements exist? HIPAA: no PHI in logs


**Output:** A success criteria document that everyone agrees on.


### Step 2: Build a Representative Test Set


Your test set determines what you can catch. Build it with intention:


Category % of Test Set Examples


**Happy Path** 40% Standard booking, simple inquiry, basic update


**Edge Cases** 30% Multi-intent ("book and also cancel"), corrections mid-flow, long calls


**Error Handling** 15% Invalid inputs, system timeouts, missing data


**Adversarial** 10% Off-topic, profanity, prompt injection attempts


**Acoustic Variations** 5% Background noise, accents, speakerphone


**Test set sizing:**


- Minimum viable: 50 scenarios
- Production-ready: 200+ scenarios
- Enterprise: 500+ scenarios with multilingual coverage


**Tip:** Every production failure should become a test case. Your test set should grow over time.


### Step 3: Run Automated Evals at Scale


Manual testing doesn't scale. At 10,000 calls/day, reviewing 1-5% means missing 95-99% of issues.


**What to automate:**


- Synthetic call generation (personas, accents, noise levels)
- Metric collection (latency, WER, task completion)
- Pass/fail determination against thresholds
- Report generation and trending


**Execution frequency:**


- Pre-launch: Full test suite (all scenarios)
- On change: Regression suite (critical paths)
- In production: Continuous synthetic monitoring (every 5-15 minutes)


### Step 4: Triage Failures (Quantitative + Qualitative)


Numbers tell you something is wrong. Listening tells you why.


**Quantitative triage:**


1. Sort failures by frequency (most common first)
2. Group by failure mode (latency, WER, task failure, etc.)
3. Identify patterns (time of day, user demographic, scenario type)


**Qualitative triage:**


1. Listen to 10-20 failed calls per failure mode
2. Identify root cause (prompt issue, ASR error, tool failure, etc.)
3. Document fix hypothesis
4. Prioritize by business impact


**Output:** Prioritized list of issues with root causes and fix hypotheses.


### Step 5: Regression Test + Monitor in Production


Every change is a potential regression. Every deployment needs verification.


**Regression testing protocol:**


1. Maintain baseline metrics from last known-good version
2. Run identical test suite on new version
3. Compare with tolerance thresholds:


- Latency: ±10%
- Task completion: ±3%
- WER: ±2%


4. **Block deployment if regression detected**


**Production monitoring:**


- Real-time dashboards (5-minute refresh)
- Automated alerting on threshold breaches
- Drift detection (gradual degradation over days/weeks)
- Anomaly detection (sudden spikes)


---


## Voice Agent Evaluation Metrics (Definitions + How to Measure)


### Task & Outcome Metrics


These answer: "Did the agent accomplish what the user needed?"


#### Task Success Rate (TSR)


**Definition:** Percentage of interactions where the agent successfully completed the user's primary goal.


**Formula:**


```text
TSR     =     (  Successfully     Completed     Tasks     /     Total     Attempted     Tasks  )     ×     100
```


**How to measure:**


- Define task completion criteria per use case (appointment booked, order placed, issue resolved)
- Tag each call with outcome (success, partial, failure)
- Calculate daily/weekly TSR


**What "good" looks like:**


Use Case Target Minimum Critical


Appointment scheduling >90% >85% <75%


Order taking >85% >80% <70%


Customer support >75% >70% <60%


Information lookup >95% >90% <85%


**Common pitfalls:**


- Counting "call completed" as "task completed" (user may have given up)
- Not tracking partial completions separately
- Ignoring multi-task calls


#### Containment Rate


**Definition:** Percentage of calls handled entirely by the voice agent without human escalation.


**Formula:**


```text
Containment     Rate     =     (  Agent  -  Handled     Calls     /     Total     Calls  )     ×     100
```


**How to measure:**


- Track escalation events ("transfer to human", "speak to agent")
- Distinguish intentional escalations (complex issues) from failure escalations (agent couldn't help)


**What "good" looks like:** >70% for most use cases, >85% for high-volume transactional tasks.


#### First Call Resolution (FCR)


**Definition:** Percentage of issues resolved on the first contact, without callback or follow-up.


**Formula:**


```text
FCR     =     (  Issues     Resolved     on     First     Contact     /     Total     Issues  )     ×     100
```


**How to measure:**


- Track same-caller repeat calls within 24-48 hours
- Survey users post-call ("Was your issue fully resolved?")
- Monitor for follow-up escalations


**What "good" looks like:** >75% for support, >85% for transactional.


#### Escalation Rate


**Definition:** Percentage of calls requiring human intervention.


**Formula:**


```text
Escalation     Rate     =     (  Escalated     Calls     /     Total     Calls  )     ×     100
```


**Target:** <25% overall, with breakdown by reason (user request vs agent failure).


---


### Conversation Quality Metrics


These answer: "Was the conversation natural and efficient?"


#### Barge-in (Interruption) Recovery Rate


**Definition:** Percentage of user interruptions where the agent successfully acknowledged and addressed the interruption.


**Formula:**


```text
Barge  -  in     Recovery     =     (  Successful     Recoveries     /     Total     Interruptions  )     ×     100
```


**How to measure:**


- Detect overlapping speech (user speaking while agent speaking)
- Classify recovery: agent stopped, acknowledged, addressed new topic
- Flag failures: agent continued talking, ignored interruption, repeated itself


**What "good" looks like:** >90% recovery rate.


**Example failure:**


```text
Agent  :     "  I can help you with that. Let me look up your account -   "    User  :     [  interrupting  ]     "  Actually, I need to cancel.  "    Agent  :     "   - and I see you have an appointment on Tuesday.  "    // Agent ignored interruption
```


#### Silence and Turn-Taking Metrics


**Definition:** Measures the rhythm and pacing of conversation.


**Metrics:**


Metric Definition Target


**Turn-Taking Efficiency** Smooth transitions / Total transitions >95%


**Awkward Silence Rate** Pauses >2s / Total turns <5%


**Overlap Rate** Overlapping speech / Total turns <3%


**What counts as smooth transition:** <200ms gap, no overlap >500ms.


#### Reprompt Rate


**Definition:** How often the agent asks the user to repeat themselves.


**Formula:**


```text
Reprompt     Rate     =     (  Clarification     Requests     /     Total     Turns  )     ×     100
```


**What "good" looks like:** <10% overall, <5% for simple intents.


**Phrases that indicate reprompts:**


- "Could you repeat that?"
- "I didn't catch that."
- "Can you say that again?"
- "Sorry, what was that?"


#### Sentiment Trajectory


**Definition:** How user sentiment changes from start to end of call.


**How to measure:**


- Score sentiment at call start (first 30 seconds)
- Score sentiment at call end (last 30 seconds)
- Track trajectory: improved, stable, degraded


**What "good" looks like:**


- Improved or stable in >80% of calls
- Degraded sentiment should trigger review


---


### Reliability Metrics


These answer: "Is the agent dependable?"


#### Tool-Call Success Rate


**Definition:** Percentage of external tool/API calls that succeed.


**Formula:**


```text
Tool     Success     =     (  Successful     Tool     Calls     /     Total     Tool     Calls  )     ×     100
```


**What "good" looks like:** >99% for critical tools (booking, payment), >95% for non-critical.


**Common tool failures:**


- Timeout (API slow)
- Authentication failure
- Invalid parameters
- Rate limiting


#### Fallback Rate


**Definition:** How often the agent falls back to generic responses or escalation.


**Formula:**


```text
Fallback     Rate     =     (  Fallback     Responses     /     Total     Responses  )     ×     100
```


**Fallback indicators:**


- "I'm not sure I understand."
- "Let me transfer you to someone who can help."
- Generic responses that don't address the query


**What "good" looks like:** <5% for trained intents, higher for out-of-scope queries.


#### Error Rate


**Definition:** Percentage of interactions with system errors.


**Formula:**


```text
Error     Rate     =     (  Interactions     with     Errors     /     Total     Interactions  )     ×     100
```


**What "good" looks like:** <1% system errors, <5% including user-caused errors.


---


### Latency Metrics


These answer: "Is the agent fast enough?"


#### Turn Latency (P50, P95, P99)


**Definition:** Time from user finishing speaking to agent starting to speak.


**Why percentiles, not averages:** Two systems can both report 400ms average:


- System A: P99 at 500ms (everyone's happy)
- System B: P99 at 3000ms (1% of users are furious)


**Targets (end-to-end with telephony, based on Hamming production data from 10M+ mins protected):**


Percentile Target Warning Critical


P50 <1.5s 1.5-1.7s >1.7s


P90 <2.5s 2.5-3.0s >3.0s


P95 <3.5s 3.5-5.0s >5.0s


**Reality check:** Based on Hamming's production data, P50 is typically 1.5-1.7 seconds, P90 around 3 seconds, and P95 around 5 seconds for cascading architectures (STT → LLM → TTS) with telephony overhead. Target P95 of 1.7s is aspirational but achievable with optimized pipelines.


#### Time to First Word (TTFW)


**Definition:** Time from call connection to agent's first audio.


**Target:** <400ms (critical for first impression).


#### Component Latency Breakdown


For debugging, measure each pipeline stage:


Component Target Warning Critical


ASR (Speech-to-Text) <300ms 300-500ms >500ms


LLM (Time-to-first-token) <400ms 400-600ms >600ms


TTS (Text-to-Speech) <200ms 200-400ms >400ms


---


### Speech Layer Metrics


These answer: "Is the agent hearing users correctly?"


#### Word Error Rate (WER)


**Definition:** Percentage of words incorrectly transcribed.


**Formula:**


```text
WER     =     (  Substitutions     +     Deletions     +     Insertions  )     /     Total     Words     ×     100      Where  :    -     Substitutions     =     wrong     words    -     Deletions     =     missing     words    -     Insertions     =     extra     words
```


**Worked example:**


Reference Transcription


"I need to reschedule my appointment for Tuesday" "I need to schedule my appointment Tuesday"


- Substitutions: 1 (reschedule → schedule)
- Deletions: 1 (for)
- Insertions: 0
- Total words: 8


**WER = (1 + 1 + 0) / 8 × 100 = 25%**


**WER benchmarks by condition:**


Condition Excellent Good Acceptable Poor


Clean audio <5% <8% <10% >12%


Office noise <8% <12% <15% >18%


Street/outdoor <12% <16% <20% >25%


Strong accents <10% <15% <20% >25%


**Common WER pitfalls:**


- WER doesn't capture semantic importance (getting a name wrong matters more than "um")
- Different ASR providers use different tokenization
- Compound words and contractions can inflate WER artificially


#### Noise Robustness


**Definition:** How WER degrades with background noise.


**How to measure:**


- Test at different Signal-to-Noise Ratios (SNR): 20dB (quiet), 10dB (moderate), 5dB (noisy), 0dB (very noisy)
- Track WER delta from clean audio baseline


**What "good" looks like:**


- <5% WER increase at 10dB SNR
- <10% WER increase at 5dB SNR


---


## Common Failure Modes and How to Test Them


This table is your testing checklist. Each failure mode needs explicit test cases.


Failure Mode Example User Utterance Test Method Metric(s) to Track


**Noise/Poor Audio** \[User in car with traffic\] "I need to book..." Inject noise at 10dB, 5dB, 0dB SNR WER by noise level, Task completion by noise


**Accents/Dialects** Regional pronunciation variations Test with speakers from target demographics WER by accent group, Intent accuracy by accent


**Crosstalk/Multiple Speakers** \[TV in background\] "...and then she said..." Inject multi-speaker audio Speaker diarization accuracy, WER


**Interruptions/Barge-in** "Actually wait - " \[mid-agent-response\] Programmed interruptions at random points Barge-in recovery rate, Context retention


**Wrong Intent Classification** "I said reschedule, not cancel" Confusable intent pairs, similar phrases Intent accuracy, Confusion matrix


**Slot/Entity Errors** "My number is 555-123-4567" → "555-123-4576" Numbers, names, addresses, dates Entity extraction accuracy by type


**Tool Call Failures** \[Booking system timeout\] Inject tool failures, timeouts, errors Tool success rate, Graceful degradation


**Policy/Compliance Violations** "What's the patient's SSN?" Prompt injection, social engineering attempts Policy compliance rate, PII leak detection


**Prompt Drift/Degradation** Agent personality changes over time A/B test prompt versions, monitor over weeks Consistency metrics, Behavior drift score


**Long Silences** Agent takes 3+ seconds to respond Load testing, complex queries P95/P99 latency, Silence detection


**Awkward Turn-Taking** Agent talks over user repeatedly Multi-turn conversations with varied pacing Turn-taking efficiency, Overlap rate


**Context Loss** "My appointment" → "What appointment?" Multi-turn scenarios requiring memory Context retention score


**Repetitive Loops** Agent asks same question 3+ times Edge cases that might trigger loops Reprompt rate, Loop detection


### Priority Order for Testing


If you can only test some failure modes, prioritize by impact:


1. **High Impact, Common:** Wrong intent, slot errors, tool failures
2. **High Impact, Rare:** Policy violations, prompt injection
3. **Medium Impact, Common:** Noise robustness, interruption handling
4. **Medium Impact, Rare:** Accent variations, context loss
5. **Lower Priority:** Edge cases in edge cases


---


## How to Build a Voice Agent Test Set


### What to Include


#### Golden Path Scenarios (40%)


Standard user journeys that should always work:


- Simple single-intent requests
- Common variations of primary use case
- Expected happy-path flows


**Example for appointment booking:**


- "I'd like to book an appointment"
- "Can I schedule a visit for next week?"
- "I need to see the doctor on Tuesday"


#### Edge Cases (30%)


Unusual but valid requests:


- Multi-intent: "Book an appointment and also update my phone number"
- Corrections: "Actually, make that Wednesday, not Tuesday"
- Clarifications: "What times do you have available?"
- Long conversations: 10+ turn interactions
- Hesitations: "Um, I think... maybe Thursday?"


#### Error Handling (15%)


Invalid inputs and system errors:


- Invalid dates: "Book me for February 30th"
- Missing information: User doesn't provide required details
- System timeouts: Simulate slow/failing backend
- Out of scope: Requests the agent can't handle


#### Adversarial (10%)


Challenging or potentially harmful inputs:


- Off-topic: "What's the weather like?"
- Profanity: Test graceful handling
- Prompt injection: Attempts to manipulate agent behavior
- Social engineering: Attempts to extract sensitive information


#### Acoustic Variations (5%)


Audio quality challenges:


- Background noise (office, street, car, restaurant)
- Accents representing your user base
- Device variations (mobile, landline, speakerphone)
- Speech variations (fast, slow, mumbled)


### Sampling from Real Calls


If you have production call data:


1. **Random sample** 100+ calls across time periods
2. **Stratify** by outcome (success, failure, escalation)
3. **Extract** user utterances and intents
4. **Anonymize** any PII before using as test data
5. **Categorize** into the buckets above


### Synthetic Generation (If No Real Calls)


If you don't have real call data yet:


1. Define user personas (demographics, technical comfort, urgency)
2. Write scenario scripts with expected variations
3. Use TTS to generate synthetic audio with different voices
4. Add noise augmentation programmatically
5. Validate with human review before using


### Multilingual Considerations


For each language you support:


- Native speaker baseline (clean audio)
- Accented speech (regional variants)
- Code-switching scenarios (mixing languages)
- Per-language WER baselines (they vary significantly)


See[Multilingual Voice Agent Testing](https://hamming.ai/resources/multilingual-voice-agent-testing) for complete per-language benchmarks and test methodology.


---


## What You Can Automate Today (And What Still Needs Human Review)


### Automation Matrix


Task Automate? Why


**Latency measurement** ✅ Fully Deterministic, no judgment needed


**WER calculation** ✅ Fully Deterministic with reference transcripts


**Task completion detection** ✅ Mostly Rules-based + LLM verification


**Regression testing** ✅ Fully Compare metrics against baseline


**Synthetic call generation** ✅ Fully Programmable personas and scenarios


**Alert generation** ✅ Fully Threshold-based triggering


**Intent classification accuracy** ✅ Fully Compare to labeled test set


**Sentiment analysis** ⚠️ Partially LLM can score, but calibrate with humans


**Conversational flow quality** ⚠️ Partially Some patterns detectable, nuance needs humans


**Edge case discovery** ⚠️ Partially Pattern detection helps, but humans find novel cases


**Root cause analysis** ❌ Human + tooling Requires context and judgment


**Prompt tuning decisions** ❌ Human Requires understanding business tradeoffs


**New failure mode identification** ❌ Human Requires recognizing unknown patterns


**User experience assessment** ❌ Human Subjective, context-dependent


### High-Impact Automations to Prioritize


If you're building out automation, prioritize in this order:


1. **Latency percentile tracking** - Catches performance issues immediately
2. **Task completion monitoring** - Tracks core business metric
3. **Regression testing on deployment** - Prevents shipping broken changes
4. **Synthetic monitoring** - Detects issues before users do
5. **Alerting on threshold breaches** - Enables fast response


### What Human Review Is Still Essential For


Reserve human attention for:


- **Calibrating LLM-as-judge scorers** - Your first prompt needs 3-5 iterations
- **Reviewing novel failure modes** - Automation catches known patterns, humans catch new ones
- **Making tradeoff decisions** - "Is 5% lower accuracy acceptable for 20% faster response?"
- **Validating sentiment scores** - LLM sentiment isn't perfect, spot-check regularly
- **Edge case adjudication** - "Did the agent actually fail, or was this an unreasonable request?"


---


## Tooling Stack for Evaluation and Production Monitoring


### Categories of Tools You Need


Category What It Does When You Need It


**Testing Harness** Generates synthetic calls, executes test scenarios Pre-launch and regression


**Evaluation Platform** Calculates metrics, scores conversations, detects failures Continuous


**Monitoring/Alerting** Real-time dashboards, threshold alerts, anomaly detection Production


**Analytics** Trending, cohort analysis, business impact correlation Optimization


### Evaluation Loop Diagram


```text
┌─────────────────────────────────────────────────────────────────┐    │                          Voice     Agent     Evaluation     Loop                         │    ├─────────────────────────────────────────────────────────────────┤    │                                                                        │    │        ┌──────────┐          ┌──────────┐          ┌──────────┐          ┌──────────┐        │    │        │        Define        │───▶│        Build         │───▶│         Run          │───▶│        Triage        │        │    │        │     Success        │          │     Test     Set     │          │        Evals         │          │     Failures     │        │    │        └──────────┘          └──────────┘          └──────────┘          └──────────┘        │    │             │                                                     │               │    │             │               ┌──────────────────────────┐                │               │    │             │               │                                │                │               │    │             ▼               ▼                                │                ▼               │    │        ┌──────────────────┐                    ┌──────────────────┐              │    │        │          Production          │◀────────────│          Regression          │              │    │        │          Monitoring          │                    │            Test              │              │    │        └──────────────────┘                    └──────────────────┘              │    │               │                                        ▲                          │    │               │          Drift     detected  ?                     │                          │    │               └──────────────────────────────────┘                          │    │                                                                        │    └─────────────────────────────────────────────────────────────────┘
```


### Tool Selection Criteria


When evaluating tools, score on:


Criterion Weight What to Look For


**Voice-native capabilities** 25% Synthetic calls, audio analysis, not just transcripts


**Metric coverage** 20% All dimensions: latency, accuracy, quality, outcomes


**Automation depth** 20% CI/CD integration, regression blocking, alerting


**Time-to-value** 15% Setup time, learning curve, documentation


**Integration** 10% API access, webhooks, your existing stack


**Cost efficiency** 10% Pricing at your scale


### Why Voice-Native Tooling Matters


Generic LLM evaluation tools (Braintrust, Langfuse) are designed for text. They miss:


- Audio-level issues (latency spikes, TTS quality, interruption handling)
- Acoustic testing (noise robustness, accent handling)
- Real telephony testing (actual calls, not simulated)


Capability Generic LLM Eval Voice-Native (Hamming)


Synthetic voice calls ❌ ✅ 50K+ concurrent test calls


Audio-native analysis ❌ Transcript only ✅ Direct audio


ASR accuracy testing ❌ ✅ WER tracking


Latency percentiles ⚠️ Basic ✅ P50/P95/P99


Background noise simulation ❌ ✅ Configurable SNR


Barge-in testing ❌ ✅ Deterministic


Production call monitoring ⚠️ Logs only ✅ Every call scored


Regression blocking ⚠️ Manual ✅ CI/CD native


### How Hamming Implements This Loop


Hamming is a voice agent testing and monitoring platform built for this evaluation lifecycle:


- **Synthetic testing at scale** - Simulate thousands of calls with configurable personas, accents, and acoustic conditions
- **Production monitoring** - Track all calls in real-time with automated scoring and alerting
- **Regression detection** - Compare new versions against baselines, block deployments on degradation
- **Full traceability** - Jump from any metric to the specific call, transcript, and audio


[Learn more about Hamming →](https://hamming.ai/)


---


## Voice Agent Evaluation Checklist (Copy/Paste)


### Pre-Launch Checklist


```text
#  #     Pre  -  Launch     Voice     Agent     Evaluation      #  #  #     Success     Criteria     Defined    -     [     ]     Task     completion     target     set     (  >  __  %  )    -     [     ]     Latency     thresholds     defined     (  P95     <  __ms  )    -     [     ]     Escalation     rate     target     set     (  <  __  %  )    -     [     ]     Compliance     requirements     documented    -     [     ]     Failure     mode     acceptance     criteria     defined      #  #  #     Test     Coverage    -     [     ]     Happy     path     scenarios     (  40  %     of     test     set  )    -     [     ]     Edge     cases     (  30  %     of     test     set  )    -     [     ]     Error     handling     (  15  %     of     test     set  )    -     [     ]     Adversarial     inputs     (  10  %     of     test     set  )    -     [     ]     Acoustic     variations     (  5  %     of     test     set  )    -     [     ]     Multilingual     coverage     (  if     applicable  )      #  #  #     Metrics     Baseline     Established    -     [     ]     Task     completion     rate     measured    -     [     ]     Latency     percentiles     (  P50  ,     P95  ,     P99  )     recorded    -     [     ]     WER     baseline     by     condition    -     [     ]     Barge  -  in     recovery     rate     measured    -     [     ]     Tool     call     success     rate     verified      #  #  #     Infrastructure     Verified    -     [     ]     Latency     within     targets     under     load    -     [     ]     No     audio     artifacts     or     quality     issues    -     [     ]     Interruption     handling     works     correctly    -     [     ]     Timeout     handling     graceful      #  #  #     Compliance     Checked    -     [     ]     No     PII     leakage     in     logs  /  transcripts    -     [     ]     Policy     compliance     verified    -     [     ]     Prompt     injection     resistance     tested    -     [     ]     Escalation     paths     working
```


### Post-Launch Monitoring Checklist


```text
#  #     Production     Monitoring     Setup      #  #  #     Real  -  Time     Dashboards    -     [     ]     Call     volume     and     success     rate     displayed    -     [     ]     Latency     percentiles     updating     (  every     5     min  )    -     [     ]     Error     rate     by     type     visible    -     [     ]     Escalation     rate     tracked    -     [     ]     Active     incidents     highlighted      #  #  #     Alerting     Configured    -     [     ]     P95     latency     alert     (  warning  :     >  5s  ,     critical  :     >  7s  )    -     [     ]     Task     completion     alert     (  warning  :     &  lt  ;  80  %  ,     critical  :     &  lt  ;  70  %  )    -     [     ]     WER     alert     (  warning  :     >  12  %  ,     critical  :     >  18  %  )    -     [     ]     Error     rate     alert     (  warning  :     >  5  %  ,     critical  :     >  10  %  )    -     [     ]     Escalation     spike     alert      #  #  #     Synthetic     Monitoring     Running    -     [     ]     Test     calls     every     5  -  15     minutes    -     [     ]     Critical     paths     covered    -     [     ]     Scenarios     rotating    -     [     ]     Failures     alerting      #  #  #     Data     Collection     Active    -     [     ]     Call     recordings     captured     (  with     consent  )    -     [     ]     Transcripts     stored    -     [     ]     Metrics     logged     with     timestamps    -     [     ]     Errors     captured     with     context
```


### Weekly Regression Checklist


```text
#  #     Weekly     Regression     Review      #  #  #     Metrics     Trending    -     [     ]     Task     completion     week  -  over  -  week    -     [     ]     Latency     trending     (  any     degradation  ?  )    -     [     ]     WER     trending     (  any     increase  ?  )    -     [     ]     Escalation     rate     trending    -     [     ]     Error     rate     trending      #  #  #     Changes     Since     Last     Week    -     [     ]     Prompt     changes     documented     and     tested    -     [     ]     Model     updates     verified    -     [     ]     Integration     changes     regression     tested    -     [     ]     Configuration     changes     validated      #  #  #     Failure     Analysis    -     [     ]     Top     5     failure     modes     identified    -     [     ]     Root     causes     documented    -     [     ]     Fix     hypotheses     created    -     [     ]     New     test     cases     added     for     failures      #  #  #     Action     Items    -     [     ]     High  -  priority     fixes     scheduled    -     [     ]     Monitoring     gaps     addressed    -     [     ]     Test     coverage     expanded    -     [     ]     Documentation     updated
```


---


## Frequently Asked Questions


### How do I evaluate beyond "it kinda works"?


Move from binary (works/doesn't work) to dimensional measurement:


1. **Define specific success criteria** - Not "works" but "completes booking task >85% of time"
2. **Measure across multiple dimensions** - Latency, accuracy, conversation quality, user satisfaction
3. **Track percentiles, not averages** - P95 latency matters more than average
4. **Test failure modes explicitly** - Don't just test happy paths
5. **Monitor continuously** - Production behavior differs from testing


The shift is from "it works in demos" to "it works reliably at scale under real conditions."


### How many test calls do I need?


Depends on your confidence requirements:


Stage Minimum Recommended Enterprise


Pre-launch validation 50 scenarios 200 scenarios 500+ scenarios


Regression testing 20 critical paths 50 critical paths 100+ paths


Synthetic monitoring 10 calls/hour 50 calls/hour 200+ calls/hour


For statistical significance on metric changes, you typically need 100+ observations to detect a 5% change with 95% confidence.


### What's a good latency target?


**Based on Hamming's production data (10M+ mins protected):**


**End-to-end with telephony (real-world targets):**


- P50: 1.5-1.7 seconds (good), <1.5 seconds (excellent)
- P90: ~3 seconds (acceptable), <2.5 seconds (good)
- P95: ~5 seconds (acceptable), <3.5 seconds (good)


**Aspirational target:** P95 at 1.7 seconds is achievable with highly optimized pipelines, but most production systems see P95 around 5 seconds for cascading architectures (STT → LLM → TTS).


**Speech-to-speech models** can achieve sub-500ms end-to-end by eliminating intermediate steps.


Research on[conversational turn-taking](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4030396/) shows 200-500ms is the natural pause in human dialogue. Past 1 second, users perceive delay.


### How do I monitor prompt drift?


Prompt drift is gradual behavior change over time. Monitor with:


1. **Consistency scoring** - Same input should produce similar outputs week-over-week
2. **A/B baseline comparison** - Compare current behavior to a frozen "known good" version
3. **Behavioral guardrails** - "Agent should always greet with X" - track compliance over time
4. **User feedback correlation** - Correlate satisfaction scores with time since last prompt change


See[Voice Agent Drift Detection Guide](https://hamming.ai/blog/voice-agent-drift-detection-guide) for detailed methodology.


### What causes voice agent latency spikes?


Common causes (in order of frequency):


1. **LLM cold starts or rate limiting** - Provider-side, often affects P99
2. **Complex function calls** - Tool use adds round-trip time
3. **ASR provider capacity** - Degrades during peak hours
4. **Long user utterances** - More audio = more processing time
5. **Network variability** - Between your components
6. **Inefficient prompt** - Too much context = slower inference


Debug by measuring latency at each pipeline stage separately.


### How do I test for different accents?


1. **Identify your user demographics** - Where are your users calling from?
2. **Source accent-representative audio** - Record from native speakers, or use high-quality TTS with accent options
3. **Measure WER per accent group** - Track separately, not aggregated
4. **Set per-accent thresholds** - Some accents are harder; baselines differ
5. **Target equitable performance** - No more than 3% WER variance between groups


### What's the ROI of automated evaluation?


Based on customer deployments:


Metric Manual QA Automated Improvement


Test capacity ~20 calls/day 200+ concurrent 10x+


Coverage 1-5% of calls 100% of calls 20-100x


Issue detection speed Days to weeks Minutes to hours 10-100x faster


Regression prevention Reactive Proactive blocking Prevents incidents


The[NextDimensionAI case study](https://hamming.ai/case-studies/next-dimension) demonstrates: 10x test capacity, 40% latency reduction, 99% production reliability.


### How do I evaluate multilingual voice agents?


For each language:


1. **Establish per-language WER baselines** - They vary significantly (English ~8%, Mandarin ~15%, Hindi ~18%)
2. **Test code-switching** - Users mix languages ("Quiero pagar my bill")
3. **Validate intent recognition** - Same intent expressed differently per language
4. **Measure latency variance** - Some language models are slower
5. **Monitor for language-specific drift** - Issues may affect one language but not others


See[Multilingual Voice Agent Testing Guide](https://hamming.ai/resources/multilingual-voice-agent-testing) for per-language benchmarks.


---


## Flaws but Not Dealbreakers


This framework looks comprehensive on paper. Here's what's harder in practice:


**The full framework is overkill for most teams starting out.** If you're pre-product-market-fit, measure latency and task completion. Add dimensions as you scale and encounter their failure modes.


**Experience metrics are still a mess.** CSAT surveys have 5-10% response rates. Inferring satisfaction from abandonment and escalation is better than nothing, but imperfect.


**This gets expensive fast.** Running synthetic tests every 5 minutes with 50 scenarios across 20 languages - do the math before committing. Start with critical paths and expand.


**Your architecture changes everything.** These latency targets assume cascading STT → LLM → TTS. Speech-to-speech models can do much better. Complex function calling can do much worse.


**Not all failure modes are equally important.** The table above lists many failure modes. Prioritize by business impact, not comprehensiveness.


---


## Related Deep-Dives


- [Voice Agent Testing Guide (2026)](https://hamming.ai/resources/voice-agent-testing-guide) - Methods, Regression, Load & Compliance Testing
- [The 4-Layer Voice Agent Quality Framework](https://hamming.ai/resources/guide-to-ai-voice-agents-quality-assurance) - Infrastructure → Execution → User Reaction → Business Outcome
- [Testing Voice Agents for Production Reliability](https://hamming.ai/resources/testing-voice-agents-production-reliability) - Load, Regression, A/B Testing
- [Multilingual Voice Agent Testing](https://hamming.ai/resources/multilingual-voice-agent-testing) - Per-language WER benchmarks, code-switching
- [Voice Agent Monitoring KPIs](https://hamming.ai/resources/voice-agent-monitoring-kpis-production-guide) - Production dashboard metrics
- [Background Noise Testing KPIs](https://hamming.ai/resources/background-noise-voice-agent-testing-kpis) - Acoustic stress testing
- [Voice Agent Drift Detection](https://hamming.ai/blog/voice-agent-drift-detection-guide) - Monitoring gradual degradation
- [7 Non-Negotiables for Voice Agent QA Software](https://hamming.ai/resources/7-non-negotiables-voice-agent-quality-assurance-software) - Tool selection criteria


---
