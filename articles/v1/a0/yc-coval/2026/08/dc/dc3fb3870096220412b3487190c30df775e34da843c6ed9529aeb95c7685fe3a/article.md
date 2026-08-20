---
schema_version: "1.0.0"
document_id: "dc3fb3870096220412b3487190c30df775e34da843c6ed9529aeb95c7685fe3a"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/improve-agent-performance-with-voice-benchmarks-ama-replay/"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-15T06:34:26.238160+00:00"
fetched_at: "2026-08-15T06:34:29.783756+00:00"
content_hash: "sha256:571c555dc9e15f924b876836df42075bbf04c19e72a5d5399eb1d12b8e0f796d"
---

# Improve Agent Performance With Voice Benchmarks: AMA Replay and Show Notes

# Improve Agent Performance With Voice Benchmarks: AMA Replay and Show Notes


[Henry Finkelstein](https://www.linkedin.com/in/henryfinkelstein) ✓ Expert verified


Founding Growth Engineer · August 13, 2026 · 6 min read


Replay Coval's live voice benchmarks AMA, with key takeaways and chaptered notes on latency, accuracy, speech-to-speech, production data, and evaluation design.


Coval’s benchmarking team joined Brooke Hopkins for a practical AMA on how to choose voice models, interpret benchmark results, and build evaluation sets that predict what users will experience in production.


The team walked through Coval’s independent, open-source benchmarks for speech-to-text, text-to-speech, and speech-to-speech systems. The second half of the session answered audience questions about refreshable test sets, production-call replay, turn detection, human baselines, multi-provider stacks, adversarial testing, and voice-level differences.


## Resources from the AMA


- [Explore the live voice leaderboards](https://benchmarks.coval.ai/)
- [Read the open-source methodology and reproduce the benchmarks](https://github.com/coval-ai/benchmarks)
- [Compare voices in the Coval Voice Arena](https://benchmarks.coval.ai/overview/arena)
- Ask to add a model or discuss a result


## The short version


- **A benchmark should change a decision.** Use public benchmarks to build a shortlist, internal benchmarks to test that shortlist on your data, and task-specific evaluation to decide whether a complete agent is ready.
- **A single average hides the failures callers notice.** Tail latency, regional infrastructure, silent audio at the start of a stream, accents, clipping, names, and numbers can matter more than the headline score.
- **Measure what reaches the user, not only what leaves the model.** When Coval added silence at the start of returned audio to its latency measurement, the text-to-speech leaderboard changed.
- **Speech-to-speech needs more than a transcript.** Naturalness, instruction adherence, turn-taking, interruptions, background noise, and recovery all affect whether the system works.
- **Production data should keep evaluation sets current.** Sample recent calls and failures into refreshable test sets while preserving a smaller, curated regression suite for behavior that must not break.
- **Replay is a spectrum, not one method.** Use exact scripts for deterministic checks, synthetic scenarios for behavioral coverage, and production-call re-simulation when the full trajectory matters.
- **Multi-provider stacks make evaluation infrastructure more important.** Teams need a stable way to compare model changes and route different tasks without becoming locked to a stack they can no longer safely change.


Event replay


## Watch the conversation


Apologies for the flickering camera—we’ll have it fixed for the next webinar.


[Open the full recording](https://drive.google.com/file/d/1W0TOmdDaxRfC4UD-jd1gnfxClnAVgx3F/view)Read the show notes


## Show notes


Slide 01 / 20


Slide 02 / 20


Slide 03 / 20


Slide 06 / 20


### 00:00 | Why voice benchmarks exist


Voice agents combine several model and infrastructure decisions. A cascaded system perceives speech through speech-to-text, reasons through a language model, and speaks through text-to-speech. Native speech systems combine more of that loop inside one model. Either way, a slow or inaccurate component compounds across every turn.


Coval publishes its benchmarks independently and does not sell model placement. The methodology and code are open source, and the tests run continuously so the results reflect serving behavior rather than one favorable moment.


Slide 04 / 20


Slide 05 / 20


Slide 07 / 20


### 04:32 | The benchmark ladder


The team described three levels of evidence:


1. **Public benchmarks** help you identify a small group of models worth investigating.
2. **Internal benchmarks** run those models against your own audio, languages, workflows, and failure conditions.
3. **Task-specific evaluation** measures whether the complete agent finishes the job safely and reliably.


Public benchmarks optimize for a broad average. They are a useful filter, not the final production decision.


Slide 10 / 20


### 08:04 | Latency is a distribution


A fast model on slow serving infrastructure is a slow product. A strong benchmark therefore measures the endpoint and the distribution users encounter, not only model throughput or the best observed run.


Coval reruns benchmarks every 30 minutes to catch variation and spikes. A 15-second latency spike may not trigger a provider status page, but it feels like a full outage to the caller who experiences it. This is why P95 behavior and the shape of a latency distribution belong beside the median.


Slide 08 / 20


### 10:00 | Break aggregate scores into failure slices


One global word error rate can hide the errors that matter most to a use case. The useful question is often whether a model fails on names, numbers, accents, clipping, noisy audio, or a particular language. Those categories let an engineering team choose for its callers rather than for the average benchmark input.


There is no universally best model across cost, speed, and quality. The point is to make the tradeoff visible.


Slide 09 / 20


### 11:56 | Measure perceived time to first audio


Many text-to-speech benchmarks stop the clock when the first audio bytes arrive. Coval found that some providers returned audio with hundreds of milliseconds of silence before speech began. When the benchmark measured the first sound a caller could actually hear, the leaderboard changed.


That example captures the broader rule: measure the behavior the user experiences, including telephony, networking, streaming, and serving infrastructure.


Slide 11 / 20


Slide 12 / 20


Slide 13 / 20


### 13:16 | Speech-to-speech and the Voice Arena


Speech-to-speech systems need latency measurement, but latency alone is not enough. The agent must stay on task across background noise, accents, interruptions, and long conversations. The team is adding instruction-adherence and environmental slices to make that tradeoff visible.


Naturalness is harder to reduce to one automated metric. Coval’s Voice Arena lets people compare systems directly and record which response sounds more natural for the use case.


Slide 14 / 20


Slide 15 / 20


### 16:36 | Benchmark on your own data


After using a public leaderboard to select three to five candidates, replay the same test cases against each model. Keep the data, scenarios, graders, and success criteria fixed. Then score the full workflow before making a production decision.


This is where a generalized ranking becomes evidence about the system you actually operate.


Slide 16 / 20


### 19:08 | The voice evaluation pyramid


Text evaluation is fast and useful for instruction following, but clean text turns do not exercise interruption, turn-taking, late transcripts, partial transcripts, or recovery. A practical test portfolio uses many inexpensive text tests, a smaller voice suite, realistic and load-oriented voice tests, and a final set of human calls.


The same portfolio helps teams compare architectures. Native speech captures more timing and emotional signal. Cascaded systems still offer more control for high-compliance workflows. Hybrid systems are likely to combine native conversation with background models and deterministic safeguards.


Slide 17 / 20


### 22:36 | Refreshable test sets and turn detection


Evaluation data should evolve with production. A team might sample a percentage of recent failed calls or a fixed number of calls each month, then add those examples to a refreshable set. Drift is useful when it keeps the data representative.


That does not replace fixed regression coverage. Keep a smaller set of critical scenarios for behavior that must continue to work across every release.


Turn detection is another system behavior that looks simple but directly affects resolution and recovery. The team identified turn-detection benchmarks and European-region measurement as active extensions to the public suite.


### 24:52 | Replay production calls and compare with humans


To reproduce a live call, Coval combines the transcript with audio re-simulation and lets the current agent respond to the same situation. Teams can compare the steps and number of turns taken by a human and an agent. An agent that finishes a ten-turn workflow in one turn may have skipped something; an agent that takes ten times longer may be looping.


Human testing can become a scalable baseline too. A subject-matter expert can call a simulation number several times, vary interruptions and phrasing, and then replay those captured cases at larger volume.


The maintenance risk is scenario decay: a test can stop reaching the behavior it was created to measure. More abstract, outcome-based scenarios are often more resilient than brittle scripts, while agents can help detect and repair decayed tests.


Slide 18 / 20


Slide 19 / 20


### 29:36 | Reproducibility and production monitoring


Coval publishes the benchmark methodology, measurement code, and part of the datasets. Some data stays private so providers cannot tune directly against every test. The goal is to balance reproducibility with resistance to gaming.


The same metrics used before launch should continue in production. Simulation and observability belong in one loop because production evidence shows where coverage is missing and which slices need new regression tests.


### 32:24 | Personas and behavior scenarios


Personas define the simulated environment: caller context, speaking behavior, interruption tendency, background noise, and other conditions. Scenarios define the job or trajectory the caller is trying to complete.


Different test types provide different control. Exact scripts and specific audio are deterministic. Transcripts and synthetic scenarios allow more variation. The right choice depends on whether the team needs an exact regression check or broad behavioral coverage.


### 34:36 | Self-improving evals and multi-provider stacks


Different tasks can justify different models. Outbound sales may reward immediate naturalness, while a regulated support workflow may reward controllability. Multi-provider routing lets teams choose by task, but it only works when they can measure a change without rebuilding the test process each time.


Evaluation infrastructure preserves that freedom. Without it, teams become locked to legacy models because changing the stack is too risky to validate.


### 38:56 | Unit tests versus full-call simulation


The team compared the test portfolio to traditional software engineering. A single-turn test with mocked tool calls is faster to set up and easier to reproduce. A full synthetic or re-simulated call costs more but captures the interaction between dialogue, tools, timing, and state.


Use both. The decision is how much setup effort to spend for the level of signal the change requires.


### 40:48 | Adversarial failures and deterministic safeguards


Audience questions covered leaked system prompts, raw tool-call output, punctuation read aloud, vocal hallucinations, and unexpected routing behavior. The recommendation was to combine adversarial scenarios with simple deterministic checks. A regular expression or output filter can still be the right safeguard when the prohibited output is easy to identify.


### 44:20 | Comparing voices within a model


Different voices within the same model can have different latency and naturalness characteristics. The benchmarking team now balances voice categories in its runs and is extending the Arena to study voice-level differences and voice cloning.


Slide 20 / 20


## Explore the live benchmarks


See current speech-to-text, text-to-speech, and speech-to-speech results in[Coval’s independent voice AI benchmarks](https://benchmarks.coval.ai/) .
