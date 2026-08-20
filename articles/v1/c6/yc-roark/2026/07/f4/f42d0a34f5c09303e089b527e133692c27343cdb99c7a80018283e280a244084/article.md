---
schema_version: "1.0.0"
document_id: "f42d0a34f5c09303e089b527e133692c27343cdb99c7a80018283e280a244084"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-tool-calls-voice-agents"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-24T00:07:13.352567+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:224bcc21ad33cc8e359cd92f6717076e5c83d20a0ce94e5af095f10e591bd2dd"
---

# Testing tool calls in voice agents

In the last three weeks, two launches turned tool-calling from a nice-to-have into the default. On July 1, xAI shipped[Grok Voice Agent Builder](https://x.ai/news/grok-voice-agent-builder) with telephony, knowledge retrieval, tools, MCP support, and guardrails bundled into one interface. On July 8, OpenAI released[GPT-Live](https://openai.com/index/introducing-gpt-live/) , a full-duplex voice model that delegates complex work to a frontier model in the background and narrates the result back to the caller. The story is the same in both cases: your agent is not just talking anymore. It is booking, refunding, transferring, and updating records while the caller is still on the line.


That is also where the failure modes concentrate. A tool-call regression will not sound wrong on the demo call. It will show up two weeks in, when an agent confidently tells a caller "you're booked for Tuesday" after passing the wrong date to` create_appointment` , or when a webhook returns a 500 and the agent decides to make something up. Unit tests on the tool schema will not catch either. This is a post about what actually breaks and how to test it under real call conditions.


## What actually goes wrong with tool calls on a live call


The tempting mental model is that tool-calling is a solved problem because the model outputs structured JSON. The receipts say otherwise. OpenAI's own case study with Retell reports that <a href="https://openai.com/index/retell-ai/">GPT-4o reached "a 70%+ success rate in multi-turn function calling"</a>, described as nearly double what the previous alternatives managed. That is a real improvement, and it also means that in a naive setup, close to three in ten multi-turn function calls miss. On a support agent that fields ten thousand calls a month, that is not a rounding error.


The failure modes that matter, in rough order of how often we see them in production replays:


1. **Wrong arguments.** The agent calls the right function with a subtly wrong value: caller said "the twenty-first," the agent booked the 12th; caller said "extra large," the agent passed` "XL"` when the schema wants` "x-large"` .
2. **Wrong function.** The agent picks a plausible-sounding sibling:` refund_order` when the caller asked for` cancel_order` , or` get_slots` when it should have called` book_appointment` .
3. **Silent tool failures.** The webhook 500s or times out, and the agent papers over it: "you're all set" when nothing was set.
4. **Hallucinated confirmations.** The agent narrates a result that never came back. There is no tool call in the trace, but the caller hears a confirmation number.
5. **Ordering and dependency errors.** The agent calls` book_appointment` before` check_availability` , or forgets to call the second tool in a two-step flow.
6. **Latency and dead air.** The tool takes four seconds, and the agent goes silent instead of saying "let me check that for you."
7. **Post-tool drift.** The tool responds correctly, and the agent then paraphrases the result wrong on the way out to the caller.


Half of these live in the model. The other half live in the boundary between the model, the tool server, and the audio stack. You cannot test them from any one layer alone.


Where tool-call failures actually happen on a live call


## Why unit-testing the schema is not enough


A JSON schema validator will happily accept` {"date": "2026-07-21"}` when the caller said "the twenty-first of next month." A dashboard tool-tester that lets you type an example prompt will not reproduce a caller who talks over the confirmation, an ASR that swaps "fifty" for "fifteen," or an endpointing model that fires the LLM turn before the caller finished the account number.


The platforms know this, which is why they have all added simulation surfaces.[Vapi's Voice Test Suites](https://docs.vapi.ai/test/voice-testing) connect two agents (yours, and their testing agent) on a real phone call.[Vapi's evals docs](https://docs.vapi.ai/observability/evals-quickstart) explicitly call out validating that a function was called and that arguments match exactly or by regex, and[advanced simulations](https://docs.vapi.ai/observability/simulations-advanced) support mocking tool responses at the scenario level so you can test paths without hitting real APIs.[LiveKit Agents](https://github.com/livekit/agents) ships first-class function tools and native MCP support with a one-line integration.[Retell exposes function calling](https://docs.retellai.com/integrate-llm/integrate-function-calling) with the same OpenAI-style contract.


The primitives are there. What most teams still lack is a repeatable way to exercise them under conditions that look like a phone call, not a chat window.


## A tool-call test plan that survives contact with production


The plan below is what we recommend to teams building on Vapi, Retell, LiveKit, Pipecat, Bland, or ElevenLabs. Every check should run over a simulated phone call, not a synthetic transcript, because the ASR and endpointing behavior are part of the system under test.


### 1. Argument capture, exact and fuzzy


For every scenario, assert on both the function name and the argument payload. Exact match where the value is deterministic (` patient_id` , enum values, currency codes). Regex or semantic match where the value is derived (a date parsed from "next Tuesday," a name spelled letter by letter). If your test framework only lets you assert on the transcript, you will miss the class of bug where the agent narrates the correct outcome and calls the wrong tool.


### 2. Ordering and dependency


Real workflows are multi-tool. Insurance verification then eligibility check then booking. Order lookup then refund then confirmation email. Tests should assert on the sequence, not just the set. Vapi's evals docs, for instance, note that <a href="https://docs.vapi.ai/observability/evals-quickstart">tool calls are validated in the order they're defined</a>. Whatever platform you are on, express the graph, not the bag.


### 3. Error injection


The happy path is the least interesting test. Inject:


- HTTP 500 from the tool endpoint
- Timeouts (return after 6, 12, and 30 seconds)
- Empty results (no availability, no record found)
- Ambiguous results (two matching patient records)
- Malformed payloads (the field you expected is missing)


For each, assert that the agent does not fabricate a confirmation. If your only signal is "did it complete," you will ship an agent that reliably lies to callers when the backend is unhappy.


### 4. Post-tool speech


After the tool returns, the agent has to explain the result on the phone. This is where hallucination lives. Assert on what the agent says in the turn after the tool call: confirmation number, date, dollar amount. Compare against the tool response payload, not the model output. On real calls this is best done with an audio-native metric that scores what the caller actually heard, not just the transcript.


### 5. Dead air and keep-alives


Tools take time. Assert that the agent produces a filler utterance ("give me one moment while I look that up") within an acceptable window (we recommend under 800 ms) and that it does not talk over the caller if they add context while it is thinking. GPT-Live's own release notes describe emitting backchannels like <a href="https://openai.com/index/introducing-gpt-live/">"mhmm" or "yeah" during conversations</a>. That is exactly what you want to verify: does your agent stay present during tool latency, or does it disappear.


### 6. Auth and identity


When tools touch third-party systems, the failure mode is not always semantic. It is often that the agent invokes the tool with the wrong user's scope. The[Scalekit team wrote a good breakdown](https://www.scalekit.com/blog/livekit-voice-agent-development) of why long-lived user tokens in the worker turn tool calling into an auth liability. Simulation suites are the right place to catch this: run the same scenario as user A and user B in parallel and assert the tools were invoked with the correct identifiers.


A tool-call scored on a live call: right function, wrong argument


## Running this in CI, not on Fridays


Every one of the checks above degrades over time if you only run it manually before big releases. Prompts drift. Models get swapped. Someone adds an enum value to the schema and forgets one call site. The pattern that works is the same pattern that works for the rest of your backend: a regression suite that runs on every deploy, and a gate that blocks promotion when a scenario fails.


Two practical constraints when you build this:


- **The tests have to run over a real phone call.** Text loopback misses ASR errors, endpointing bugs, and TTS/tool-timing interactions. If a test says "the agent called` book_appointment` with the right date" but the caller was never transcribed correctly in the first place, you are grading the wrong exam.
- **Failures have to be replayable.** When a scenario fails, you need the audio, the transcript, the tool-call arguments, the tool response, and the exact turn where things went wrong. Pointing at "conversation quality: 62%" without a reproducer is not actionable.


Both of these are why we built[Roark](https://roark.ai/) the way we did.


An illustrative pre-launch sim run gating a deploy on tool-call assertions


## Where Roark fits


Roark's simulation testing dials your voice agent over real telephony (PSTN and WebRTC), not a text loopback. Simulations are built from personas, scenarios, run plans, and schedules, and they can be triggered over HTTP so a CI job can gate a deploy on the outcome. Personas define caller voices, languages and accents, speech pace, emotional register, and background noise, which is exactly the axis where tool-call arguments get corrupted by ASR errors. We simulate and score in 45 languages and accents.


On the scoring side, Roark evaluates every call against 64+ built-in metrics plus unlimited custom metrics. Audio-native models score the sound of the call, not just the transcript: pronunciation, emotion, vocal stress, pace and pauses, interruptions. When a call fails a metric, an issue gets filed automatically, which means a wrong-argument tool call or a missing keep-alive utterance surfaces the same way a security ticket does, not as a line item in a dashboard nobody reads.


The piece that closes the loop is production call replay. When a real caller hits a tool-call failure in production, Roark captures the call and lets you replay it against updated agent logic, turning the failure into a repeatable regression test. That is how a one-time incident becomes a scenario in your CI suite instead of a lesson you have to relearn.


Roark ships one-click integrations for[Vapi, Retell, LiveKit, Pipecat, Bland, and ElevenLabs](https://docs.roark.ai/) , plus a Node SDK (` @roarkanalytics/sdk` ) and a Python SDK (` roark-analytics` ). Calls can be ingested with` client.call.create` when you want to push recordings from your own stack. For teams in regulated verticals, Roark is SOC 2 Type II certified, pen-tested, and a HIPAA BAA is available.


## The bar has moved


A year ago, the interesting question was whether a voice agent could hold a conversation. Now that Grok Voice Agent Builder can spin up a production phone agent with tools and MCP in under two minutes, and GPT-Live can delegate complex reasoning while it stays present on the call, the interesting question is whether the tools it calls do the right thing under real conditions. Testing that is the difference between an agent that sounds impressive and an agent you can trust with revenue.


If you are building on Vapi, Retell, LiveKit, or any of the other stacks in this space, and you want a regression suite that exercises tool calls the way real callers do,[talk to us](https://roark.ai/) .
