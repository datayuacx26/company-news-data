---
schema_version: "1.0.0"
document_id: "4d26c6e4e227640f50f27295698086703fbd8f1847baf91c5c07232f0cffd288"
company_key: "agora-inc-american-depositary-shares"
company: "Agora Inc."
source_id: "agora-inc-american-depositary-shares-news-import-bf245f251fd4"
canonical_url: "https://www.agora.io/en/blog/openai-didnt-publish-gpt-lives-latency-so-we-measured-it"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-23T01:17:52.872297+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:263cb441988141b2cf9eb6c3d4261a850cae8a60fcd2deedcef7210315cfe330"
---

# OpenAI Didn’t Publish GPT-Live’s Latency. So We Measured It.

*On a real phone, GPT-Live was 500 ms slower to yield when interrupted, ignored background speech that fooled the older modes, and gave up 314 ms to packet loss that cost the previous generation 2.4 seconds.*


*Agora Media Lab · July 10, 2026*
*Methods and limits at the end.*


[OpenAI launched GPT-Live](https://openai.com/index/introducing-gpt-live/) on July 8 promising AI powered voice conversations that feel natural; it listens while it speaks, waits while you think, and hands harder questions to a background model without leaving the conversation. What the launch didn’t include is any end-to-end number for what “natural” means on a real device.


These just so happen to be the numbers our lab’s engineers measure as part of their daily tests. We ran the latest ChatGPT app through our full protocol, from testing on real devices, to repeatable speech injected through an artificial mouth (actual lab equipment, not a metaphor), controlled network impairment, and every number read off of dual-track waveform recordings. We tested three generations of ChatGPT’s voice interface:


- **GPT-Live-1** , the new full-duplex mode. It listens while it talks, like a phone call.
- **Advanced Voice Mode** , the previous generation. A single speech-to-speech model, but strictly turn-based: it waits for you to finish before it responds.
- **Standard Voice Mode** , the original pipeline: transcribe your speech, generate a text reply, read it aloud. Three hand-offs per turn.


To our knowledge, this is the first public controlled measurement of GPT-Live as users experience it: model, client, network, serving, and playback together.


Three results stood out. A fourth kept coming up in the lab but resisted measurement; it’s at the end.


## 500 ms to think: slower to stop, harder to fool


When you deliberately interrupt while the AI is speaking, GPT-Live takes the longest to go silent. It spends that time deciding whether you meant it:


Measured alone, that’s a 498 ms regression against Advanced, the turn-based predecessor, and getting talked over for an extra half second is the kind of thing you notice. But stop latency isn’t the only thing users experience. We also played sounds that should *not* count as interruptions:


The cough row is one event versus two, statistically a tie. The background-speech row is not: GPT-Live rejected all thirty probes; Advanced stopped for twenty; Standard stopped for every one. (In the same noise, all three modes understood the primary speaker, 30/30. The generations don’t differ in hearing you through noise; they differ in staying on course while speaking through it.) The old systems treat any sound as an interruption: fast, consistent, and frequently wrong. GPT-Live waits for enough evidence to tell a genuine barge-in from an acoustic event that happens to overlap its speech. (The 498 ms is an end-to-end number: decision policy, network round trip, playback buffer, and client stop behavior all contribute.)


The new policy also fails in a way the old reflexes never could. In a noisy room where nobody was addressing ChatGPT, GPT-Live spontaneously answered background voices in 4 of 30 ten-second windows; the older modes stayed silent. That’s a speaker-attribution problem: the system detects speech correctly but misjudges who it’s for.[OpenAI’s help docs](https://help.openai.com/en/articles/20001274/) acknowledge the boundary.


**The trade-off: GPT-Live takes longer to yield, it fell for 1 of our 60 false-interruption probes where Advanced fell for 22, and picks up a new risk of joining conversations that weren’t addressed to it.**


## GPT-Live’s real gain is 5× less jitter, not 5× more speed


Response latency, from the last frame of user speech to the first audible frame back:


You can write off Standard Voice Mode: the original transcribe-reply-speak pipeline is a museum piece at five seconds per turn, two product generations behind industry norms. The live comparison is GPT-Live against Advanced Voice Mode, and at the median GPT-Live wins by just 205 ms, hardly a generational leap. The tail is where the generation changed. Advanced’s P90 is 2,318 ms, nearly double its own median: one response in ten leaves you hanging past two seconds, and you never know which. GPT-Live’s P90 sits 104 ms above its median. Call it **turn-taking jitter** , borrowing the transport term for what users feel: GPT-Live barely moved the median, but it collapsed the variance around it. The spread shrank nearly fivefold (σ: 489 → 104 ms).


Your brain interprets that consistency as “natural.” Human turns transition in roughly 200 ms ([Stivers et al., PNAS 2009](https://www.pnas.org/doi/10.1073/pnas.0903616106) ); a response that lands on the same beat every time reads as thoughtful, while one that oscillates between 1.3 and 2.3 seconds reads as stalling. GPT-Live is nowhere near human speed, but it made the distance predictable.


“First audible frame” hides something, though: it includes acknowledgments. When a question needs search or reasoning, GPT-Live says “let me check that” while GPT-5.5 works in the background, so its first sound arrives early even when its answer doesn’t. The hard latency didn’t disappear; GPT-Live **redistributed it into the conversation** . Voice agents need three clocks: acknowledgment onset, substantive-answer onset, and answer completion. This round measures the first; the next will instrument all three.


## 10% packet loss breaks the old models and barely touches GPT-Live


We degraded the network to find out how much damage it takes to erase a generation of model progress. Turns out 10% packet loss does it with room to spare: GPT-Live’s P90 under loss (1,836 ms) still beat the P90 that turn-based Advanced managed on a *clean* network (2,318 ms). The degraded new model outperforms the healthy old one.


The medians say the same thing. Under 10% uplink loss, GPT-Live gives up 314 ms while Advanced gives up 2,448 ms, nearly tripling to within sight of Standard’s clean-network baseline. Swap the impairment to the downlink and add 100 ms of delay, and the ranking doesn’t move.


From outside the stack we can’t separate the three plausible contributors: an **incremental architecture** that consumes audio continuously, so a lost packet costs context rather than stalling a pipeline; **transport and client differences** in codec, loss concealment, and jitter buffering across modes; and **training** on degraded audio. Assigning percentages would require internal telemetry, and we won’t pretend otherwise. For anyone building on these models the split matters less than the sum: the resilience ships with the product, whatever produced it.


## The hidden requirement: full-duplex is a network contract


In a text chatbot, jitter is an inconvenience. In a full-duplex system, timing *is* meaning: a pause is input, so network delay can masquerade as user hesitation; barge-in is a round trip, which is why stop latency is a systems number, not a model number; backchannels have a deadline, so *predictable* round-trip time matters as much as low; and loss concealment now serves two listeners, because audio smoothed for human ears may not preserve what a model needs.


A full-duplex model can’t deliver a natural experience alone: model, serving, transport, and client have to agree on time. OpenAI tunes all four because it owns the stack (see the[infrastructure work it published this spring](https://openai.com/index/delivering-low-latency-voice-ai-at-scale/) ). Most teams own none of it; they consume a model API over the public internet and inherit every trade-off as their own problem. That gap is the layer Agora builds: a global real-time network in place of the public internet’s default paths, loss-resilient transport tuned for model consumption, and the client-side barge-in and AI-VAD behavior that decides how fast an agent shuts up when you talk over it. Disclosure: this is our business, so discount our emphasis accordingly. It’s also why we knew which probes to build; we’ve spent years tuning exactly these trade-offs.


## What we felt but haven’t measured yet


Our test engineers kept coming back to one behavior rather than any single number: GPT-Live answers quickly, works on something else in the background, and never leaves dead air while doing both. The older modes went silent in exactly that gap. That’s the distance the three-clock framework above is meant to quantify: *sounding* responsive versus *being* done.


Three more consistent observations: GPT-Live handled *our* backchannels without losing its thread, where older modes treated a murmured “mhmm” as a new turn and changed topic. Its emotional register shifted naturally with context. And it showed a working sense of elapsed time in “remind me in 40 seconds” probes, though it occasionally missed; older modes fail these outright. All are candidates for the next protocol, alongside speaker attribution, code-switching, and long-session drift.


Putting it all together, on July 8 the bar was reset for every voice agent: roughly **1.1 s** to acknowledge, **1.4 s** to yield when interrupted, zero false stops under background speech, **+314 ms** under 10% packet loss. Those are the numbers users will now feel in anything they compare to it. (The GPT-Live API itself is still[waitlist-only](https://openai.com/form/gpt-live-1-in-the-api/) ; the current Realtime API default,[gpt-realtime-2.1](https://developers.openai.com/api/docs/models/gpt-realtime-2.1) , is a different model that we didn’t test.)


## Methods and limits


**Setup.** iPhone 13, ChatGPT v1.2026.183 (App Store build), Plus account with GPT-Live-1 active. All speech in English. Tested July 9, 2026.


**Timing.** Prerecorded utterances played through an artificial mouth, so every trial hears identical input. Both sides recorded on separate tracks; every number read off the waveform, none from a stopwatch. n=30 per condition. Response latency: last frame of user speech to first audible frame of output. Stop latency: first frame of the barge-in to the last audible frame from the AI. Deliberate barge-ins succeeded 30/30 in every mode, which is why we report how fast each yields, not whether.


**Limits.** At n=30, don’t over-read single-digit differences, and treat P90s as descriptive. One device, one location, one account, launch week. One impairment level, and the downlink test mixed loss with delay. Treat this as a snapshot, not a census.


**Next.** We’ll run the same protocol on other realtime voice models as they ship.


The full-duplex era has started, and now we can measure it.
