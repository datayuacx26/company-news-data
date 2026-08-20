---
schema_version: "1.0.0"
document_id: "7b38969ae15c6fb035a17b8a801b57d592912b051c431f439f85e08c59305964"
company_key: "yc-retell-ai"
company: "Retell AI"
source_id: "yc-retell-ai-news-import-48ab15cc20a2"
canonical_url: "https://www.retellai.com/blog/asr-llm-fallbacks"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:02.485527+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:3ba08aa01c8faf1f01cb330715334913c452fa504f1b92cc44c9145571418339"
---

# Stronger, Smarter Call Reliability: ASR + LLM Fallbacks

Retell's voice system works like a live chain with three core steps. And each one depends on the step before it.


**ASR → LLM → TTS**


**ASR (automatic speech recognition)** listens to the caller and turns their speech into text.


**LLM (language model)** reads that text, understands what was said, and decides what to say back.


**TTS (text-to-speech)** takes that response and turns it into spoken audio the caller hears.


So the flow is basically: **caller speaks → ASR transcribes it → LLM generates a response → TTS speaks it back** .


When someone is on a call, both the ASR and LLM systems are the critical systems working in real time. And both can fail, slow down, or behave unpredictably. We need to be able to rely on both systems working, so we've built a setup that's constantly monitoring, backing up, and switching in real time. And here's how the two work together and why these updates matter:


### First, we watch for lag (not failure)


We're constantly asking one simple question: "Is the system keeping up with the conversation?" Every 0.1 seconds, we compare:


- How much audio we've sent
- How much audio has actually been processed


If the gap grows beyond 5 seconds, that's our signal: This provider is falling behind. Not dead. Not broken. But headed there. And that's when we act.


### We keep a live "safety net" of your audio


As audio is being processed, we keep a rolling backup of anything that hasn't been fully handled yet. Think of it like this:


- If the system confirms it processed something → we discard it
- If it hasn't yet → we hold onto it


So at any moment, we have a perfect copy of the "in-between" audio, which is the part that's most at risk of getting lost. No guessing. No gaps.


### Then we swap in a backup


The moment we detect lag, we don't wait around. We:


- Spin up a backup provider (there's a priority order: fastest, closest, most reliable first)
- Send over all that "in limbo" audio so it can catch up
- Shut down the struggling provider


We also give the new provider a short grace period (~20 seconds) to stabilize before we start judging its performance.


### The result?


The transcript just… keeps going. No jump. No rewind. No weird gaps. From the caller's perspective, nothing happened.


### Why this matters


Most systems wait for a provider to fully crash before switching.


We don't. We catch the moment it starts to struggle and replace it before it ever becomes a problem.


### Bottom line


- We don't wait for failure
- We detect slowdowns in real time
- We preserve every second of audio
- We switch providers seamlessly


So the conversation keeps flowing exactly the way it should.


## LLM: from provider-level to deployment-level routing


## How our LLM fallbacks work


Now on the response side, failure isn't so obvious. It's more subtle. For each LLM model (i.e. GPT-4.1) we have a set of "deployments" that serve that model. Think of a deployment as a physical data center. When we want to generate an AI response—or retrieve grounded context from an[LLM Knowledge Graph](https://www.puppygraph.com/blog/llm-knowledge-graph) to improve accuracy and reduce hallucinations—we need to specify a particular deployment to process that request.


Under the hood, there are a few moving parts and can get a bit complex, but the core idea is simple:


## We route to deployments that have lower latency


Ensures that responses are generated faster and reduces lag to keep conversations in sync in real time.


## We constantly measure & monitor the error rate of each deployment


And we cut traffic to the ones that have high error rates.


## We've got a need for speed when sending a request to a deployment


If it's slow, we don't wait. We send the request somewhere else and keep going until one responds.


## May the best AI model win


If a model fails across multiple deployments, we don't keep trying it. We switch to another and keep going.


## Bottom line


We're not relying on one model or one provider.


We're constantly:


- routing to the best option
- avoiding what's failing
- racing for faster responses
- and switching when needed


So even when systems have a bad day, your conversations don't.
