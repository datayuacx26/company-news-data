---
schema_version: "1.0.0"
document_id: "91d1cb06033b4557e33b5438f80a9a7b4a94a9eac721c3c9767ecb82943a7132"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/testing-voice-agents-mcp-servers"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T10:31:07.248745+00:00"
fetched_at: "2026-08-12T10:31:07.921779+00:00"
content_hash: "sha256:30bdb6f8feb8e2855c70e3c4a9d462812cc302d0c01715a78d08e387906002a9"
---

# Testing voice agents that call remote MCP servers

Twelve months ago, a voice agent that needed to look up an order status, book a slot, or refund a charge did it through a hand-written function call. Your client caught the tool event, ran the function, stuffed the result back into the session, and the model kept talking. That plumbing is disappearing. Every major voice stack now speaks Model Context Protocol directly:[OpenAI's Realtime API added remote MCP servers at GA](https://openai.com/index/introducing-gpt-realtime/) ,[xAI's Voice Agent Builder ships with MCP support out of the box](https://x.ai/news/grok-voice-agent-builder) , and[Azure Voice Live added MCP tool servers in the 2026-01-01-preview API](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/how-to-voice-live-mcp-server) . Point the session at a URL, the model discovers the tools, and the platform runs them for you.


That is a huge win for teams shipping voice agents. It is also a new class of failure that a chat harness cannot see. MCP calls now sit inside the same tight audio loop the caller hears, and every gotcha, expired tokens, silent dead air during a slow tool, schema drift on the server, gets translated into something a human on a phone line experiences in real time. This post is a practical guide to the failure modes MCP introduces on voice, and how to test for them before a real caller finds them.


## What actually changed when MCP hit the voice stack


With function calling, your application code is the middleman. The model emits` tool_call` , your client executes it, and you decide what goes back. With remote MCP, the platform itself connects out to your server.[OpenAI's docs are explicit about this](https://developers.openai.com/api/docs/guides/realtime-mcp) : "Unlike function tools, MCP tools are executed by the Realtime API itself." You lose the client-side hook where you used to inject retries, timeouts, redaction, and telemetry.


You gain, in exchange, a few things worth testing:


- **Auto-discovery.** The session lists tools from the MCP server at connect time. If discovery is slow, the caller waits.
- **Session-level tool surface.** Change the MCP server, and the tool set changes without a redeploy of your agent. That is a feature until it becomes a regression source.
- **Platform-managed auth.**[The Realtime session accepts either an authorization value or a headers map, not both](https://tryskilly.app/learn/openai-mcp-server-tutorial/) , and token expiry is now something your platform has to notice mid-call.
- **Approval flows.**[Azure Voice Live exposes a RequireApproval mode where a tool call can be gated on explicit user approval before it runs](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/how-to-voice-live-mcp-server) . On a voice channel, that approval is a spoken question and a spoken answer, and it has to work.


The interesting part is not the mechanism. It is that the boundary of "your agent" now includes a remote service you may not control, sitting on the audio critical path.


Where an MCP tool call sits inside a voice turn


## The failure modes chat never sees


Text harnesses have been testing MCP for months. There are decent guides on hit rate, tool selection, and parameter assertions,[Merge's write-up is a reasonable primer](https://www.merge.dev/blog/mcp-server-testing) . What those guides miss is that voice compresses the acceptable-latency budget by an order of magnitude and forces a set of behaviors that only exist over audio. Six failure modes worth calling out:


**1. Discovery-time dead air.** A voice session that opens with` mcp_list_tools` blocks the model from responding until the list returns. If your MCP server is cold, or behind a slow authenticated proxy, the caller hears a two-second silence before the greeting. Some models will fill that silence with a random greeting they generate without the tools bound, which is worse.


**2. Mid-tool silence.** A synchronous MCP call that takes 1.5 seconds does not feel like anything in a chat window. On a phone line it is a hang. The agent needs a filler ("give me one second while I pull that up") and the model needs to actually emit it. Whether it does depends on the prompt, the model, and sometimes the phase of the moon. This has to be measured, not assumed.


**3. Auth token expiry mid-call.** In production the top failures are[an expired token returning 401 and firing mcp_list_tools.failed , or a wrong server_url path](https://tryskilly.app/learn/openai-mcp-server-tutorial/) . On chat you retry. On voice the caller is already ten seconds into a sentence when the tool call fires. What does your agent say when it can't reach the tool? Test it.


**4. Schema drift without a redeploy.** MCP's whole pitch is that tools live behind a URL, so[an existing MCP server wired to a CRM can be attached to a voice agent in the same step as the prompt](https://pondero.ai/news/2026-07-03-xai-voice-agent-builder/) . Great. It also means the CRM team can ship a breaking change to` refund_order` on a Tuesday and your voice agent's regression tests would never notice, because there was no code change on your side.


**5. Verbatim playback of tool payloads.** Tools return JSON. Voice agents have to turn that JSON into something a human wants to hear. When they don't, callers get read a UUID, a timestamp in ISO 8601, or worse, the raw PII of another customer because a query returned too much. This is not a hypothetical: any MCP tool with looser scoping than you expected can leak into an audio channel.


**6. Approval flows that don't work in speech.** Approval-required tools are a good pattern for anything that spends money or changes state. But the transcript has to be unambiguous ("I'm about to charge $147.20 to the card ending in 4412, is that correct?"), the yes/no detection has to be robust, and the "no" path has to actually cancel the call, not fire the tool anyway.


## A concrete test matrix


For every MCP tool your agent can invoke, you want scenarios that cover four axes: discovery, invocation, response handling, and failure. In a sim harness this looks less like a unit test and more like a scored phone call. Roughly:


Axis What to simulate What to score


Discovery Session open with cold MCP server, warm server, unreachable server Time to first agent speech; whether tools list contained the expected names


Invocation Persona asks for each tool by natural intent (not by name) Correct tool selected; parameters extracted correctly from the caller's speech


Latency Force a 1s / 2s / 4s MCP response delay Whether the agent emits filler audio; total time to spoken result


Response Return large payloads, empty payloads, payloads containing PII of unrelated records Whether the agent summarises or reads verbatim; whether it leaks PII


Auth failure Rotate token to invalid mid-session Whether the agent recovers, escalates, or hangs


Approval Approval-required tool with caller saying "yes", "no", "not sure", and silence Whether the tool fired, whether it was cancelled, whether the confirmation was accurate


The rule of thumb is that if a scenario cannot be described as a phone call you could imagine a real person making, it does not belong in a voice MCP test suite. And if it can, it should live in a regression pack you run on every prompt change and every MCP server change, not just when you push new agent code.


One row of an MCP sim run against a booking agent


## Where a simulation harness earns its keep


You can build the above with a few Python scripts and a lot of patience. Some teams do. But three properties make MCP testing genuinely painful without a dedicated harness.


First, you need real audio. Text-only evals will happily tell you that an agent picked the right tool for the intent "I want to reschedule Tuesday's 3pm to Thursday". They will not tell you what happens when that sentence is delivered with a Glasgow accent over a 3G call. MCP testing needs the same audio realism as any other voice test, and the ecosystem's mainstream stacks ([Vapi, Retell, LiveKit, Pipecat, Bland, ElevenLabs](https://roark.ai/) ) all sit behind real PSTN or WebRTC endpoints for a reason.


Second, you need to exercise the whole loop including the MCP server. That means dialling your production endpoint, not stubbing the tool. Otherwise you are testing a fictional agent that runs against a fictional CRM. Roark's simulation testing[dials voice agents over real telephony and WebRTC](https://roark.ai/) so the MCP call executes end-to-end with the same latency and auth path a real call would take.


Third, you need to score audio-native things: whether the agent emitted filler audio, how long the caller waited between the last word of their request and the first word of the response, whether the tool payload was verbalized cleanly or read as raw JSON. WER and intent classification will not catch any of these. Roark scores pauses, pace, pronunciation and interruption behavior on every call, and issues get filed automatically when a metric fails. When something breaks that you didn't have a metric for,[custom metrics](https://docs.roark.ai/) let you write the check once and run it against every future call.


What a scored call looks like when an MCP tool call goes long


## Turning MCP incidents into regression tests


The most valuable property of voice testing shows up after launch, not before. When a real caller trips over an MCP failure, whether it's a slow tool, a bad payload, or a wrong tool selected from a homophone, you want that call to become a permanent test.


Roark's production call replay captures the real call and replays it against the updated agent, so a genuine "I want to cancel the second appointment, not the first" ambiguity that fooled the model in production becomes a fixed regression the next release has to pass. That matters more for MCP than for prompt logic, because MCP failures often come from the tool side and you need a way to catch them without waiting for a human to notice. If your CRM's` list_orders` tool starts returning` null` instead of an empty array on a Tuesday, you want yesterday's captured calls to fail today's suite before the next real caller hits it.


## The short version


MCP moved a big chunk of "what the agent does" from your code into a network hop the platform manages for you. That is worth it. The trade is that the failure surface got wider and now sits inside the audio loop where callers can hear it. If your voice testing has not yet grown scenarios for cold-start discovery, mid-tool silence, auth expiry, schema drift, verbatim playback, and spoken approval, those are the six places to start. And if you're running MCP-backed agents on top of[Vapi, Retell, LiveKit, Pipecat, Bland, or ElevenLabs](https://roark.ai/) , the simulation and replay tooling to test them the same way you test anything else on the audio path is already there. Wire it up before your next MCP server changes on a Tuesday.
