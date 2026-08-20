---
schema_version: "1.0.0"
document_id: "c3f82e3746da298e8667ef27cf1e8de6970b175f5e421386b99e333d288b99ab"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/on-device-ai-browser/"
published_at: "2026-04-25T00:00:00+00:00"
first_seen_at: "2026-07-25T17:38:53.548823+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:0b718e5753fe839e2b2ae2c3b9c39ea90103fe96da184dbd4a87dd02e4650e9e"
---

# The State of On-Device AI in the Browser

Running AI directly in the browser sounds straightforward until you try to ship it. The APIs are fragmented, hardware requirements vary wildly, and what works on one device silently fails on another. Before you wire up your first inference call, it helps to understand what’s actually available, how the layers fit together, and where the real gaps are in early 2026.


## Key Takeaways


- On-device browser AI spans three distinct layers: built-in browser APIs, JavaScript inference libraries, and low-level acceleration primitives. Choosing the wrong layer for your use case leads to compatibility and performance problems.
- Chrome’s built-in AI APIs, including Summarizer, Translator, and Language Detector, require no model hosting but tie you to Chrome’s implementation and a model you don’t control.
- Transformers.js and ONNX Runtime Web offer broad browser-based model inference with full model choice, though model size, backend support, and caching strategy remain key constraints.
- WebNN promises hardware-accelerated ML with NPU access, but browser support is still partial. Most teams will benefit from it indirectly through frameworks before using it directly.
- A hybrid approach, attempting local inference first and falling back to a cloud endpoint, is the most realistic production pattern today.


## Three Distinct Layers, Not One Thing


The biggest source of confusion in on-device browser AI is treating all approaches as interchangeable. They’re not. There are three distinct layers, and picking the wrong one for your use case creates real problems.


### Layer 1: Browser-Provided AI APIs


Chrome ships built-in AI APIs backed by models it provides and manages directly in the browser, including Gemini Nano. As described in the[Chrome built-in AI docs](https://developer.chrome.com/docs/ai/built-in) , these models are downloaded and handled by Chrome itself. Chrome has made APIs such as **Summarizer** , **Translator** , and **Language Detector** available in stable versions, while others remain more limited. The Prompt API is stable for Chrome extensions, but web-page usage is still experimental or origin-trial based. Writer and Rewriter are also not something you should treat as universally production-ready.


Microsoft Edge takes a similar approach using Phi-4-mini and exposes its own API surface. The model is built directly into the browser and can be accessed through APIs like the[Prompt API](https://learn.microsoft.com/en-us/microsoft-edge/web-platform/prompt-api) , which is currently available in developer preview in Edge Canary and Dev builds. However, these APIs are still experimental and not broadly available in production environments. Firefox has AI features such as chatbot integration and Smart Window experiments, but it does not currently expose a Chrome-style built-in AI API surface for web developers.


The appeal is obvious: no model hosting, no bundle size cost, minimal setup. The catch is equally obvious: you’re tied to a specific browser implementation, the model is fixed, and you have no control over what version is running. These APIs also require the model to be downloaded first, which can be large and happens asynchronously. You need to handle that gracefully.


```text
// Feature-detect before using a built-in browser AI API
if (  '  Summarizer  '   in   self  ) {
const   availability   =   await   Summarizer  .  availability  ();


if (  availability   !==   '  unavailable  '  ) {
const   summarizer   =   await   Summarizer  .  create  ();
const   summary   =   await   summarizer  .  summarize  (  articleText  );
}
} else {
// Fall back to cloud or skip the feature
}
```


### Layer 2: JavaScript-Based Inference with Transformers.js and ONNX Runtime Web


If you need broader browser support or want to choose your own model,[Transformers.js](https://huggingface.co/docs/transformers.js/en/index) is one of the most practical options right now. It runs Hugging Face models directly in the browser using ONNX format and can use WebGPU acceleration when available, falling back to WebAssembly where supported.


[ONNX Runtime Web](https://onnxruntime.ai/docs/get-started/with-javascript/web.html) gives you similar reach with more control over execution providers. Both are reasonable choices for classification, translation, sentiment analysis, embeddings, and lightweight text generation tasks.


Note that Transformers.js v3 moved to the` @huggingface/transformers` package. The` @xenova/transformers` import shown below applies to v2, which remains common in existing projects:


```text
// Transformers.js v2
import   {   pipeline   }   from   '  @xenova/transformers  '  ;


// Transformers.js v3+
// import { pipeline } from '@huggingface/transformers';


const   classifier   =   await   pipeline  (  '  sentiment-analysis  '  );
const   result   =   await   classifier  (  '  This article is genuinely useful.  '  );
```


Model size is the main constraint. A quantized model suitable for browser inference may range from tens to hundreds of megabytes, depending on the task. Larger models become impractical without careful caching via IndexedDB or the Cache API.


### Layer 3: WebGPU and WebAssembly as Acceleration Primitives


[WebGPU](https://developer.mozilla.org/en-US/docs/Web/API/WebGPU_API) and[WebAssembly](https://developer.mozilla.org/en-US/docs/WebAssembly) are not AI APIs. They’re low-level primitives that frameworks like Transformers.js, ONNX Runtime Web, and TensorFlow.js can use internally to run inference faster. You rarely interact with them directly unless you’re building a framework or doing custom compute work.


WebGPU in particular unlocks meaningful GPU acceleration for matrix operations, which matters for anything beyond tiny models. Support is much broader than it used to be, but it still needs feature detection because browser, operating system, device, driver, and mobile support vary.


## What WebNN Adds to the Picture


[WebNN](https://www.w3.org/TR/webnn/) (Web Neural Network API) is a W3C API designed to expose hardware-accelerated neural network operations, including NPU access on supported devices, through a consistent browser interface. It sits between your framework and the hardware, much like WebGPU, but is specifically designed for ML workloads.


Browser support is limited in early 2026. Chrome has partial implementation, and broader support across other browsers is still in progress. Frameworks like ONNX Runtime Web are already adding WebNN as an execution backend, so you’ll likely benefit from it indirectly before you use it directly.


## The Honest Tradeoffs


Approach Browser Support Model Control Setup Cost Best For


Built-in APIs Chrome stable APIs; Edge previews None Minimal Summarization, translation, detection


Transformers.js Broad modern-browser support Full Medium Cross-browser NLP, classification


ONNX Runtime Web Broad modern-browser support Full Medium Custom models, multi-task inference


WebNN (direct) Partial Full High Future hardware acceleration


Privacy benefits are real but conditional. Local inference means input data doesn’t leave the device during processing, but the website can still log what users type before it reaches the model. “Local” doesn’t automatically mean private end-to-end.


Offline capability is similarly conditional. Once a model is cached, inference can work without a connection. But the initial download requires one, and model updates require reconnection.


## Hybrid Is the Realistic Default


Most production applications won’t go fully on-device. The practical pattern is to attempt local inference, check for API availability and hardware capability, then fall back to a cloud endpoint when either is missing. This gives capable devices a faster, more private experience without breaking the feature for everyone else.


## Conclusion


On-device AI in the browser is genuinely useful today for specific, bounded tasks: summarizing a document, detecting a language, classifying short text, generating embeddings, or running a lightweight assistant. Full LLM-scale experiences in the browser remain inconsistent and hardware-dependent. Build for the realistic middle ground, and you’ll ship something that actually works.


## FAQs


Can I use on-device browser AI in Firefox or Safari?


Not through built-in AI APIs comparable to Chrome's current APIs. However, JavaScript inference libraries like Transformers.js and ONNX Runtime Web can run across modern browsers, usually with WebAssembly fallback when WebGPU or other acceleration backends are unavailable.


How large are the models I need to download for browser-based inference?


Chrome's built-in models are managed by the browser and may require a significant one-time download. For libraries like Transformers.js, quantized models often range from tens to hundreds of megabytes, depending on the task and model. Caching them with IndexedDB or the Cache API avoids repeated downloads, but the first load still requires a network connection.


Is on-device AI truly private if it runs in the browser?


Input data can stay on the device during inference, which is a real privacy gain over cloud-based processing. However, the website's own JavaScript can still read, log, or transmit user input before or after it reaches the model. Local inference reduces exposure but does not guarantee end-to-end privacy on its own.


Should I use Chrome's built-in AI APIs or Transformers.js for my project?


If your audience is primarily Chrome desktop users and a fixed browser-managed model meets your needs, built-in APIs offer the simplest setup. If you need broader browser support, custom model selection, or predictable versioning, Transformers.js gives you more control at the cost of managing model downloads and caching yourself.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
