---
schema_version: "1.0.0"
document_id: "ac4fd03bfaca14ed0665ad9cbbd26a553c17453e748bbdfd6fcc60715d4d33d9"
company_key: "yc-daily"
company: "Daily"
source_id: "yc-daily-rss-fbcbe287eccb"
canonical_url: "https://www.daily.co/blog/advice-on-building-voice-ai-in-june-2025/"
published_at: "2025-06-24T17:47:09+00:00"
first_seen_at: "2026-07-20T23:23:39.254949+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:c01493d9a651c18eaf25851e016850a4cf97b44469b6991291fd0ccceaea2b87"
---

# Advice on Building Voice AI in June 2025

My top three pieces of advice for people getting started with voice agents.


1. Spend time up front understanding why latency and instruction following accuracy drive voice AI tech choices.
2. You will need to add significant tooling complexity as you go from proof of concept to production. Prepare for that. Especially important: build lightweight evals as early as you can.
3. The right path is: start with a proven, "best practices" tech stack -> get everything working one piece at a time -> deploy to real-world users and collect data -> then think about optimizing cost/latency/etc.


Let's take these one at a time.


## Latency and function calling accuracy drive voice AI architecture


### Latency


A good rule of thumb is that you should be aiming for 800ms median voice-to-voice latency (eventually).


It's okay to start with a looser target (1,500ms in initial proof of concept, for example). But you should understand from the beginning what contributes to latency. This drives model choice, network stack, design of your main conversation loop, the fact that you shouldn't (mostly) use MCP in a voice agent, etc.


Big contributors to latency


1. Network - 200ms (if you use WebRTC, worse with WebSockets)
2. Turn detection and transcription - 400ms
3. LLM - 500ms
4. Text to speech - 200ms


Some things to think about:


- Any conversation turn with a tool call doubles the LLM latency.
- The above are P50 numbers for the best hosted services today. Using services with worse P50 numbers or big P95 spreads will have a substantial impact on your total voice-to-voice latency.
- Trade-offs abound: you really like the voice from provider X, but the P50 TTFB is 700ms rather than 200ms. Is that worth it? It might be! But measure everything so you can make choices that are both quantitative and qualitative.


References:


- [https://voiceaiandvoiceagents.com/#latency](https://voiceaiandvoiceagents.com/#latency)


### Instruction following


"Instruction following" just means that the LLM does what you expect it to do, given a good prompt.


The most important subset of instruction following is function calling accuracy.


Almost all production voice AI agents rely heavily on function calling for things like:


- context look-up (RAG)
- saving data to back-end systems
- integrating with telephony systems
- cleanly terminating a session


Instruction following accuracy is critical for voice agents. Sadly, even today's best LLMs don't have great instruction following performance in multi-turn conversation contexts.


GPT-4o is the best general-purpose model on the Berkeley Function-Calling Leaderboard. It scores 72% overall accuracy on that benchmark. But on the "multi-turn" subset of the BFCL, GPT-4o scores 50%. GPT-4o-mini scores 34% on multi-turn accuracy.


This has three implications:


1. For almost all voice AI use cases, you need to use the current best available model. Any other model choice reduces agent performance unacceptably. **That means that today you should generally be starting with GPT-4o or Gemini 2.5 Flash, until you collect enough data to write evals that show you if other models work well enough for your app.**
2. Make things as easy as possible for the LLM. Define as few tools as possible. Write detailed, multi-shot prompts. Don't inject extra indeterminacy if you can avoid it (be very selective about where you use MCP vs hard-coding your tool calls, for example).
3. Because instruction following degrades quickly as multi-turn context length grows, you will very often need to do in-session "context engineering" to achieve acceptable success rates. This means compressing and focusing conversation context at specific points in the voice workflow.


**A note on speech-to-speech models/APIs like the OpenAI Realtime API, the Gemini Live API, and AWS Nova Sonic**


These next-generation models and APIs are the future. But for most use cases, they aren't the present, yet. Instruction following performance, ability to manage context flexibly via the API, and ability to do end-to-end monitoring and debugging are all worse with speech-to-speech APIs, today.


Most of us building in voice AI today recommend starting with the three-model (transcription -> text-mode LLM -> voice generation) in almost all cases. The exceptions to this rule are if your use case is "narrative" and doesn't require high instruction following accuracy, or if you are doing mixed-language conversations such as language tutoring.


The good news is that if you're using a framework like Pipecat, it's fairly easy to write your agent code so you can switch between the three-model approach and speech-to-speech models without changing any of your app logic. So you can test every new model release, benchmark against your evals, etc. If a speech-to-speech model performs well on your evals, you can move from the 3-model approach to that speech-to-speech model.


References:


- [https://gorilla.cs.berkeley.edu/leaderboard.html](https://gorilla.cs.berkeley.edu/leaderboard.html)
- [https://voiceaiandvoiceagents.com/#speech-to-speech](https://voiceaiandvoiceagents.com/#speech-to-speech)
- [https://voiceaiandvoiceagents.com/#scripting](https://voiceaiandvoiceagents.com/#scripting)


## Understand the tooling necessary to go from proof of concept to production


It's fairly easy today to build a really good voice AI demo. It's not as easy to go from ~90% conversation success rate to >99%.


On the way from initial POC to production ...


### You will definitely need


**Traces and observability tooling.**


In production, your agent will fail. You will have:


- Prompt issues (that you can relatively easily improve if you have good monitoring in place).
- Service-level issues (your providers will not be as reliable as you wish they were, at this stage in the evolution of AI).
- Bugs in your code :-).


The easier it is to look at stack traces, otel spans, and inference results for every unsuccessful conversation, the faster you will be able to improve your agents.


Inference traces are especially important for building an evals flywheel. Once you have an agent running in production, you can constantly improve your conversation success rates by pulling real-world data into a lighweight evals regime. This is one of the highest impact ways you can spend your engineering/product effort.


### You will probably need


**Context compression**


As described above, instruction following and function calling accuracy degrade considerably over the course of a multi-turn conversation.


If your voice agent needs to follow a series of steps reliably, or will perform conversations longer than a few turns, you will probably need to think about doing "context engineering" to keep the conversation context short and focused.


One useful way to think about context engineering is to design your conversation as a series of workflow states. Each state corresponds to a specific "job to be done" during the voice interaction. For each state, you can define:


1. A system instruction for the state.
2. A context transformation to do when you enter the state. This is typically an LLM prompt that takes the full previous conversation state and summarizes it, focusing on the most relevant information.
3. Tool calls available in this state.
4. Next states that the LLM can proceed to from this state.


The popular Pipecat Flows library implements helpers for this state machine approach.


**Async tool use**


Does your agent need to look up information in back-end systems? (To do RAG, for example.) Do you need to use MCP servers? Web search?


All of the above things are probably too slow to do synchronously in your core conversation loop. So you will need to implement some kind of async function calling approach.


The basic idea is to either:


1. Return from the tool call right away and insert a place-holder function call output message in the conversation context. Later, when the actual tool call returns, you can incorporate the results in the context. (There are a few different ways you might want to do this, depending on your use case.)
2. Don't do the tool call from the main conversation loop at all. Run a parallel inference pipeline dedicated only to tool calling that inserts information into the conversation context from outside the main conversation loop. This works well for situations where the tool call is contextual but not actually triggered by anything specific that the user says. For example, updating an image to match the context of an interactive children's story, or doing safety checks on input content and generated content.


References:


- Optimizing AI Voice Agents:[https://www.youtube.com/watch?v=I86dFivLzXY](https://www.youtube.com/watch?v=I86dFivLzXY)
- [https://voiceaiandvoiceagents.com/#async-function-calls](https://voiceaiandvoiceagents.com/#async-function-calls)
- [https://github.com/pipecat-ai/pipecat-flows](https://github.com/pipecat-ai/pipecat-flows)


### You might need a long tail of additional capabilities


Production voice agents often need things like language switching, content safety filters, voicemail detection logic, phone tree navigation, call center "warm transfers," and many more.


## Use a proven stack and make it work, before you optimize or get creative


There are enough moving parts in a production-quality voice AI stack that it generally makes sense to start with elements you know people are using successfully in production, at scale. Get things working reasonably well. And then start optimizing/experimenting.


This means use a framework that gives you reliable network transport, echo cancellation, good turn detection and interruption handling, context management and async function calling helpers, audio resampling and buffer management, etc.


You'll also need to choose a speech-to-text (transcription) model, an LLM, and a text-to-speech (voice) model.


- Pick a transcription model that works well for your language. The most widely used transcription models for realtime AI are (in alphabetical order): Cartesia, Deepgram, and Gladia
- For the LLM, use GPT-4o or Gemini 2.5 Flash
- Pick a voice you like from Cartesia, ElevenLabs, or Rime.


There are other choices in all of these categories. But the fastest path to production is to use models that work well for realtime voice AI as your starting point.


Here are a couple of examples of issues you may face with models other than the ones above.


- There are great voices from other TTS providers, including some amazing open source options. But if your TTS model doesn't have word-level timestamps, you can't align the conversation context with what the user heard, when the user interrupts the agent. For most use cases, you need to be able to do this. Cartesia, ElevenLabs, and Rime all have word-level timestamp support.
- For STT, if your model delivers final transcripts outside your VAD timeout window, you have to figure out how to handle that. There's no perfect option. You can introduce an additional aggregation delay. (That's what Pipecat does.) But that pads your voice-to-voice response time substantially.


These kinds of things take a lot of time to wrap your head around when they happen in your voice agent and you're new to debugging voice AI pipelines.


Relatedly, start with the default VAD (voice activity detection) and interruption handling settings in your framework. Get *everything* else working before you start trying to tweak turn detection and interruption handling.


## Summary


I'll summarize all of the above recommendations by turning them into a list of *don'ts* .


- Don't start with a speech-to-speech model, an open weights model, a "mini" or "light" model, a fine-tuned model, or anything other than GPT-4o or Gemini 2.5 Flash. You may very well be able to use a wide variety of models for your use case. But you won't know that until you have good, real-world usage data and basic evals.
- Don't use MCP unless you have a specific reason you *need* MCP. Hard-code all the tools calls that you can.


Don't go down any rabbit holes trying to improve VAD, turn-taking logic, hacking together your own echo cancellation, etc. If you are building on a widely used voice tooling/platform (like Pipecat), people are shipping voice agents at scale to production using the defaults of that platform. Get everything working reasonably well before you start changing default settings.


*A plain-text version of these notes is also available as a*[GitHub Gist](https://gist.github.com/kwindla/f755284ef2b14730e1075c2ac803edcf)


#### Never miss a story


Get the latest direct to your inbox.
