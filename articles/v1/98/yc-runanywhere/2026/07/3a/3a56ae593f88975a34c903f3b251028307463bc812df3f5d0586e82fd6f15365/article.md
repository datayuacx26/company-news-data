---
schema_version: "1.0.0"
document_id: "3a56ae593f88975a34c903f3b251028307463bc812df3f5d0586e82fd6f15365"
company_key: "yc-runanywhere"
company: "RunAnywhere"
source_id: "yc-runanywhere-news-import-450c102900e1"
canonical_url: "https://www.runanywhere.ai/blog/llm-hub-ios-case-study"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-22T12:25:05.894272+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:44e44a0494a554f8ef6d9ed034d9f412e7a358e017dbca27a77933cbfc9f9b2d"
---

# Android to iOS in Six Weeks, with the RunAnywhere SDK

In March 2026, indie developer **Timmy Qian** had a local-AI app called **LLM Hub** doing roughly **$2,000 a month in revenue** . Chat, document RAG, web search, voice, image generation, all of it 100% on-device. No accounts, no servers, no telemetry. He had one problem: no Mac.


On **March 7** he made the first commit to an empty` ios/` folder. On **April 21** , LLM Hub shipped on the App Store. Six and a half weeks, solo, with **RunAnywhere SDK** as the LLM inference backbone.


LLM Hub on iPhone running Gemma 4 locally. Fully on-device, no network required


The numbers


**6.5 weeks** from empty` ios/` folder to App Store


**$2K MRR** revenue before the iOS port


**1** developer, first Swift project


**16+** SDK patches upstreamed back to RunAnywhere


---


## Why This Was Supposed to Be Hard


Building an iOS LLM app from scratch means: compile llama.cpp with Metal, wire up embeddings, build a model registry and downloader, write a streaming pipeline that never locks the main thread, handle mmproj vision projectors, and get five or more chat-template families right, all in a language he had never written. That is months of plumbing before the first feature.


---


## What RunAnywhere Collapsed It Into


The whole runtime boots in a handful of lines, straight from` LLMHubApp.swift` :


swift


```text
1  try     RunAnywhere  .  initialize  (  environment  :     .  development  )     2   LlamaCPP  .  register  (  priority  :     100  )     3   MainActor  .  assumeIsolated   {     ONNX  .  register  (  )     }     4
5   registerRunAnywhereModelCatalog  (  )     6   Task     {     7        await     RunAnywhere  .  flushPendingRegistrations  (  )     8        _     =     await     RunAnywhere  .  discoverDownloadedModels  (  )     9   }
```


That turns on a unified, streaming, multimodal runtime: **llama.cpp with Metal for GGUF** and **ONNX Runtime for embeddings** , both shipped as prebuilt, signed XCFrameworks. Generation is one call:


swift


```text
1  let   stream   =     try     await     RunAnywhere  .  generateStream  (  prompt  ,   options  :   options  )     2   for     try     await   token   in   stream  .  stream   {     appendTokenToUI  (  token  )     }
```


Vision is one call:


swift


```text
1  let     (  stream  ,     _  )     =     try     await     RunAnywhere  .  processImageStream  (     2      image  :     VLMImage  (  image  :   uiImage  )  ,     3      prompt  :     "Describe this."  ,     4      maxTokens  :     256     5   )
```


The SDK also covers the unglamorous parts: a model registry with multi-file support (a GGUF and its vision projector registered together via` registerMultiFileModel` ), downloaded-model discovery, per-model context lengths, reasoning-token extraction, and per-stream metrics like tokens per second surfaced straight from the engine.


---


## One SDK, Six Tools


Every text-generation feature in the app routes through a single facade built on RunAnywhere:


Tool What it does


**AI Chat** Multi-turn, multimodal (text, image, PDF), sliding-window context management, streaming through` generateStream`


**Vibecode** An on-device IDE. The model streams patched files back, the app writes them to disk and previews HTML live in Safari


**VibeVoice** Hands-free voice loop: listen, generate, speak, repeat. LLM turns stream through RunAnywhere


**Translator** 50+ languages, including photo translation through the RunAnywhere vision path


**Scam Detector** Paste a suspicious message; the app pulls any URLs in it and analyzes everything locally. Screenshots go through` processImageStream`


**Writing Aid** Summarize, rewrite, fix grammar, generate code. Same facade, same streaming call


Plus **document RAG and persistent memory** : the app calls RunAnywhere's C core directly (` rac_embeddings_create` ,` rac_embeddings_embed` ) to run **EmbeddingGemma 300M** through the ONNX backend. Hybrid semantic and lexical retrieval, per-chat document pools, a global memory store, all offline. Your PDF never touches a network.


And one detail we did not expect: LLM Hub consumes **Apple Foundation Models through RunAnywhere too** . On iOS 26+ the app maps Apple's system model to the SDK's` foundation-models-default` id, so Apple Intelligence sits behind the same` generate()` call as every GGUF model, thinking-content extraction included.


---


## Vibecode: The Feature That Stopped Us


` VibeCoderScreen` is an IDE, not a chat. The user picks a workspace folder (persisted with a security-scoped bookmark), describes what they want, and the model streams patched files back through` RunAnywhere.generateStream` . The app extracts the fenced code block and writes it to disk. For HTML, a loopback HTTP server built on raw BSD sockets (deliberately avoiding Apple's Network framework so it never triggers the local-network permission prompt) serves the result on` 127.0.0.1` and opens Safari.


Vibecode: describe what you want, code streams in, live preview opens, all offline


A live context gauge estimates token usage against the loaded model's real context window, and when it fills, the app rolls into a fresh session on its own. You can vibe-code a working page on a plane.


---


## A Power User's Integration


The most credible part of this story is how deep the integration goes:


- **Hand-rolled chat templates** for five model families (Gemma, Gemma 4, Llama, Mistral, gpt-oss Harmony), passed through the SDK's raw-prompt mode so the app controls formatting token-for-token
- **Two-phase Harmony generation** for gpt-oss-20B: stream the analysis channel to completion, then re-prompt for the final answer, giving a clean thinking/answer split in the UI
- **Sliding-window context builder** that budgets chat history against the actual loaded context window, reserving a quarter for the response
- Mid-stream stripping of Gemma 4 thinking tokens, plus model-residency management so an LLM and a VLM never occupy memory at once


And because the SDK is open source, he could go further than the public API. Between April and June, Timmy tagged **16+ custom builds** of the RunAnywhere iOS SDK in his own repo (` ios-sdk-v0.19.7-patched-v2` through` v16` ): llama.cpp version bumps, a chunked-decode KV-cache fix, context-size forwarding, Metal batch-size tuning to stop out-of-memory crashes on 7B and 8B models, Gemma 4 prompt-format fixes. We took the changes back upstream. **That is the loop we want with every developer.**


---


## The Model Catalog This Unlocks


LLM Hub's iOS catalog, a 3,500-line` ModelData.swift` , registers everything through the RunAnywhere model registry. All streaming, all offline:


Model Notes


**Gemma 4** E2B / E4B / 12B up to the 31B MoE, with mmproj vision projectors


**Llama 3.2 1B / 3B** 17 quants each, 128K context


**OpenAI gpt-oss-20B** Harmony format with thinking-token support


**IBM Granite 4.x** Up to 512K context


**LFM-2.5 / Ministral-3 / Phi-4 Mini** Including vision variants


**EmbeddingGemma 300M** ONNX backend, powers RAG and memory


**Apple Foundation Models** iOS 26+, bridged through the same SDK


**Custom GGUFs** User imports migrate into the SDK-managed folder so` loadModel(id)` just works


---


## Six and a Half Weeks, Start to Finish


From the first commit to an empty` ios/` folder on March 7 to the App Store on April 21: six and a half weeks, solo, in a language he had never written before.


If a solo developer can ship a **$2K MRR** local-AI app to the App Store in six weeks on RunAnywhere SDK, "is on-device AI ready for production?" stops being a real question.


---


## Building Something Local-First?


Prebuilt llama.cpp with Metal, ONNX embeddings, model registry, streaming, vision. One SDK, one init call.


- [Read the docs](https://docs.runanywhere.ai/)
- [runanywhere-sdks on GitHub](https://github.com/RunanywhereAI/runanywhere-sdks)


**LLM Hub links:**


- [LLM Hub on the App Store](https://apps.apple.com/us/app/llm-hub/id6762511820)
- [Source on GitHub](https://github.com/timmyy123/LLM-Hub)


---


*If you've shipped something with RunAnywhere SDK,[get in touch](https://www.runanywhere.ai/) . We'd love to write the next one of these about you.*
