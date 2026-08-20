---
schema_version: "1.0.0"
document_id: "ce957fe27374e142cc43c64601c719f0b44e8c0448989bbebf0326bd86db1192"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/voice-agent-slos"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T21:03:14.317045+00:00"
fetched_at: "2026-07-29T21:03:16.156505+00:00"
content_hash: "sha256:abeda77dd2a46e16d89686d7d6c563e543f2fb0d2f53e6229882683866c0ca36"
---

# Voice agent SLOs: designing reliability targets for agents that talk

Last week OpenAI launched Presence, an enterprise voice-agent platform, with a headline claim: its Codex-driven improvement loop[reduced human handoffs on OpenAI's own English-language support line by 15 percentage points over 10 days](https://venturebeat.com/orchestration/openai-unveils-presence-a-new-platform-that-lets-enterprises-launch-and-manage-realtime-voice-agents-and-chatbots) , and Presence now resolves 75% of that line's inbound calls without human help.


It is a striking pair of numbers. It is also the wrong shape of number for most voice-AI teams to run their program on. "Handoffs down 15 points" is a snapshot from one operator on one line during one window. It does not tell you whether next Tuesday's calls are still meeting the bar you promised the business, and it does not tell you which failure mode ate the last three points. Presence itself acknowledges the underlying risk:[an agent that works at launch may become less reliable when policies, products or user behavior change](https://venturebeat.com/orchestration/openai-unveils-presence-a-new-platform-that-lets-enterprises-launch-and-manage-realtime-voice-agents-and-chatbots) . The improvement loop only works if you can see when reliability slips and by how much.


Every voice-AI program needs a definition of "reliable enough" that survives past launch day. That definition is a set of service-level objectives, or SLOs, and voice agents need a different shape of SLO than the web-service ones your infra team already runs.


## Why traditional SLOs miss voice agents


The classic SRE stack is availability, error rate, and latency percentiles. For an HTTP API, that is most of the truth: if the endpoint returns 200s within the p99 budget, the customer is happy. For a voice agent it is a tiny fraction. Your platform can be 100% available, your median first-audio can be 400ms, and every call can still be broken because the agent talked over half the callers or mispronounced the drug name.


Agent reliability has more dimensions than infrastructure reliability.[A traditional SaaS product usually starts with availability and latency; an agent also needs task success, tool-call reliability, output quality, escalation behavior, and cost control](https://www.buildmvpfast.com/blog/ai-agent-sla-uptime-accuracy-response-time-guarantee-2026) . Voice adds another layer on top:[silence functions as a paralinguistic signal](https://www.twilio.com/en-us/blog/developers/best-practices/guide-core-latency-ai-voice-agents) , and endpointing errors, interruptions, and dead air degrade a call that every dashboard would call healthy.


The practical failure of "just use uptime" shows up the first time the exec team asks how the agent is doing this month. If your answer is "5 nines, p99 under a second," and the QA lead is on the same call saying "we found 40 bad calls last week," you do not have SLOs. You have infra metrics with a voice-AI logo on them.


Traditional SaaS SLOs vs voice-agent SLOs


## The SLIs voice agents actually need


An SLI is the thing you measure; the SLO is the target you set on it. For voice, six categories cover most of what matters.


**Turn-taking latency.** End-to-end from caller stops speaking to first audio out, measured at p50 and p95. Averages hide bad tails; callers remember the pauses.[Under about 300ms feels natural, 500ms is tolerable, past 800ms callers start to talk over the agent or assume the line dropped](https://cloudgramam.com/blog/how-fast-should-ai-voice-agent-respond) . Your target depends on model choice and where you route calls, but you need this measured, not asserted.


**Task completion rate.** Did the caller get what they came for? For a booking agent, was a booking created and confirmed. For a support agent, was the case resolved or correctly escalated. This is the SLI closest to what the business actually pays for, and it is the one that "handoffs down 15 points" is a rough proxy for.


**Escalation correctness.** Two failure modes, not one. Missed escalations (should have handed off, did not) and spurious escalations (routed to a human when it should not have). A single "handoff rate" number can go down for either good or terrible reasons.


**Audio-native quality.** Pronunciation of names, numbers, and domain terms; interruption rate; dead-air incidents; pace and vocal stress. These are the metrics traditional log-based monitoring cannot see because the transcript looks fine.


**Tool-call reliability.** Correct tool selected, correct arguments passed, correct handling of the response, including the graceful degradation path when the API 500s. Full-Duplex-Bench-v3 measured tool-call latency separately from first-word latency for exactly this reason, and found[systems whose fast first-word numbers hide 6+ second tool-call latencies because the model speaks a filler before calling the tool](https://arxiv.org/pdf/2604.04847) . Users still wait for the answer, not the filler.


**Policy and disclosure adherence.** AI disclosure when required, PII handling, script compliance, correct execution of human-handoff triggers. In regulated verticals these are non-negotiable and belong in SLOs, not in a QA spreadsheet.


## Setting targets: baseline first, then tighten


The common mistake is to write down aspirational targets before you have measured what the agent actually does. Praesidia's guidance on AI-service SLOs puts it plainly:[set your initial SLO target close to your current measured baseline, then tighten gradually as you address the root causes of failures](https://praesidia.ai/blog/slo-service-level-objectives-ai-services) . Google's original SRE framing says the same thing, and it is especially true for voice, where the delta between "acceptable" and "excellent" is small and the tail matters more than the median.


A workable first pass looks like this:


1. Run a representative simulation suite against your current agent: 200 or so scenarios spanning your real intents, personas, languages, and edge cases.
2. Compute your candidate SLIs from those runs: p50 and p95 turn latency, task completion, escalation correctness, audio-native metrics, tool-call success.
3. Set each SLO target at your current p50 or p75, not your best day. If you cannot meet your baseline consistently, the target is decorative.
4. Give each SLO an error budget. A task-completion target of 92% over 28 days means an 8% budget. When you burn it faster than the calendar, feature velocity stops and reliability work starts.


Percentile-wise, treat voice like a real-time system, not a batch one. A p95 target does more work than a p50, because a caller who hits a two-second pause once in ten calls remembers exactly that call.


## From SLO breach to regression test


The reason SLOs matter more than dashboards is what happens when they break. A dashboard tells you a number moved. An SLO with an error budget tells you the number moved past what you promised, and it forces a specific response: identify the failing calls, understand why, and prevent the same class of failure next release.


This is where a lot of voice teams stall. They see the SLO breach, listen to a few calls, tweak a prompt, and hope. What they need is a loop: every call that trips an SLO becomes a filed issue, every filed issue becomes a replayable regression test, and every release runs those regressions before touching production.


[Roark](https://roark.ai/) is built around exactly this loop. It scores every production call against a suite of 64+ built-in audio-native metrics plus any custom SLIs you define, and files an issue automatically when a call fails a metric. Because it captures the production audio, you can replay the same call against updated agent logic and prove the fix before it ships. That turns "the SLO went red last Thursday" from an anecdote into a permanent regression scenario that runs on every future change.


The pre-launch side of the loop matters just as much. Roark's simulation testing dials your agent over real telephony or WebRTC using personas that control voice, accent, pace, emotion, and background noise, so you can compute your SLIs against a stable, repeatable population before a single real caller hits the number. When OpenAI Presence, or your platform of choice, proposes a policy change, you run the suite, look at the deltas on each SLI, and decide whether the change actually improved the agent or just moved the failures around.


Illustrative pre-launch simulation run checking SLO thresholds


## Reporting to the business


Once SLOs exist, the exec-team update writes itself. Instead of "we handled a lot of calls and most were fine," you report:


- Task completion this month: 91.3% against a 90% target. Green.
- Turn-taking p95: 720ms against an 800ms target. Green.
- Escalation correctness: 94% against 95%. Yellow, one week of budget remaining.
- Disclosure adherence: 100% against 100%. Green.
- Open issues from SLO breaches: 7, of which 4 have regression tests in the suite.


That format survives audit, gives the QA lead and the platform team a shared vocabulary, and lets you say something honest about what "improved by 15 percentage points" would mean for your line, not OpenAI's.


For the infra-adjacent side of the picture, Roark exposes[OpenTelemetry traces](https://docs.roark.ai/) for every call, so your existing SRE tooling can chart the same SLOs your product team reviews. Same numbers, one source of truth.


## Start with three SLOs, not thirty


Programs that stall on SLOs do it by trying to instrument everything at once. Start with three: turn-taking p95, task completion, and one audio-native metric that matters for your vertical. Pronunciation accuracy for healthcare. Interruption rate for outbound sales. Disclosure adherence for regulated verticals. Add error budgets. Wire the alerts. Ship.


The other SLIs come next, one release at a time, once you have felt what it is like to work against the first three. A short list you actually enforce beats a long list you read.


Voice-AI platforms are going to keep shipping improvement loops that promise to close the gap for you. Some will genuinely help. None of them will replace the small operational discipline of writing down what your agent must do, measuring whether it did it, and reacting when it did not. That discipline is what SLOs are for. If you build one voice-AI habit this quarter, build that one.
