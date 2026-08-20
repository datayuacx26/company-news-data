---
schema_version: "1.0.0"
document_id: "135b96517aac35d4a1a7dca280b56485623e7a4b9918ce69382c45e060284fec"
company_key: "backblaze-inc-class-a-common-stock"
company: "Backblaze Inc."
source_id: "backblaze-inc-class-a-common-stock-rss-a06767c1ff83"
canonical_url: "https://www.backblaze.com/blog/introducing-genblaze-a-python-sdk-for-generative-media-pipelines/"
published_at: "2026-06-11T19:21:40+00:00"
first_seen_at: "2026-07-20T23:18:53.525659+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:7017295fd77aa20e3938d8a4837629d3e4f621256851d40acd6a8e9a53d7f197"
---

# Introducing Genblaze: A Python SDK for Generative Media Pipelines

A year ago, a good video model was a novelty. Today there are at least six worth using, and most of the teams we talk to are wiring up two or three of them into the same product, alongside image models, voice synthesis, and music generation. The hard question isn’t whether you can generate this kind of media. It’s how to build a pipeline that handles five providers without falling over.


That’s why we built Genblaze, an open-source Python SDK from Backblaze for building generative media pipelines: one API across video, image, and audio providers, swappable models, durable object storage, and a SHA-256-verified provenance manifest on every run.


## **The pipeline is becoming the moat**


Models are commoditizing. New video, image, and audio releases drop every couple of months, and each one tends to be the best at one specific thing and middling at the rest. Nobody we work with is betting on a single provider anymore. They build a portfolio and configure fallbacks.


The pipeline is what stays. It’s where you’ve figured out which model handles which shot type and which voice fits which brand. It’s where retry logic and output guards live, and where your audit trail comes from. That work survives the next model release. The prompts you tuned for last quarter’s hero model don’t.


For a pipeline to actually be durable, though, it has to be reactive. Hard-coding one provider, blocking on every step, and returning a single synchronous result is fine for a demo. In production it ages out in weeks. The pipelines that hold up stream progress as events, fan out concurrent work, handle backpressure from slow providers, and let you add a new model with a one-line change.


That’s what Genblaze is designed to be. One pipeline object, every provider behind the same surface, and a new model is one more` .step()` .


## **A workflow that uses five providers**


Here’s a concrete example: producing a short brand film from a one-paragraph brief.


**1. Storyboard frames.** Lock the visual direction with Seedream 5.0 Lite or FLUX via GMI Cloud, or Imagen on Google.


**2. Animate the approved frame.** Kling image-to-video on GMI Cloud, Veo on Google, Runway Gen-4 Turbo, or Luma Ray-2. They’re good at different shot types, so we usually try two and pick. Setting` chain=True` on the pipeline passes the image from step one into the video step automatically.


**3. Score and sound design.** Music from Stability AI’s Stable Audio or GMI Cloud’s MiniMax. Ambient effects and voiceover from ElevenLabs. LMNT for low-latency text to speech (TTS) when responsiveness matters.


**4. Upscale.** There’s an upscale step type built in. Route the rendered video through a Replicate upscaler like Real-ESRGAN to hit delivery resolution.


**5. Classify and tag.** Use a vision-capable` chat()` call to tag scenes, run brand safety checks, or generate accessibility metadata. Gemini 2.5, GPT-4o, or Llama 3.2 Vision on GMI Cloud all handle this.


That’s five providers across five different model types, defined in one pipeline. The same retry behavior, fallback chains, and provenance manifest apply to every step.


```text
from genblaze_core import Pipeline, Modality  from genblaze_gmicloud import (      GMICloudImageProvider, GMICloudVideoProvider, GMICloudAudioProvider,  )  from genblaze_replicate import ReplicateProvider  from genblaze_google import GeminiChatProvider   run, manifest = (      Pipeline("brand-film", chain=True)      .step(GMICloudImageProvider(), model="seedream-5.0-lite",            prompt="...", modality=Modality.IMAGE)      .step(GMICloudVideoProvider(), model="Kling-Image2Video-V2.1-Master", prompt="...", modality=Modality.VIDEO)      .step(GMICloudAudioProvider(), model="minimax-music-2.5",            prompt="...", modality=Modality.AUDIO)      .step(ReplicateProvider(),     model="nightmareai/real-esrgan",      step_type="upscale")      .step(GeminiChatProvider(),    model="gemini-2.5-pro",               step_type="classify",            prompt="Tag scenes, return JSON with shots, mood, brand-safety flags.")      .run(sink=storage, timeout=900)  )
```


Swap any step for a different provider and nothing else in the pipeline has to change.


## **Provenance**


Every run produces a canonical, hash-bound manifest that records the provider, model, prompt, parameters, timestamps, and the URI of every asset it produced. You can embed it directly into the output file (.mp4, .png, .jpg, .webp, .mp3, .wav are all supported by the matching media handler), or persist it as a sidecar JSON.


The hash is deterministic, so anyone downstream can verify the file by calling` manifest.verify()` . The same manifest is replayable:` genblaze replay manifest.json` reconstructs the run with the same parameters. And because every manifest carries a` parent_run_id` , you can trace a v3 video back through v2 and v1, including the fork where you tried Runway instead of Kling.


If you’re building customer-facing pipelines, this is what gets you from “we generated this” to “here’s the proof.”


## **Storage**


Assets and manifests land wherever you want. We default to Backblaze B2, which the SDK wires up with` ObjectStorageSink(S3StorageBackend.for_backblaze("my-bucket"))` and which gives you durable URLs that don’t expire and don’t need credentials to fetch. The same sink works against any S3-compatible store: AWS S3, Cloudflare R2, MinIO.


A few B2 features pair particularly well with this kind of pipeline.


**Event Notifications** fire to a webhook, queue, or function endpoint when an asset or manifest lands. That gives you a clean way to kick off downstream encoding, indexing, or moderation without polling.


**Object Lock** lets you write manifests under a retention policy that nobody (not even the account root) can overwrite until the window expires. Combined with the SHA-256 hash inside the manifest, you’ve got cryptographic integrity and storage-layer immutability.


**Lifecycle rules** handle the cleanup. Final assets and manifests stay around as long as you want them to. Storyboard iterations, rejected takes, and pre-upscale renders prune themselves on whatever schedule you set.


## **Partnering with GMI Cloud: a unified AI inference platform for open source**


GMI Cloud is a unified AI inference platform for open source. It supports LLM, image, video, and multimodal inference through one consistent API. The catalog covers Seedance, Kling, Veo, and Wan for video; Seedream and FLUX for image; MiniMax for music; ElevenLabs voices; and Llama, DeepSeek, and Qwen for chat and multimodal. One API key reaches all of it.


The` genblaze-gmicloud adapter` maps GMI’s image, video, audio, and chat endpoints onto the pipeline surface and tracks their catalog as new models ship. The first sample app below uses it heavily.


## **Two sample apps**


[genblaze-gmicloud-pipeline](https://github.com/backblaze-labs/genblaze-gmicloud-pipeline) goes deep on a single provider. A prompt becomes an anchor image via seedream-5.0-lite. You iterate by passing the current image to` flux-kontext-pro` for reference-based refinement. Once you approve a frame, the app fans out concurrently to three video models (Kling-Image2Video-V2.1-Master, wan2.6-i2v, pixverse-v5.6-i2v). Manifests get written to B2 next to the assets, and the Genblaze integration sits in a single ~100-line file.


[genblaze-gen-media-multi-provider-sample](https://github.com/backblaze-labs/genblaze-gen-media-multi-provider-sample) is the workflow above end to end. One sentence becomes a narrated, scored, captioned MP4. gpt-4.1-nano writes the storyboard, Imagen 4 produces the keyframes, Decart Lucy or GMI Cloud Kling animates them, NVIDIA Magpie TTS narrates, GMI Cloud MiniMax scores, and ffmpeg composes the final video. Five providers, one pipeline, every artifact ending up in B2 with a verifiable manifest.


Both are MIT-licensed. Clone, fill in .env, run.[GMI Cloud docs can be found here.](https://docs.gmicloud.ai/model-quickstarts/multimodal-overview)


## **Get started**


bash


```text
pip install genblaze
```


That umbrella installs` genblaze-core` plus the B2/S3 storage backend, which is enough for a working provenance pipeline. Add` genblaze\[gmicloud\]` ,` genblaze` , or` genblaze\[all\]` to pull in providers.


### **Where this goes**


Most of the interesting work in generative media is happening above the models now, in the pipelines that string them together. Whatever model you’re using today probably won’t be your favorite in six months. The orchestration around it is what lasts.


Genblaze is what we built. It’s MIT-licensed and lives at[github.com/backblaze-labs/genblaze](https://github.com/backblaze-labs/genblaze) .
