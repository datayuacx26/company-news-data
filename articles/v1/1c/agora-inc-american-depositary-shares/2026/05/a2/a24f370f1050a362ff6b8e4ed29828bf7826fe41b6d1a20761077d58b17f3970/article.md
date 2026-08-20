---
schema_version: "1.0.0"
document_id: "a24f370f1050a362ff6b8e4ed29828bf7826fe41b6d1a20761077d58b17f3970"
company_key: "agora-inc-american-depositary-shares"
company: "Agora Inc."
source_id: "agora-inc-american-depositary-shares-news-import-bf245f251fd4"
canonical_url: "https://www.agora.io/en/blog/gpt-realtime-2-is-here-and-preambles-change-how-voice-agents-feel"
published_at: "2026-05-07T00:00:00+00:00"
first_seen_at: "2026-07-23T01:17:52.872297+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:c4e576c20799e64ec865cc032df1a0652fedb10b4c7b0b023b4b497e30c3698b"
---

# GPT Realtime 2 Is Here — And Preambles Change How Voice Agents Feel

OpenAI just released **GPT Realtime 2** , the next generation of their flagship speech-to-speech model. It is a meaningful step up from the original gpt-realtime in instruction following, tool calling, and multilingual performance — and it brings a new interaction primitive, **preambles** , that we haven't seen in production Speech-to-Speech (S2S) models before.


Reasoning-capable voice models aren't new at this point — the broader field has been moving in this direction for a while, and other strong production-grade S2S models are out there doing real work. What'sdistinct about Realtime 2 is *how* it surfaces its reasoning to the user. That's the part worth digging into if you're building voice agents.


## **Reasoning, but with the silence problem solved**


The newer wave of S2S models — Realtime 2 included — moves past that by making reasoning a first-class part of the loop. You can turn thinking on and pay for it with real compute time before the response. The challenge with that approach is obvious: if the model takes two seconds to reason, the user spends two seconds listening to silence, and silence on a phone call feels broken in a way that silence on a chat screen doesn't.


Realtime 2's answer is **preambles** : short, in-character spoken acknowledgments emitted before the substantive response, while the model is still reasoning. The model talks to you *during* the thinking window instead of after it. Total response time goes up. Perceived response time stays low because the user starts hearing the model immediately.


This is the central insight: silence is the latency, not seconds. A two second gap of dead air feels broken. Two seconds of *"sure, let me pull up your account"* feels like a competent human on the other end of the line — even if the actual lookup takes longer than the silent version would have.


In practice, this means three things you will feel immediately when shipping with Realtime 2:


- **Preamble-bridged reasoning** that fills the thinking window with speech, so the conversation never goes quiet


- **Stricter adherence** to the instructions and persona you set in the system prompt


- **More structured, multi-step outputs** instead of immediate conversational replies


It's the trade developers have been asking for: stop optimizing the wrong number. Time-to-first-audio is not the same as time-to-useful-answer, and preambles are how Realtime 2 keeps the first one low *while* the second one gets dramatically better.


## **The headline feature: preambles**


Preambles are the mechanism that makes reasoning-first voice work, and they deserve a closer look. They are **lightweight, model-controlled spoken acknowledgments** delivered before the final response — things like *"one moment while I check that for you"* or *"let me pull up your account."* The model decides when to use them and what to say.


We've already covered the latency story. But preambles unlock capabilities that previous voice systems literally could not express:


- **Multilingual split.** A preamble can be in one language while the main answer is in another — useful for handoffs, translation flows, or routing layers that need to acknowledge the user before switching contexts.


- **Agent signaling.** The model can communicate intent, state, and uncertainty in-band. *"I think you mean…"* or *"I'm not totally sure, but…"* become first-class parts of the output, not hacks bolted on top.


- **Interruptibility.** Preambles give the user a window to jump in and redirect *before* the model commits to a long answer. This is enormous for any agent doing longer reasoning or multi-tool work — users can course-correct early instead of waiting through a wrong answer.


- **Tool transparency.** *"Checking your calendar."* *"Looking that up in the database."* The user understands what is happening and why there is a brief pause. Black-box silence is replaced by visible action.


- **Error recovery.** *"Retrying that"* or *"having trouble reaching the server"* surface gracefully instead of failing silently into dead air.


If you have ever shipped a voice agent and watched users hang up during a tool call because they thought the line went dead, you already understand why this matters. Preambles fundamentally change the UX of speech-to-speech.


### **Why this isn't just "say something when you call a tool"**


Developers coming from chained STT → LLM → TTS pipelines will recognize a familiar pattern here: bolt a "checking your calendar now" message onto every tool call so the line doesn't go silent. That works, sort of, but it's a fundamentally different mechanism — and it only covers the easy case.


Chained-pipeline tool announcements are **deterministic, hand-authored, and only fire on tool invocations.** You write the strings, you wire them to specific function calls, and they trigger when (and only when) a tool runs. They are scaffolding around a known event.


Realtime 2 preambles are **model-generated, context-aware, and fire whenever the model needs thinking time** — tool call or not. That last part is the one that matters. Most of the hardest latency problems in voice agents have nothing to do with tools.


Consider a customer support agent for a SaaS product. A user calls in and says:


*"Hey, my team has been seeing weird billing on our enterprise plan — we got charged for two extra seats last month, but we definitely didn't add anyone, and now our SSO is failing for half the team. I think it might be related to the migration we did in October, but I'm not sure. What should I do?"*


There is no tool to call here. There is no API lookup that resolves this. The model has to actually *reason* — parse three intertwined issues (billing dispute, SSO failure, possible causal link to a past migration), figure out which one to address first, decide whether to ask a clarifying question or propose a diagnostic path, and structure a coherent response. That's two or three seconds of real thinking.


In a chained pipeline, you get silence. There's no tool call to attach a message to, so your hand-authored scaffolding doesn't fire. The user sits in dead air wondering if the agent understood, and a meaningful percentage of them will start repeating themselves or hang up.


In Realtime 2, the model emits something like *"okay, let me think through this — sounds like there might be a couple of things going on,"* then delivers the actual structured response a moment later. The thinking happens, the user knows it's happening, and the call stays alive. No tool was involved. No engineer wrote that preamble. The model decided it needed time to reason and bridged the gap itself.


This generalizes. Anytime the model is doing genuine reasoning — disambiguating a vague request, weighing options, deciding between multiple valid interpretations, structuring a complex multi-part answer — preambles cover the gap. Chained-pipeline tool announcements cannot. That's the difference.


## **Reasoning effort, in your hands**


Realtime 2 introduces a new reasoning effort parameter that you can set for the entire interaction or override on a per-turn basis. This is the most flexible knob OpenAI has shipped on a voice model to date, and it opens up some genuinely new patterns:


- Run the bulk of a conversation at low for snappy back-and-forth


- Bump to high mid-conversation when the user asks something genuinely hard, or when a tool call needs careful argument construction


- Drop to minimal for confirmation turns ("yes," "no," "go ahead")


You can shape the cost/latency/quality curve dynamically inside a single session — something that previously required model-swapping or routing layers.


## **Better emotional range and steerability**


Realtime 2 is also significantly more expressive and significantly more steerable. The list of styles the model can convincingly produce is much wider — whispers, anger, jealousy, empathy, ecstasy — and the model is much better at *matching* the caller's energy. Talk to it flatly and it will tend to respond calmly. Bring intensity and it will meet you there.


For developers, this means persona prompts actually do what they say. *"Speak like a quirky teacher."* *"Pirate voice, but professional when discussing the bill."* *"Whisper this part."* It works.


## **Longer context for agentic workflows**


Realtime 2 ships with a longer context window built specifically for agentic voice work — long-running customer support sessions, multi-tool workflows, conversations where the model needs to remember what was decided ten minutes and three tool calls ago. Combined with the upgrades to tool calling and instruction following, this is the first voice model that holds up across genuinely long sessions without degrading.


## **Testing tips for shipping with Realtime 2**


A few things we have learned in early testing that will save you time:


- **Start with reasoning = low.** Most flows do not need more, and you will get the snappiest experience by default. Move up only when you measure a real quality gap.


- **Use preambles thoughtfully.** Best practice is *no preamble for simple turns* , and *one short preamble for longer reasoning or tool-calling turns* . For very simple flows, use minimal or explicitly tell the model in the system prompt that you do not want preambles.


- **Iterate on your prompts.** Realtime 2's instruction following is dramatically better — but that means conflicting or vague instructions hurt more, not less. Remove contradictions and be explicit about pacing ("speak quickly," "speak slowly"), terminology, and custom vocabulary the model should listen for, language constraints ("only listen and speak in Chinese"), and role or style ("pirate," "quirky teacher," whatever fits).


- **Test emotional matching.** The model adapts to the caller's tone by default. If your test inputs are flat and monotone, the model will sound flat too — that is a feature, not a bug, but you need to test withrealistic energy to see what users will actually hear.


## **Build production voice agents on Agora**


Production-grade S2S is here. Reasoning-capable voice models are pushing past the demo-quality ceiling and into territory where they can carry out real work — handle ambiguity, call tools reliably, hold up across long sessions, follow instructions that matter to a business outcome. Realtime 2's specific contribution is **preambles** , a clean answer to the silence-during-reasoning problem that every thinking-enabled voice model has to solve somehow.


Agora powers over 80 billion minutes of real-time audio + video every single month, and our **Conversational AI Engine** is the fastest way to put Realtime 2 into a production voice agent — built-in interruption handling, low-latency global transport, and the orchestration layer to make reasoning-first models feel native on the call.


Try Realtime 2.0 on the Conversational AI Engine and see what preamble-driven voice can do:[https://docs.agora.io/en/conversational-ai/models/mllm/openai](https://docs.agora.io/en/conversational-ai/models/mllm/openai)


‍
