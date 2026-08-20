---
schema_version: "1.0.0"
document_id: "d7c7459ced3619a8478de5319d9168981e72c46b4a4ba1672c2b0a0fda132c19"
company_key: "yc-runanywhere"
company: "RunAnywhere"
source_id: "yc-runanywhere-news-import-450c102900e1"
canonical_url: "https://www.runanywhere.ai/blog/runanywhere-sdk-powers-on-device-ai-coaching-picklerate"
published_at: "2026-03-13T00:00:00+00:00"
first_seen_at: "2026-07-24T00:25:59.131595+00:00"
fetched_at: "2026-07-28T22:00:56.367665+00:00"
content_hash: "sha256:0989d1a90dbad936ae582a6aac8a77ca0c6fff6d074a0e3912cd3f0a66fb22eb"
---

# How RunAnywhere SDK Powers On-Device AI Coaching in PickleRite

Pickleball is the fastest-growing sport in the US. But improving at it usually requires one thing most players don't have access to: a coach who watches every session.


[PickleRite](https://picklerite.com/pickle-rite) is a performance tracker that closes that gap — and the engine behind its AI coaching is the **RunAnywhere SDK** , which lets it run a specialized language model directly on-device, on every iPhone, with zero network dependency.


This case study walks through exactly how PickleRite integrated RunAnywhere, why on-device inference outperforms cloud APIs for this use case, and what the developer experience looks like end-to-end.


PickleRite Dashboard — track sessions, mistakes, accuracy, and improvement over time


> **About the app:**[PickleRite](https://picklerite.com/pickle-rite) is a free iOS app with a 4.9-star rating on the[App Store](https://apps.apple.com/us/app/pickle-rite/id6754095362) . It tracks "Pickles" (errors) to help players systematically improve through data-driven insights and AI-powered coaching. Available on iPhone and Apple Watch.


> **About the author:**[Badarinath Venkatnarayansetty](https://www.linkedin.com/in/baaborig/) is a Senior Staff Software Engineer at Intuit with 15+ years of experience building mobile apps. He's an active open-source contributor (CardParts, StepperView, StackCardView) and writes about iOS development, AI integration, and mobile engineering on[his Substack](https://badarinath.substack.com/) . This article was originally published on[Badarinath's Substack](https://badarinath.substack.com/p/how-runanywhere-sdk-powers-on-device) .


---


## What Is RunAnywhere?


RunAnywhere is an SDK for Apple platforms that enables local LLM inference on iPhone, iPad, and Mac. It supports multiple inference backends — including` LlamaCPP` (for GGUF models with Metal GPU acceleration),` ONNX` , and` WhisperKit` — and exposes a clean Swift API that integrates with Swift Concurrency.


The package breakdown in PickleRite:


text


```text
1  runanywhere-sdks    2  ├── RunAnywhere          — core SDK, API surface    3  ├── RunAnywhereLlamaCPP  — GGUF model inference via llama.cpp + Metal    4  ├── RunAnywhereONNX      — ONNX model runtime    5  └── RunAnywhereWhisperKit — Whisper-based speech recognition
```


For PickleRite, the integration uses` RunAnywhere` +` RunAnywhereLlamaCPP` . The LlamaCPP backend leverages the iPhone's Metal GPU for hardware-accelerated inference on quantized GGUF models — meaning even a 350M-parameter model runs fast enough for real-time coaching feedback.


---


## The Model: LiquidAI LFM2 350M


The model powering PickleRite's AI coaching is **LiquidAI's LFM2 350M** — a compact, instruction-tuned model hosted on HuggingFace.


**Why this model?**


Factor Detail


**Size** 250 MB on disk


**Memory** Fits comfortably in device RAM


**Load time** Seconds, not minutes


**Specialization** Instruction-following with domain-specific prompts


For a sports coaching app, you don't need GPT-4-scale reasoning — you need fast, focused, domain-specific output. LFM2 350M delivers exactly that when combined with well-engineered prompts.


---


## SDK Setup: Initialization and Model Registration


swift


```text
1  import     RunAnywhere     2
3   func     initializeRunAnywhere  (  )     {     4        do     {     5            let   config   =     Config  .  loadConfig  (  )  .  runAnywhere    6
7            // Initialize the SDK with your API key and environment     8            try     RunAnywhere  .  initialize  (     9              apiKey  :   config  .  apiKey  ,     10              baseURL  :   config  .  baseURL  ,     11              environment  :     .  production    12            )     13
14            // Register the LlamaCPP backend (enables Metal GPU acceleration)     15            LlamaCPP  .  register  (  )     16
17            // Register the specific model we want to use     18            if     let   modelURL   =     URL  (  string  :   config  .  modelURL  )     {     19                RunAnywhere  .  registerModel  (     20                  id  :   config  .  modelId  ,     21                  name  :   config  .  modelName  ,     22                  url  :   modelURL  ,     23                  framework  :     .  llamaCpp  ,     24                  memoryRequirement  :     Int64  (  config  .  memoryRequirement  )     25                )     26            }     27        }     catch     {     28            print  (  "RunAnywhere initialization failed"  )     29        }     30   }
```


The` memoryRequirement` parameter lets RunAnywhere make informed decisions about whether the device can safely load the model — a critical safety valve for memory-constrained devices.


---


## Model Download and Loading with Progress Streaming


The model GGUF file is downloaded once and cached on-device. On subsequent launches, RunAnywhere serves it from the local cache without re-downloading. The loading flow streams download progress, making it trivial to build a loading UI:


swift


```text
1  func     loadModel  (  )     {     2        let   modelId   =     Config  .  loadConfig  (  )  .  runAnywhere  .  modelId    3        Task     {     4            do     {     5                // Attempt to load from local cache first     6                try     await     RunAnywhere  .  loadModel  (  modelId  )     7                print  (  "Model loaded from cache"  )     8            }     catch     {     9                // Not cached — download the GGUF from HuggingFace     10                let   progressStream   =     try     await     RunAnywhere  .  downloadModel  (  modelId  )     11                for     await   progress   in   progressStream   {     12                    print  (  "Download:   \(  Int  (  progress  .  overallProgress   *     100  )  )  %"  )     13                    if   progress  .  stage   ==     .  completed   {     break     }     14                }     15                // Load into memory after download     16                try     await     RunAnywhere  .  loadModel  (  modelId  )     17                print  (  "Model downloaded and loaded"  )     18            }     19        }     20   }
```


The` AsyncSequence` -based progress stream fits naturally into Swift Concurrency. You can pipe` progress.overallProgress` directly into a` @Published` property to drive a progress bar in SwiftUI — no callbacks, no delegates.


---


## Generating Coaching Reports with Streaming Inference


The core of the RunAnywhere integration is` RunAnywhere.generateStream()` — an async streaming API that yields tokens as they're generated, enabling typewriter-effect UI without any extra work.


### Full Coaching Summary (RiteAI Tab)


In` AIAnalysisAction.swift` , PickleRite generates a complete structured coaching report from the player's session data:


swift


```text
1  @MainActor     2   private     func     executeWithRunAnywhere  (  appConfig  :     AppConfig  )     async     ->     AISummary  ?     {     3        do     {     4            let   fullPrompt   =   summaryConfig  .  instructions   +     "\n\n"     +     buildPrompt  (  )     5
6            var   accumulated   =     ""     7            let   result   =     try     await     RunAnywhere  .  generateStream  (     8              fullPrompt  ,     9              options  :     LLMGenerationOptions  (  maxTokens  :     600  )     10            )     11
12            // Stream tokens as they arrive     13            for     try     await   token   in   result  .  stream   {     14              accumulated   +=   token    15            }     16
17            // Parse JSON from the accumulated response     18            return     decodeJSON  (  from  :   accumulated  )     19        }     catch     {     20            print  (  "RunAnywhere stream error:   \(  error  )  "  )     21            return     nil     22        }     23   }
```


The 600-token budget is enough for a rich coaching report covering overall insight, error analysis, 2–3 drill recommendations, and a motivational closing. The streaming approach means the user sees the response forming in real-time rather than waiting for a full round-trip.


PickleRite AI-Powered Insights — AI Analysis, Focus Areas, and Drill Recommendations generated on-device by RunAnywhere


### Focus Messages (Analytics Tab)


For the Analytics tab, shorter burst messages (under 15 words each) are generated per-error-type with a tighter token budget:


swift


```text
1  private     static     func     generateWithRunAnywhere  (  ...  )     async     ->     String     {     2        let   result   =     try     await     RunAnywhere  .  generateStream  (     3          fullPrompt  ,     4          options  :     LLMGenerationOptions  (  maxTokens  :     100  )     5        )     6        var   message   =     ""     7        for     try     await   token   in   result  .  stream   {     8          message   +=   token    9        }     10        return   message    11   }
```


---


## Prompt Engineering: Structured JSON Output


Apple's Foundation Models framework has the luxury of` @Generable` — a Swift macro that generates JSON schema from struct definitions and enforces type-safe structured output at the model level. RunAnywhere doesn't have that (yet), so PickleRite uses **prompt-enforced JSON structure** with a custom decoder.


### The Prompt


yaml


```text
1  instructions  :     |     2    You are a pickleball coach. Analyze session data and respond    3    with ONLY a JSON object.    4    Do not write any text before or after the JSON.    5    Do not use markdown. Do not explain yourself.    6    Output must start with { and end with }.     7
8   prompt  :     |     9    Session data:    10    {sessionContext}    11    Respond with this exact JSON structure:    12    {    13      "overallInsight": "2-3 encouraging sentences about performance.",    14      "errorAnalysis": "2-3 sentences identifying top errors...",    15      "recommendations": [    16        {    17          "title": "Drill name",    18          "description": "How to do the drill and what it targets."    19        }    20      ],    21      "motivationalClosing": "One encouraging sentence."    22    }
```


Putting the exact JSON template directly in the prompt works reliably with LFM2. The model fills in the values without modifying the structure.


---


## Dual-Provider Architecture: RunAnywhere + Cloud


One of the most interesting parts of this implementation is the dual-provider model picker. Users can switch between:


- **RunAnywhere (on-device)** — free, private, offline-capable, domain-specific with LFM2 350M
- **Cloud API** — for users who want larger model reasoning via Apple Foundation Models


This architecture also makes A/B testing trivial — you can toggle the provider in` config.yml` and ship a build to compare output quality across a segment of users.


---


## Why RunAnywhere Wins for a Domain-Specific Sports App


### 1. Any Model, Any Architecture


RunAnywhere is model-agnostic. You point it at a GGUF URL, register it, and call` generateStream` . That means as better small models emerge — whether from LiquidAI, Mistral, Meta, or the community — you swap the URL in` config.yml` and ship. No SDK updates, no API migrations.


### 2. Works on iOS 18+ (Not iOS 26+)


Apple's Foundation Models framework requires iOS 26 and Apple Intelligence hardware. That's a hard cutoff that excludes a significant portion of devices today. RunAnywhere works on **iOS 18+** — the model is downloaded once and runs via Metal, with no OS-level Apple Intelligence dependency. Your whole user base gets AI, not just early adopters.


### 3. Streaming is First-Class


` RunAnywhere.generateStream()` returns an` AsyncThrowingStream` of tokens. It drops into Swift Concurrency's` for try await` loop with zero boilerplate — the same idiom used throughout the app. Streaming feedback appears token-by-token, making the coaching report feel alive and responsive.


### 4. Cost Is Zero Per Inference


Unlike cloud LLM APIs — where every coaching report is a billable token call — RunAnywhere inference runs entirely on the user's device. For a consumer app where engaged players might generate coaching reports after every session, this is the difference between a sustainable business and a cost structure that scales against you.


### 5. Offline by Default


PickleRite players are on courts, in gyms, at outdoor facilities. Network reliability is unpredictable. After the model downloads once on a good connection, every subsequent coaching analysis works offline — no spinner, no "check your connection" error, no degraded experience.


### 6. Privacy Without a Privacy Policy Footnote


Session data — mistake counts, playing patterns, improvement trajectories — stays on the device. It never leaves. There's no data retention policy to write, no GDPR or CCPA compliance burden for AI inference, and no user trust to earn around "we send your data to an AI provider."


---


## Getting Started with RunAnywhere


If you're building a domain-specific iOS app and want to integrate on-device LLM inference, the RunAnywhere SDK is a low-friction path. The three key calls you need:


swift


```text
1  // 1. Initialize     2   try     RunAnywhere  .  initialize  (  apiKey  :  ,   baseURL  :  ,   environment  :  )     3   LlamaCPP  .  register  (  )     4
5   // 2. Register your model (any GGUF from HuggingFace or your CDN)     6   RunAnywhere  .  registerModel  (     7      id  :  ,   name  :  ,   url  :  ,     8      framework  :     .  llamaCpp  ,     9      memoryRequirement  :     10   )     11
12   // 3. Load and generate     13   try     await     RunAnywhere  .  loadModel  (  modelId  )     14   let   stream   =     try     await     RunAnywhere  .  generateStream  (     15      prompt  ,     16      options  :     LLMGenerationOptions  (  maxTokens  :     200  )     17   )     18   for     try     await   token   in   stream  .  stream   {     /* render token */     }
```


Check out our[Swift SDK documentation](https://www.runanywhere.ai/blog/runanywhere-swift-part1-chat) to get started.


---


## Conclusion


RunAnywhere SDK gave PickleRite something no cloud AI provider could: **an AI coach that runs everywhere, costs nothing per inference, and keeps your data yours** . By pairing RunAnywhere with LiquidAI's LFM2 350M — a small, quantized model tuned for instruction following — PickleRite delivers coaching output that is fast, domain-specific, and available offline to every player regardless of iOS version.


For iOS developers building apps where domain expertise matters more than raw model scale, RunAnywhere is worth serious attention. You choose the model, you own the inference, and your users get AI that feels native — because it is.


**[PickleRite](https://apps.apple.com/us/app/pickle-rite/id6754095362) is available on the App Store.** Learn more at[picklerite.com](https://picklerite.com/pickle-rite) . Built with SwiftUI, SwiftData, RunAnywhere SDK, and a lot of time on the court.


---


*This article was originally published by[Badarinath Venkatnarayansetty](https://badarinath.substack.com/p/how-runanywhere-sdk-powers-on-device) on his Substack.*
