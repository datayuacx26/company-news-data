---
schema_version: "1.0.0"
document_id: "c7a3584600c4347d28f436549a48551c3eb4e4cd3e9154a1e1262bf859e9d694"
company_key: "twilio-inc-class-a-common-stock"
company: "Twilio Inc."
source_id: "twilio-inc-class-a-common-stock-rss-c0df8d7be67f"
canonical_url: "https://www.twilio.com/en-us/blog/developers/tutorials/product/handle-background-noise-when-using-conversation-relay"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T23:03:59.381469+00:00"
fetched_at: "2026-07-28T23:04:00.097062+00:00"
content_hash: "sha256:2d7859a812b4fa214da719ab96bd6afe953246154852b7dffa9ca1c1d2c4e74a"
---

# How to Handle Background Noise When Using Conversation Relay

Time to read:


-
-
-
-
-


July 28, 2026


**Written by**[Dylan Frankcom](https://www.twilio.com/en-us/blog/authors/author.dylan-frankcom) Twilion


**Reviewed by**[Amanda Lange](https://www.twilio.com/en-us/blog/authors/author.amanda-lange) Twilion


[Dhruv Patel](https://www.twilio.com/en-us/blog/authors/author.dhrpatel) Twilion


[Matthew Setter](https://www.twilio.com/en-us/blog/authors/author.msetter) Twilion


---


## How to Handle Background Noise When Using Conversation Relay


Background noise is one of the most common real-world failure modes for voice AI agents. When your speech-to-text model mishears a caller, the entire response chain breaks down from that first bad transcript. This tutorial walks you through choosing the right Deepgram speech model for noisy conditions in a[Twilio Conversation Relay](https://www.twilio.com/en-us/products/conversational-ai/conversationrelay) agent, and shows you which TwiML (Twilio Markup Language) attributes to tune when the defaults fall short.


## Programming Language Support


This tutorial is geared towards Python developers, but you can find other languages below:


- [Javascript](https://www.twilio.com/en-us/blog/developers/tutorials/product/handle-background-noise-when-using-conversation-relay-nodejs)
- [C#](https://www.twilio.com/en-us/blog/developers/tutorials/product/handle-background-noise-when-using-conversation-relay-in-csharp)
- [PHP](https://www.twilio.com/en-us/blog/developers/tutorials/product/handle-background-noise-when-using-conversation-relay-in-php)


## Prerequisites


To follow this tutorial you will need:


- The[Owl Air phone agent](https://www.twilio.com/en-us/blog/developers/tutorials/product/ai-phone-agent-twilio-conversation-relay) from the previous tutorial, or any Conversation Relay agent built with Python and FastAPI
- A voice-capable Twilio phone number
- [ngrok](https://ngrok.com/) running and your` DOMAIN` environment variable set


## How background noise affects voice AI


Before tuning anything, it helps to understand exactly where noise causes issues. When a human call center agent hears a noisy caller, they can ask for clarification or piece together meaning from context. A voice AI agent cannot do this. It processes a chain of three sequential steps, and noise corrupts the very first one.


The chain looks like this: the caller speaks,[Deepgram](https://deepgram.com/) converts that speech to text (STT), your server passes the text to the language model (LLM), the model replies in text, and[ElevenLabs](https://elevenlabs.io/) converts the reply to speech (TTS). If background noise causes Deepgram to transcribe something incorrectly, that error is what reaches the LLM. The model has no audio to fall back on. It can only work with what the transcript says. The result could be a confused or nonsensical response.


All the leverage is at that first step. If the STT transcript is accurate, the rest of the chain works correctly regardless of what the caller's environment sounds like.


## Testing methodology


To get concrete findings rather than speculation, the testing covered four conditions on speakerphone at arm's length: no noise (quiet room baseline), a vacuum cleaner (~70 to 75 dB at one metre), an intermittent power drill (~85 to 90 dB), and both running simultaneously.


Nova-2 was also evaluated briefly at the outset and ruled out. It underperformed Nova-3 even in the quiet baseline picking up audio from a podcast playing quietly on a nearby speaker, so it wasn't carried forward into the noise conditions.


Three configurations made it through the full testing suite: the baseline Deepgram Nova-3 config from the previous tutorial; a tuned Nova-3 config with the attribute changes described later in this post; and Deepgram Flux on its default configuration.


Each call followed a six-exchange script covering policy questions, flight number lookups, number transcription, low-information input, incomplete phrases, and deliberate interruptions. The terminal logged transcripts via the` CALLER:` log lines. A failure counted when Deepgram's output differed meaningfully from what was said in a way that changed the agent's response.


All three configurations handled single-noise conditions without issue. The gap only showed up under combined noise: default Nova-3 produced two transcription failures, while tuned Nova-3 and Flux both passed cleanly.


## Model configuration


### Nova-3


The previous tutorial used` nova-3-general` . Nova-3 handles single-noise conditions cleanly. The default config does break under worst-case combined noise, however. Two exchanges failed: Deepgram transcribed "Can you explain the loyalty points program in detail" as "Can you explain the loyalty forms program in detail", and "What's the check-in window?" came through as "What's the second window?".


Two attribute changes brought the error count to zero.


#### Reducing false interruptions


` interruptSensitivity` controls how easily background sound can interrupt Hoot mid-response. The previous tutorial used` medium` . In a noisy environment,` medium` sensitivity can cause Hoot to stop speaking every time a noise spike crosses the detection threshold.


Setting it to` low` raises that threshold so only deliberate speech interrupts the agent:


Copy code


```text
interruptSensitivity="low",
```


The tradeoff is that a caller who genuinely wants to interrupt will need to speak a little more deliberately. For most support calls, that is a reasonable price for an agent that does not stop mid-sentence every time a power tool runs nearby.


#### Suppressing backchannel responses


` ignoreBackchannel` controls whether Conversation Relay responds when a caller makes a brief affirmative sound while Hoot is actively speaking. Sounds like "uh huh", "yeah", or "mm" signal the caller is listening rather than taking a turn. When set to` true` , those sounds are ignored and Hoot continues without treating them as a new prompt:


Copy code


```text
ignoreBackchannel="true",
```


This setting is distinct from` interruptSensitivity` .` interruptSensitivity` governs whether any sound stops Hoot mid-sentence.` ignoreBackchannel` governs whether a recognised backchannel triggers a response once Hoot has finished speaking. You typically want both in a noisy environment:` interruptSensitivity="low"` to prevent noise spikes from cutting Hoot off, and` ignoreBackchannel="true"` to prevent a caller's "uh huh" from generating an unnecessary reply.


` ignoreBackchannel` only applies while Hoot is talking. A caller saying "yeah" as a standalone turn after Hoot has finished is a legitimate prompt, and the agent will respond to it regardless of this setting.


#### Nova-3 endpoint configuration


Here is the full` /twiml` endpoint with both attribute changes applied:


Copy code


```text
@app.post("/twiml")
async def twiml_endpoint():
response = VoiceResponse()
connect = Connect()
connect.conversation_relay(
url=f"wss://{DOMAIN}/ws",
welcomeGreeting="Thanks for calling Owl Air! I'm Hoot. I can help with flight status, baggage policy, loyalty points, or booking changes. Which of those can I help you with?",
ttsProvider="ElevenLabs",
transcriptionProvider="Deepgram",
speechModel="nova-3-general",
ignoreBackchannel="true",
hints="Owl Air, loyalty points, baggage, carry-on, check-in, boarding pass",
interruptible="any",
interruptSensitivity="low",
elevenlabsTextNormalization="auto",
)
response.append(connect)
return Response(content=str(response), media_type="text/xml")
```


The changes from the previous tutorial are` ignoreBackchannel` set to` true` and` interruptSensitivity` changed from` medium` to` low` . No other code in *main.py* changes.


### Flux


[Flux support was recently added to Conversation Relay](https://www.twilio.com/en-us/changelog/conversation-relay-now-supports-deepgram-flux---new-features) and is the better option for any agent that can adopt it. It passed all four noise conditions on its default config, including the worst-case combined condition, without any additional configuration. Beyond noise handling, it fuses transcription and turn detection into a single model. This eliminates the race conditions that cause agents to cut callers off mid-sentence, reduces response latency by 200 to 600ms, and cuts false interruptions by roughly 30 percent compared to Nova-3. It also adds native multilingual support across ten languages. See[Deepgram's Flux announcement](https://deepgram.com/learn/introducing-flux-conversational-speech-recognition) for the full breakdown.


To use it, update` speechModel` in your` /twiml` endpoint:


Copy code


```text
speechModel="flux",
```


Both` interruptSensitivity` and` ignoreBackchannel` are available on Flux as well if you want additional hardening on top of the defaults.


#### Tuning end-of-turn detection


Flux performed well across all noise conditions during testing without any additional tuning. If you are in an exceptionally loud environment and still seeing turn detection misfiring, Flux exposes` eotThreshold` as an additional dial. It controls how confident the end-of-turn (EOT) detector must be before finalising a transcript and defaults to` 0.8` . Lowering it to` 0.75` tells the detector to fire on reasonable confidence, rather than waiting for a certainty level that sustained noise may prevent it from reaching. Try not to go below` 0.7` , as lower values risk cutting off callers who speak slowly or pause mid-sentence. This attribute is not available on Nova-3.


Copy code


```text
eotThreshold=0.75,
```


#### Flux endpoint configuration


If you are upgrading to Flux, the only change is` speechModel` . The` interruptSensitivity` and` ignoreBackchannel` attributes are kept as additional hardening but are not required for Flux to pass the noise conditions tested here:


Copy code


```text
@app.post("/twiml")
async def twiml_endpoint():
response = VoiceResponse()
connect = Connect()
connect.conversation_relay(
url=f"wss://{DOMAIN}/ws",
ttsProvider="ElevenLabs",
transcriptionProvider="Deepgram",
speechModel="flux",
ignoreBackchannel="true",
hints="Owl Air, loyalty points, baggage, carry-on, check-in, boarding pass",
interruptible="any",
interruptSensitivity="low",
elevenlabsTextNormalization="auto",
)
response.append(connect)
return Response(content=str(response), media_type="text/xml")
```


If you are on Flux and still seeing turn detection misfiring after that, try lowering` eotThreshold` to` 0.75` .


## Preprocessing audio


Audio preprocessing libraries like` noisereduce` and the ElevenLabs Audio Isolation API are designed for offline processing and are not compatible with a Conversation Relay pipeline.


The core constraint is that your server never receives raw caller audio. Twilio routes inbound audio directly to Deepgram for transcription, and your server only receives JSON messages containing the finished transcript. There is no point in that path where you can intercept audio, clean it, and forward the result.


ElevenLabs Audio Isolation has a similar limitation. It processes audio files you upload and is only involved on the outbound side in a Conversation Relay call, converting text responses to speech. It has no role in the inbound audio path.


Twilio's` <Stream>` verb can fork raw audio to your server, but it cannot run alongside` <ConversationRelay>` .` <ConversationRelay>` takes full ownership of the call's media pipeline, and` <Stream>` blocks subsequent TwiML execution once it connects. Using` noisereduce` in a Twilio pipeline would require replacing Conversation Relay entirely with a custom Media Streams architecture: your own STT, LLM, and TTS with audio preprocessing in the middle. That is a significantly larger project and introduces latency at every additional hop.


## Conclusion


Talking on speakerphone while simultaneously running a vacuum cleaner and a power drill is most likely a rare occurrence, but it did produce a clear answer. Flux handled all four noise conditions on its default config without any additional changes. For agents that cannot move to Flux yet, Nova-3 with` interruptSensitivity="low"` and` ignoreBackchannel="true"` brings it to zero failures under the same worst-case conditions. See the[Conversation Relay documentation](https://www.twilio.com/docs/voice/twiml/connect/conversationrelay) for the full attribute reference.


*Dylan Frankcom is a Software Engineer on Twilio's Developer Content team. He builds educational content to help developers get the most out of Twilio's APIs. You can reach him at dfrankcom \[at\] twilio.com or find him on*[LinkedIn](https://ca.linkedin.com/in/dylanfrankcom) *.*
