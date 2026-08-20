---
schema_version: "1.0.0"
document_id: "177e2e38d8b688a59854d6bad932826899a986e2eec093583b198529360fde43"
company_key: "yc-lemonslice"
company: "LemonSlice"
source_id: "yc-lemonslice-news-import-9806857f7d31"
canonical_url: "https://lemonslice.com/blog/lemonslice-flash"
published_at: "2026-04-17T00:00:00+00:00"
first_seen_at: "2026-07-24T02:14:03.501451+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:6eb872457f8c5fad7af4a5ee3ce32fd8f9d7d147c74adfc4a63d983d26c4e480"
---

# LemonSlice 2.1 Flash: A new benchmark in interactive avatars

# LemonSlice 2.1 Flash: A new benchmark in interactive avatars


Today, we’re announcing LemonSlice 2.1 Flash, a video diffusion transformer model (DiT) and inference framework optimized for real-time, interactive avatars with state-of-the-art latency. Leveraging a novel model architecture and system-level optimizations, LemonSlice 2.1 Flash surpasses comparable avatar models in end-to-end latency benchmarks, while also offering key advantages as a one-stage DiT-based pipeline. We report an average time-to-first-byte of 471ms, measured from the time the first conditional audio chunk is received. When paired with third-party TTS, LLM, and STT components, LemonSlice 2.1 Flash achieves an average end-to-end response latency of 2.04s, enabling deployment in latency-sensitive applications.


### Optimized for Speed


We previously released[LemonSlice 2](https://lemonslice.com/research) , a 20-billion parameter real-time avatar model that runs on a single GPU. Key features included zero-shot avatar generation from a single image, the ability to animate any style of character, expressive gestures, and infinite-length generation without error accumulation. Our LemonSlice 2.1 model release introduced additional controls, including gesture and emotion manipulation so that avatars can update their behavior in real-time to a conversation's context.


LemonSlice 2.1 Flash builds upon this foundation with targeted changes that reduce end-to-end latency. Achieving this required a system-level approach, with optimizations spanning both model architecture and runtime execution. Rather than relying on a single improvement, we focused on reducing overhead across each stage of the inference pipeline.


At the model level, we introduce modifications to the diffusion transformer that enable more efficient incremental inference, reducing per-step compute without degrading output quality. The VAE decoding path was similarly optimized to minimize latency during frame generation.


At the systems level, we leverage CUDA graph capture to reduce CPU-side dispatch overhead and streamline GPU execution. We also implement a tightly coordinated CPU-GPU execution model that minimizes idle time and overlaps compute with data movement where possible.


Finally, our runtime incorporates purposeful queue management to ensure high throughput and avoid bottlenecks across asynchronous components such as audio processing and model inference. Together, these optimizations reduce both time-to-first-byte and end-to-end latency. We run inference on[Modal](https://modal.com/) , who's been a great partner in our quest for ultra-low latency.


In controlled testing, we find that LemonSlice 2.1 Flash delivers an average time-to-first-byte of 471ms. When paired with fast third-party STT, LLM, and TTS components, LemonSlice 2.1 Flash achieves an average end-to-end response latency of 2.04s.


Summary of key latency measurements with LemonSlice 2.1 Flash


### Quantitative Latency Measurement


To measure end-to-end latency, we connect to a LemonSlice-powered avatar using the publicly available[LiveKit Agents](https://docs.livekit.io/agents/) framework. Each call consists of 10 automated back-and-forths between a synthetic client and a locally hosted LiveKit agent. A prerecorded audio clip containing the utterance “hello” is played into the call, and the avatar is instructed via the LLM’s system prompt to respond with a random number. To minimize bias, we repeat data collection across 5 calls (N=50 total responses), with the agent reinitialized each time.


Speech segments are extracted from each call by applying an RMS envelope to raw audio with hysteresis thresholding. We define the end-to-end latency of each response as the time difference between the end of a user's utterance and the onset of the corresponding avatar response.


Representative audio trace illustrating extracted latency measurements


### Comparison with Other Models


We compare LemonSlice 2.1 Flash against other commercially available avatars by replacing the LiveKit` AvatarSession` with the corresponding LiveKit integration. All data was collected on the same date to minimize bias (April 15, 2026), and using the same STT/LLM/TTS models (see Appendix). Only the corresponding` AvatarSession` object was changed.


We find that LemonSlice 2.1 Flash is significantly faster than Tavus Phoenix-4 (p=8.1e-5; two-sided Welch t-test) and Anam Cara-3 (p=3.2e-10), while having comparable latency to LiveAvatar (p=0.83) and Simli (p=0.19). LemonSlice 2.1 Flash is also the fastest when measuring p75, p90, p95, and p99 percentiles.


LemonSlice 2.1 Flash is either comparable or faster than other avatar models with respect to end-to-end response latency


We note that LemonSlice 2.1 Flash is also a general purpose DiT model that can work with any reference image, does not require per-avatar fine-tuning or training, and also supports precise in-call gesture and emotion control.


Feature LemonSlice LiveAvatar Simli Anam Tavus


Instant Avatar from 1 Image ✓ x x ✓ x


Cartoon Avatars ✓ x ✓ ✓ ✓


Animal Avatars ✓ ✗ ✗ ✗ ✗


Hands/Gestures ✓ ✓ ✗ ✗ ✗


Whole-Body Avatars ✓ ✗ ✗ ✗ ✗


Your browser does not support the video tag.


### Appendix


To make our results reproducible by the broader community, we share the LiveKit agent and avatar configurations of third-party models used for testing. The LLM agent prompt was "Always respond with a random number between 1 and 100. Never repeat a number until you have reused them all." With other system prompts, latencies may be slower due to additional LLM processing. We note that hosting the LiveKit agent locally likely introduced additional latency in the system that would not be encountered in production environments.


Agent Component Model


STT deepgram/nova-3


LLM qwen/qwen3-32b (groq)


TTS cartesia/sonic-turbo


Vendor / Model AvatarSession Config


LemonSlice 2.1 Flash agent_image_url: https://lemonslice.com/flash-test.jpg


LiveAvatar avatar_id: e9844e6d-847e-4964-a92b-7ecd066f69df


Tavus Phoenix-4 replica_id: r3f427f43c9d, persona_id: pfb078329b77


Anam Cara-3 avatarId: ca8726c8-36de-42cd-a6cf-97898e509a76


Simli face_id: 804c347a-26c9-4dcf-bb49-13df4bed61e8
