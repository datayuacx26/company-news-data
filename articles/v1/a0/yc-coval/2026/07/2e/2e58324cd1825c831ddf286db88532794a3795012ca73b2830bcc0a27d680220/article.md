---
schema_version: "1.0.0"
document_id: "2e58324cd1825c831ddf286db88532794a3795012ca73b2830bcc0a27d680220"
company_key: "yc-coval"
company: "Coval"
source_id: "yc-coval-news-import-66f2770dd546"
canonical_url: "https://www.coval.ai/blog/future-of-agentic-voice-webinar-recap/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-21T15:11:00.592145+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:2e963748881c6cddecdc6f6e77324480ca7408cd3f97782c81c7666422136837"
---

# The Future of Agentic Voice: Webinar Replay and Takeaways

# The Future of Agentic Voice: Webinar Replay and Takeaways


[Henry Finkelstein](https://www.linkedin.com/in/henryfinkelstein) ✓ Expert verified


Founding Growth Engineer · July 16, 2026 · 13 min read


A concise replay of Brooke Hopkins' live AMA on the future of agentic voice, with key takeaways, chapter links, slides, and transcript.


Brooke Hopkins shared the market shifts Coval sees across production voice agents — the company works with teams from Perplexity to Zoom as they scale to tens of millions of calls — then opened the floor to live questions on architecture, evaluation, reliability, and adoption.


## The short version


- **Naturalness gets commoditized. Reliability becomes the moat.** Sounding natural is expected. Finishing the job is valuable. Recovering well is defensible.
- **Hybrid wins before native audio wins everything.** Cascaded buys control and inspectability; native audio buys timing and expression. Hybrid lets teams choose where each matters.
- **The transcript survives. Transcript-only evaluation does not.** The new ground truth is three layers of evidence: what was said, how it sounded, and what the system did.
- **The best voice agents do work while they talk.** Voice is becoming the front end to a real-time workflow engine, not just a channel.
- **Voice agents will improve themselves.** Find the failure, propose the next test and a targeted tweak, rerun the simulations, and report what changed — with a human approving the loop.
- **Trust moves from a principle to a product surface.** Identity, consent, permissions, evidence, and human override have to live inside the system, not in a policy document beside it.


Event replay


## Watch the conversation


[Open the full recording](https://drive.google.com/file/d/1YE7f2-Z8KB3EAFnnLnQkFmxw23xd1Kuf/view)[Download the slides](https://www.coval.ai/events/future-of-agentic-voice/slides.pdf)Read the transcript


## Four hot takes from the deck


Brooke staked out four bets, one slide each. The full deck is downloadable above.


Current platforms expose both native audio and transcription, which makes architecture a tradeoff rather than a one-way migration — and in high-stakes systems, inspectability may decide adoption faster than raw capability.


As Henry put it live: “‘Yeah, right’ versus ‘yeah, right, totally’ — those look the same on a transcript, but not in the audio.”


The best agents do more than fill silence — they narrate useful work while processing it, which gives the caller confidence and expands what users are willing to ask an agent to do.


> “You’re no longer marketing based on features the way 2020-era B2B SaaS did — you’re marketing on quality.” — Brooke Hopkins


## Coming next in this series


This AMA was the first session in a series. Two follow-ups Brooke committed to during the live Q&A:


- **Speech-to-speech explainability** — tracing why a black-box audio model produced the wrong response, and what graceful recovery looks like.
- **Real-time detection of IVR-to-human handoffs** — beyond the post-call classification metrics that exist today.


In the meantime, the IVR testing guides mentioned during the Q&A are already live — start with[Automated IVR Testing](https://www.coval.ai/blog/automated-ivr-testing) .


## Full transcript


*This transcript has been edited for clarity and length. Timestamps mark the actual start of each topic in the trimmed replay. Audience questions are credited as they were asked live.*


### 00:00 | Welcome and audience questions


**Henry Finkelstein:** Welcome, everyone. Drop where you are calling from and the one question you want answered today. We will collect those for the live Q&A. Brooke, let’s get started.


### 02:01 | Why this series and the Waymo lens


**Brooke Hopkins:** Thanks for joining. This is the first in a webinar series we are starting because we hear many of the same questions from customers, and at Coval we see hundreds of different voice systems. The landscape changes week by week, so we wanted a place to share what we see across the market.


I spent my career at Waymo and led our evaluation infrastructure team. My team built the developer tools for launching and running simulation at scale so we could understand system performance and build confidence that it was safe for the road.


When I left Waymo, I saw that voice AI had many of the same properties as self-driving systems. An agent is navigating the world around it and deciding which turn to take next to get from point A to point B. A voice agent also has an objective, and no two situations are exactly the same. Coval helps teams scale those systems end to end — everything from Perplexity to Zoom, scaling to tens of millions of calls — which gives us a view across many use cases and enterprises.


### 03:49 | What breaks in production


The first pattern is that almost every case that ends up on a phone call is an edge case. People often call about something they could not resolve in an app. Every interaction has a different environment, language, and sound profile. You need coverage across many simulated environments.


Failures can also happen at every point in a cascaded system. A transcription issue can propagate through the rest of the pipeline, but the impact depends on what was missed. Dropping a greeting may have no effect. Dropping one digit or misspelling a name can change the entire outcome.


### 05:25 | Cascading impact and failure probability


**Brooke Hopkins:** Accuracy at the component level is not enough. You need to understand how an error changes the overall success rate. Voice conversations are a black box at scale, so when a customer reports a problem, the real question is whether it happens once or in half of all calls. Like self-driving systems, voice teams need to reason about the probability of different failures.


### 06:17 | Simulation and observability


Simulation and observability go hand in hand. A scripted demo will not represent the real world or evolve with the agent’s behavior. Simulation maps dynamic cases that are easier to maintain; observability shows what actually happened. The two create a flywheel.


### 07:01 | Five forces reshaping voice AI


Several forces are moving at once. It has been an exciting week: OpenAI just released a new full-duplex speech-to-speech model, a huge step forward, and native audio is improving quickly. Latency remains difficult because agents are being asked to use more tools and do more computation. Naturalness has advanced, but edge cases such as vocal hallucinations, volume shifts, and inconsistent delivery still appear.


Outcome-based pricing is becoming more feasible as costs fall, but it is not right for every workflow. A booked appointment is a clear outcome. Avoiding a human handoff may be a success in one business and a failure in another.


Trust and quality are becoming the differentiators. You are no longer marketing based on features the way 2020-era B2B SaaS did — you are marketing on quality. In-person enterprise sales, forward-deployed engineering, benchmarks, and pilots are becoming non-optional. Enterprise buyers need evidence that an agent will work across millions of conversations, not one impressive demo.


### 10:45 | Hybrid architectures


**Brooke Hopkins:** My bet is that we go hybrid before we go fully speech-to-speech. A transcript plus an LLM gives you context and controllability, and you can decide which information to send back to the user or where to add checks. Another hybrid pattern keeps the speech-to-speech model in the foreground while a background agent watches the transcript, runs analysis, and helps correct the system.


Voice-to-voice simulation matters in either architecture because these systems are temporal. An interruption, repeated phrase, or muffled sound can change the logic and the agent’s ability to recover.


The best agents do more than fill silence. They narrate useful work while processing it, much like a text agent showing the steps it is taking. That gives the caller confidence that something is happening and expands what users are willing to ask an agent to do.


Naturalness is a major focus, but recovery matters just as much. When the agent misunderstands or produces a bad turn, can it self-correct gracefully?


### 13:53 | Self-improving voice agents


The biggest change from last year is the push toward self-improving and self-healing agents. Maintenance becomes expensive when systems break, credentials change, user behavior drifts, or models drift — we see it with OpenClaw and all of these agents people are spinning up. The system needs to adapt to new cases, even when a human remains in the loop to approve changes.


### 15:09 | Audience AMA begins


**Henry Finkelstein:** We clustered the submitted questions into nine topics. The room is voting across architecture, hard audio problems, and eval operations, so let’s start with eval operations.


### 16:01 | Eval operations and AI sovereignty


**Brooke Hopkins:** Evals are not only about increasing your chance of success on the first launch. They are a mechanism to make you really agile — an organization that can still change models two years from now. Without an evaluation system, teams get trapped on legacy models because moving customers is too risky. AI sovereignty is a really interesting concept, and evals are what unlocks it: you can switch between models because you have a way to know what changed.


They also force product clarity. If it is difficult to write an eval, it may be unclear what the product should do or what success looks like.


### 17:25 | Eval hierarchy and smoke tests


Your strategy should not be to run every test on every change. Put most of your coverage on the behavior you just improved and the behavior you are about to improve. Keep a smaller set for things that must always work, such as answering the phone, canceling an appointment, or rescheduling. Those are smoke tests.


At Waymo we used layers: basic smoke tests, tests for the feature area under active development, broader standard evals, and post-submit evals. A hierarchy like that creates an ongoing flywheel and helps control the cost of voice testing.


Timing and background noise remain non-trivial because they affect whether the agent can recover, not just whether the audio sounds clean.


### 19:33 | Speech-to-speech readiness


**Henry Finkelstein:** Can speech-to-speech replace cascaded systems? What do we lose, and what does hybrid actually mean?


**Brooke Hopkins:** OpenAI’s new realtime model is a major improvement in instruction following and full-duplex conversation. It feels more natural and handles interruption better. The remaining challenge is controllability. Once the model decides to say something, you may not have time to insert a guardrail or correction, and tool calling happens inside the model. I expect more hybrid systems with background agents and other models handling different parts of the pipeline.


**Doug (attendee):** We have it in beta with a few clients. It was straight up faster, and the language is really solid. But there are quirks. We saw an unexpected voice change, and in multilingual evaluations the intent could sound natural in English but strange in Spanish. If only English speakers are doing the evaluation, that becomes a complex and non-scalable problem.


**Brooke Hopkins:** That is why human judgment needs to remain part of the evaluation system — it is also why we started providing labeling. A native Spanish speaker can label a sample of calls, and then you can measure how other automated metrics correlate with those judgments. The goal is to scale human judgment, not pretend you can remove it entirely. No reviewer can listen to a thousand calls on every iteration — and for some teams the question is even simpler: how do I even get to a Japanese speaker?


### 24:17 | Build vs. buy evaluation infrastructure


**Henry Finkelstein:** When can a team take an evaluation system off the shelf, and when should it build its own?


**Brooke Hopkins:** Building simulation sounds easier than it is. There is a dog-chasing-its-tail problem: if your system is good enough to test that behavior, then your system is probably already good enough to do that behavior. Otherwise, you are building a second product.


The simulated caller is also engineered in the opposite direction from the agent. You want the agent to avoid stumbling, repetition, poor audio, and awkward behavior. The simulator needs to produce those things. We actually simulate how sound reacts to rooms, because sound refracts off other voices and other sounds — the simulator has to model rooms, background noise, speech variation, and callers who behave unpredictably.


The durable advantage for a voice team is not owning generic eval infrastructure. It is knowing which tests to run, which datasets matter, when to run them, and how to feed the findings back into engineering. Advanced teams also build self-healing pipelines that turn evaluation results into action.


*Related reading:[Voice AI Testing Build vs. Buy: When Internal Tools Hit a Wall](https://www.coval.ai/blog/voice-ai-testing-build-vs-buy)*


### 27:29 | Evaluation beyond the transcript


**Henry Finkelstein:** In a native-audio world, what should teams measure beyond the transcript?


**Brooke Hopkins:** Look for awkward pauses and outliers in naturalness, especially audio that falls outside the distribution of what your users normally hear. Voice is more like moving from a two-dimensional world to a three-dimensional one. Interruptions, chopped audio, timing, and environmental response can change both the experience and the underlying logic. The transcript alone can make two very different interactions look identical.


**Henry Finkelstein:** “Yeah, right” versus “yeah, right, totally” — those would sound and look the same on a transcript, but not necessarily in the audio.


**Brooke Hopkins:** One hundred percent.


### 28:53 | Business outcomes and enterprise trust


**Brooke Hopkins:** Many voice agents can achieve real business outcomes today. What blocks enterprise deployment is trust and a lack of mechanisms for handling failure. Systems are too often designed as if they must succeed 100 percent of the time, which is not realistic even for traditional software. If something does fail, do we send it to a review queue? Do we have a fallback?


### 30:49 | Failure recovery in production


Self-driving systems have layers of fallback: redundant computers, background checks, the ability to slow down, and the ability to pull over. Voice agents need industry-specific equivalents. If identity verification gets stuck on a name or number, perhaps a human can verify that one field and let the call continue. A human handoff does not need to replace the entire interaction.


Names, letters, and numbers remain hard in high-stakes workflows. Teams need creative fallbacks such as text entry, a brief human assist, or another verification path. For failures such as vocal hallucinations or repetition, measure the probability across many calls. A buyer should not make a decision from one unlucky conversation.


### 32:53 | Rolling out voice AI


For a cautious CX team, define one concrete success metric and start with one bounded use case that has a high likelihood of success. Add clear fallbacks: route to a person, or have a person call back when something went wrong.


**Henry Finkelstein:** Old-school change management, applied to an agentic workflow. I also think a lot of agent platforms are going to start publishing their success rates — their P99 rates. At some point, sounding normal in a demo isn’t going to cut it. If I run 10,000 calls, what’s my probability of hitting this particular type of problem?


### 34:25 | Choosing an eval provider


**Henry Finkelstein:** Every week there seems to be a new eval platform. How should a team choose one?


**Brooke Hopkins:** Start with the team. Is this a group with the domain expertise you want to work with for the long term? Then look beyond a similar-looking product surface to platform maturity. Can it handle tens of millions of conversations, with 150 people working alongside each other on the platform? Validate scale through customers and through testing.


Finally, ask whether it fits the complete evaluation strategy. Simulation, observability, and human review or labeling should feed one another. That is how you create the improvement flywheel.


**Henry Finkelstein:** An eval platform is only useful if it improves agent performance.


### 36:49 | Raw-audio models and explainability


**Brooke Hopkins:** In the extended Q&A, Aidan asked about models for evaluating raw call audio. We have mostly experimented with Gemini for evaluating the audio itself. One effective technique is to use the transcript to identify a short window, then give the multimodal model only that relevant slice instead of an entire call.


Speech-to-speech explainability is still an open problem. Tool-call traces remain important, but when a black-box model produces the wrong response, determining the exact cause is difficult. Voice drift is another example — Amit flagged one in the comments: a male voice turning female mid-conversation, or English switching to Spanish. Detection can happen after the problematic output has already started, so graceful recovery and better controls remain necessary.


Bring those real examples back to model providers. Concrete call samples give them something they can investigate.


### 40:41 | IVR and human handoffs


**Brooke Hopkins:** One question asked about a call that must navigate an IVR and then speak to a human. Coval can simulate the IVR and the human within the same call — we have guides on how to test both. In production monitoring, teams can also create metrics that classify whether the other side was a human, an agent, or a voicemail. I don’t have tips for doing that in real time yet — I will ask around and hopefully publish more on it.


*Related reading:[Automated IVR Testing: How to Build a Regression Suite That Runs on Every Deploy](https://www.coval.ai/blog/automated-ivr-testing)*


### 42:25 | Scaling human feedback


**Henry Finkelstein:** Ryan asks how to scale human feedback on call flows.


**Brooke Hopkins:** Start with a taxonomy of the ways a workflow can fail. We provide triage categories for exactly this, and customizable metrics where you can write your own query — those triage categories become labels, and you can write metrics that correlate with them. Traces are especially valuable because they show which backend steps actually happened. A transcript-based workflow metric can only infer those steps.


**Henry Finkelstein:** Human feedback works at two levels. First, people can calibrate whether an LLM judge is grading correctly. Once the team trusts that metric, it can be applied across the whole call set. Second, people can comment on individual transcripts and turn that context into ideas for prompts, tools, and new tests.


Human review is expensive, so use it where it has leverage. Calibrating a reusable judge is high leverage because the benefit propagates across every call the judge scores.


### 45:01 | State machines vs. prompts


**Harrison (via chat):** For a voice-agent building platform, what is the better authoring UX: a graphical state-machine flow builder, or a single prompt plus API tool integrations?


**Brooke Hopkins:** There is no single answer, though we have seen a lot of teams moving away from node-based workflows. They may still make sense for something like legal intake, while being less useful in other customer-service workflows. Prototype one bounded use case both ways and benchmark the result.


A graphical state machine offers determinism but gives up some autonomy and recoverability. A prompt-driven agent offers flexibility but may not provide the control a specific workflow requires. Voice-agent platforms are a great way to get started quickly; engineering-heavy use cases with unusual timing or infrastructure needs may eventually require a hybrid or custom approach.


### 47:49 | The enduring bottleneck


**Henry Finkelstein:** Last question, also from Harrison: what will voice evals look like when we get human-level voice agents?


**Brooke Hopkins:** We will have bigger problems when we have AGI for voice. But even then — we still train call centers, we still train employees. We benchmark all the different models today, and benchmarking is hard: what does it mean to be excellent? Even if the models become perfect by some benchmark, companies still need to decide what they want the agent to do. Is escalation the right answer or the wrong one? Different businesses will answer differently.


Evals are your product.


**Henry Finkelstein:** The strategy of what exactly to evaluate will always be the hardest thing to solve. Let’s go build the evals — and build the things that do good work for customers on top of them. Thanks for joining.


## Put the ideas into practice


If you are building a production voice agent, start with the failures you cannot afford and turn them into a repeatable test suite. See how Coval helps teams[simulate](https://www.coval.ai/products/simulation) ,[monitor](https://www.coval.ai/products/observability) , and[review](https://www.coval.ai/products/human-review) voice agents.
