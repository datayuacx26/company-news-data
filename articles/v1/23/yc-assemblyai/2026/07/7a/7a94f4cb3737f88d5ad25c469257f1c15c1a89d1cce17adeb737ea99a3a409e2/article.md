---
schema_version: "1.0.0"
document_id: "7a94f4cb3737f88d5ad25c469257f1c15c1a89d1cce17adeb737ea99a3a409e2"
company_key: "yc-assemblyai"
company: "AssemblyAI"
source_id: "yc-assemblyai-news-import-c38147bde659"
canonical_url: "https://www.assemblyai.com/blog/introducing-universal-streaming"
published_at: null
first_seen_at: "2026-07-24T08:06:00.112884+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:dc9f2a3aaa1e8315c061c0c1028868bc1c52e98a457b95f07f2293bfcddc1ab5"
---

# Speech-to-Text for voice agents - Universal-Streaming

We have released new and improved models since this article was published. See[our docs](https://www.assemblyai.com/docs) for our most current model offerings.


Voice agents have leapt forward in the past year, yet they still stumble—misheard account numbers, awkward pauses, and cutting off users before they finish speaking.


**Universal-Streaming** is our new, purpose-built speech-to-text (STT) model that fixes those gaps with immutable transcripts in ~300ms, superior accuracy, intelligent endpointing, and unlimited concurrency. Priced at just **$0.15 per hour** , builders can ship voice agents that feel more natural, finish tasks successfully, and scale without surprise fees.


Try Universal-Streaming


Universal-Streaming is available now, get started today.


[Try it now](https://www.assemblyai.com/docs/speech-to-text/universal-streaming)


## **Addressing voice agents' most critical challenges**


Voice agent developers face distinct technical challenges that directly impact user satisfaction and task completion rates. Universal-Streaming provides significant improvements in five key areas:


1. **Immutable transcripts in ~300 ms** - understands audio content as users speak.
2. **Accuracy on the tokens that matter** - email addresses, confirmation codes, and product IDs that are captured correctly the first time.
3. **Intelligent endpointing** – combines acoustic and semantic features with traditional silence-based methods for more effective and accurate end-of-turn detection with built-in fallback reliability.
4. **Transparent pricing with unlimited concurrency** - scale from 5 to 50,000 streams with no caps, stream fees, or prepaid reservations—just $0.15/hr pricing based on total session duration, not audio duration or pre-purchased capacity.
5. **Background noise robustness** - 73% fewer false outputs from noise compared to Deepgram Nova-2, with 28% improvement over Deepgram Nova-3.


Here's how Universal-Streaming performs compared to Deepgram Nova-3:


These improvements directly translate to more successful voice interactions, higher task completion rates, and improved user satisfaction.


> *Universal-Streaming is amazing for our real-time note-taking experience. The speed difference is immediately noticeable - our users see their conversations transcribed almost instantaneously. It feels so much more responsive than what we were using before. We're excited to roll this out to all our users - it's exactly the kind of breakthrough the voice AI space has been waiting for. ~Jonathan Kim, Software Engineer at*[Granola](https://www.granola.ai/)


## **Under the hood: what makes Universal-Streaming unique**


Universal-Streaming is built from the ground up to address the real-world challenges voice agents face today. It delivers lightning-fast immutable transcripts, intelligent turn detection, and superior accuracy—all at just $0.15/hr through a robust API designed specifically for modern voice applications.


### 1. Ultra-low latency with immutable transcripts


Reliable and fast transcription is critical for voice interfaces. To realize natural conversation flows, voice agent developers must squeeze speech understanding and associated processing logic within a limited time frame.


Universal-Streaming flips the usual “partials-now, finals-later” model on its head:


- **Word emission within ~300 ms** - lightning fast transcription, offering opportunities for voice agents to handle user interruptions intelligently based on the spoken content (e.g., agents can distinguish backchannels and short acknowledgements from substantial interruptions in real-time)
- **41% faster median latency** in word emission than Deepgram Nova-3 (307 ms vs 516 ms) and **nearly 2× faster** on P99 latencies (1,012 ms vs 1,907 ms)
- **Immutable from the start** - **** what the industry calls a *final* lands on your websocket from the outset, meaning that transcripts won't be revised once received, allowing your agent to start thinking while the user is still talking **‍**
- **Latency-tunable features** - toggle punctuation and auto-casing per request for maximum response speed


Other solutions stream *mutable partials* that can be retroactively edited until a final arrives—forcing you to choose speed ( *use partials* ) or reliability ( *wait for immutable finals* ). Universal-Streaming removes that dilemma: every character emitted is final and never changes, and the model does this so fast, even generating subwords before completing full words. This allows you to utilize all transcription information and implement processing logic within a natural conversation timeframe.


### 2. Accuracy where it matters — emails, codes, and names


Voice agents often struggle with the content that matters most to users: email addresses, confirmation codes, product identifiers, and proper names.


Universal-Streaming delivers substantial improvements in these challenging areas without the latency tradeoff:


- **12% overall recognition improvements** , ensuring superior accuracy across the board
- **21% fewer alphanumeric errors** on entities like, order numbers and IDs so they’re not propagated to downstream processing
- **5% improvement in proper noun recognition** for names of people, products, and businesses


Universal-Streaming now tops **91% overall word accuracy** on noisy, real-world audio. This means fewer correction loops and smoother conversations. More importantly, it helps prevent *silent transcription errors* — when the system confidently mishears you (like "Tuesday at 2:30" instead of "Thursday at 2:30") and proceeds without question, leading to missed appointments and frustrated users.


### 3. Intelligent endpointing for smoother turn detection


Traditional speech-to-text systems rely primarily on Voice Activity Detection (VAD) to determine when a user has finished speaking. Voice agent developers are forced to choose either awkward pauses (long silence) or the risk of making premature interruptions (short silence), both of which disrupt the natural flow of conversation.


Universal-Streaming integrates end-of-turn detection natively into the STT, making it ideal for voice agent applications. This is in contrast to current approaches in which legacy VAD systems that were not built for voice agents are appended onto STT models as a workaround. Instead, Universal-Streaming's intelligent endpointing combines acoustic and semantic features with traditional VAD for fast, more accurate end-of-turn detection.


This transforms the turn-taking experience:


- **Semantic understanding** of natural speech patterns rather than simple silence thresholds
- **Support for natural pauses** and thinking time without premature responses
- **Smooth conversation flow** without awkward interruptions or artificial delays


And like all aspects of Universal-Streaming, developers maintain full control with configurable silence thresholds and confidence parameters to fine-tune the experience for your specific use case. The result is a voice experience that feels more intuitive and responsive while still giving you the flexibility to optimize for your unique requirements.


### 4. Transparent pricing with unlimited concurrency


Voice has graduated from an experimental to a frontline channel; traffic can spike from **5 to 50,000+ streams** in seconds.


Universal-Streaming is architected—and priced—for that reality:


- **Transparent pricing** at just $0.15/hr —you pay only for the total session duration, not audio duration or pre-purchased capacity
- ‍ **Unlimited, autoscaling concurrent streams** with no hard caps or over-stream surcharges **‍**
- **Consistent performance from 5 to 50,000+ streams** without performance degradation or usage commitments **.** At least 99.9% uptime for your business-critical voice applications


All of this premium performance comes at a fraction of the cost of alternative streaming models and APIs. For real-time voice agents, we've simplified pricing to eliminate negotiation headaches that have become too common in the Voice AI space. Our straightforward, transparent pricing starts at $0.15/hr with volume discounts available for larger implementations. No complex tiers or hidden fees—just predictable pricing that scales with your success and lets you focus on building great voice experiences.


Plus, you no longer need to keep idle streams open to avoid cold-start lag—scaling up or down is quick and cost efficient. This enterprise-grade reliability ensures your voice applications can scale confidently with user adoption, maintaining performance even during peak usage periods.


## **Quick integration with voice agent ecosystems**


Universal-Streaming integrates with the leading voice agent orchestration platforms and tools, enabling easy adoption without requiring architectural changes:


- **Drop-in compatibility** with[LiveKit](https://www.assemblyai.com/docs/integrations/livekit) ,[Daily.co via Pipecat](https://www.assemblyai.com/docs/integrations/pipecat) , Vapi, and other popular voice platforms.
- **Standard WebSocket interface** familiar to streaming audio developers
- **Client libraries** for JavaScript, Python, and other common languages
- **Comprehensive**[migration guides](https://www.assemblyai.com/docs/guides/dg_to_aai_streaming) for transitioning from other providers


This ecosystem compatibility ensures that you can quickly implement Universal-Streaming and begin seeing improvements in your voice agent performance without disrupting existing workflows or requiring extensive redevelopment.


> I'm really looking forward to seeing all the new things developers build with Universal-Streaming. This new release from AssemblyAI addresses the real pain points that voice AI developers face going from prototype to production, and then scaling in production: high quality audio understanding at ultra low latency, reliable performance across the full range of real-world contexts, and an easy to use API. We've worked closely with the AssemblyAI team to make sure that AssemblyAI + Pipecat and Pipecat Cloud delivers exceptionally performant voice understanding for our community and customers.
>
>
> ~ Kwindla Hultman Kramer, CEO at[Daily](https://www.daily.co/)


## **What's coming next**


Universal-Streaming is just the beginning of our voice agent-focused innovations. Here's a glimpse of what's on our roadmap:


- **Multi-region support** - EU region deployment for lower latency and data residency compliance
- **Expanded language support** - Adding more languages and dialects to our streaming capabilities
- **English code-switching** - Better handling of non-English terms and phrases within primarily English conversations


We're committed to continuously improving Universal-Streaming API based on your feedback and real-world usage patterns. Stay tuned for these enhancements and more in the coming months.


## **Start using Universal-Streaming today**


Universal-Streaming turns voice agents into what they should be—fast, accurate, intelligent, and ready for production. Don't settle for “good enough.”


**Three ways to get started:**


1. **Implement immediately** : Universal-Streaming is available now through our API. Simply open a websocket to our[wss://streaming.assemblyai.com/v3/ws](http://wss//streaming.assemblyai.com/v3/ws) endpoint using your current API key.
2. **Try the Playground** : Use our no-code[Playground](https://www.assemblyai.com/playground/streaming) to see Universal-Streaming's performance with your specific audio and use cases using our interactive testing environment.
3. **Explore the documentation** : Review our comprehensive[Getting Started Guide](https://www.assemblyai.com/docs/speech-to-text/universal-streaming) and[technical documentation](https://www.assemblyai.com/docs/api-reference/streaming-api/streaming-api) for detailed implementation information. We also have a[cookbook](https://www.assemblyai.com/docs/guides/v2_to_v3_migration) for migrating from the v2 to v3 API.


Start building with $50 in free credits


Start building with Universal-Streaming and create voice agents that feel natural, responsive, reliable, and genuinely helpful.


[Get started now](https://www.assemblyai.com/dashboard/signup)


## ***Notes***


**Measuring transcription latency** – for each correctly transcribed word, measure the time from when the word ends in the audio stream ( **x** ) to when that same word first appears in the partial transcript received by the client ( **y** ). Latency = *y − x* . Only words that are transcribed correctly are included in the calculation.


The latency is measured using audio files balanced across:


- **Clean speech** – LibriSpeech excerpts
- **Real-world calls** – internal benchmark recordings (retail support, drive-thru, B2B triage)
- **Stress clips** – crafted edge-cases (rapid speaker turns, burst noise, long silences)


This mix captures everyday usage *and* the extreme scenarios that typically break streaming systems.


**Measuring transcription accuracy** - We use word error rate (WER) for accuracy measurement. For comprehensive evaluation, we use datasets totaling more than 205 hours of audio. These datasets cover various domains, including meetings, broadcasts, and call centers, as well as a wide range of English accents. They also encompass various audio conditions, in terms of duration, signal-to-noise ratios, speech-to-silence ratios, and other factors.
