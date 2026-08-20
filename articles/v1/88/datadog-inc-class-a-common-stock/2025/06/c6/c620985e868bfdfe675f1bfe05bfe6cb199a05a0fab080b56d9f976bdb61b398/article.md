---
schema_version: "1.0.0"
document_id: "c620985e868bfdfe675f1bfe05bfe6cb199a05a0fab080b56d9f976bdb61b398"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/noise-suppression-library/"
published_at: "2025-06-24T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:78ab4f3056e2a9dc194236eceb3e8ccaffb521b47e8b513cb3f2c6a100e96290"
---

# How we built a real-time, client-side noise suppression library without server dependencies

Jason Thomas


Senior Staff Engineer


Brandon West


Rosa Trieu


A few years ago, we had to scramble to find a quiet place for meetings to avoid the inevitable interruptions—leaf blowers, barking dogs, and kids shouting in the background. Today, thanks to the pervasiveness of both remote work and mobile devices, we have tools to help. With some clever algorithms and modern AI-powered processing, it’s possible to filter out much of that noise in real time.


The Datadog[CoScreen](https://www.datadoghq.com/product/coscreen/) team wanted to provide that experience in our real-time collaboration tool for remote and hybrid engineering teams. But when Senior Staff Engineer Jason Thomas set out to add real-time noise suppression to CoScreen, he quickly discovered a problem: no off-the-shelf libraries or tools were simultaneously performant enough to run in real time, portable enough to run on client devices, and easy to integrate with[WebRTC](https://webrtc.org/) —the open source real-time communication software supported by Google, Mozilla, and Opera.


So, he built one. Our solution,[dtln-rs](https://github.com/DataDog/dtln-rs) , is an open source noise suppression library that’s embeddable across native clients and the web. It’s optimized for speed—on a standard M1 MacBook Pro, it processes **one second of audio in just 33 ms** , well within the real-time threshold.


In this post, we explain how we implemented deep learning–based noise reduction for CoScreen, optimized it to run efficiently without server-side dependencies, wrapped it in a portable Rust library with targets for[WebAssembly](https://webassembly.org/) , Node.js, and native clients, and open-sourced it so you can add high-quality noise suppression to your own WebRTC app.


## Introducing dtln-rs: A lightweight, open source noise suppression library


Today,[we’re open-sourcing dtln-rs](https://github.com/DataDog/dtln-rs) , a lightweight and portable noise reduction library built on Dual-Signal Transformation LSTM Network ([DTLN](https://www.isca-speech.org/archive/interspeech_2020/westhausen20_interspeech.html) ). Dtln-rs is a small Rust project that can produce a WebAssembly module, a native Rust target library, and a Node.js native module to embed in your clients and interface with WebRTC. We hope it will help developers and companies using open source audio and video solutions easily add real-time noise reduction to their projects.


We are also publishing a[demo application](https://github.com/DataDog/dtln-rs-demo) that shows how to embed the standalone noise filter in an application or web page.


## Try out the demo


Dtln-rs was built out of a need we experienced first-hand. One of our engineers, Brad, had a neighbor who would often mow their lawn in the middle of our team meetings. It became a regular disruption—but behind the scenes, we were already experimenting with a next-generation noise reduction filter.


One day, when Brad enabled this experimental filter in CoScreen and mentioned that the lawn mower was running again, our colleague Jesse asked, “Really? What are you talking about?”


The noise reduction had worked: it had completely removed the background noise from the lawn mower. They immediately stopped to make a comparison recording—one capturing what Brad heard on his side:


And another capturing what Jesse heard after noise reduction was applied:


This was the moment we knew this little embedded library worked well enough to deliver real value to our customers.


But you don’t have to take our word for it—[try it out yourself](https://github.com/DataDog/dtln-rs-demo) .


## How DTLN enables real-time noise suppression


Broadly speaking, AI-based noise reduction works by analyzing the data it’s fed and learning to distinguish between the signal (the stuff we want to hear or see) and the noise (the stuff we don’t). Think of it as teaching a computer to listen like a human—to intuitively understand which sounds are important and which can be filtered out.


Neural networks are trained using vast datasets, learning from thousands of examples what noise looks like and how to remove it without sacrificing the quality of the signal. Multiple strategies are at work in AI-based noise reduction, most of which require significant computing resources. Many are designed for offline or non-real-time processing, or to be run on powerful computers. DTLN stands out as an efficient and versatile approach—one that enables real-time noise suppression for a broader set of use cases.


DTLN uses a[short-time Fourier transform (STFT)](https://en.wikipedia.org/wiki/Short-time_Fourier_transform) to break down sounds into smaller chunks, analyze the volume of each frequency in each chunk, and provide a magnitude spectrum. Sounds also have **phase information** , which represents the starting point of each frequency in the wave. The spectrum and phase information are used as parameters in a model that identifies which parts of the sound are likely speech and which are likely noise.


The combination of real-time processing and deep learning that comes from analyzing both the magnitude and phase components of sound enables DTLN to provide near-instantaneous noise reduction—making it well-suited for real-time applications such as live streaming, online gaming, and virtual meetings. The model’s efficiency lies not only in its computational design, but also in its adaptability to different noise environments. It can dynamically adjust its filtering based on the type of noise present, whether it’s the hum of an air conditioner, chatter in a cafe, or rustling of paper.


With its innovative use of STFT, phase information, and a[long short-term memory](https://en.wikipedia.org/wiki/Long_short-term_memory) (LSTM) neural network, DTLN shows how AI can solve complex real-time audio challenges.


## Why off-the-shelf noise suppression wasn’t enough


For people making audiovisual (AV) products these days, the really difficult part of implementing state-of-the-art noise reduction is that most machine learning models need to run on beefy backend hardware, and their results are sent back over a network.


[WebRTC](https://webrtc.org/) is a widely used, open source, and free technology for building video and audio conferencing systems. It backs[Google Meet](https://workspaceupdates.googleblog.com/2021/06/background-noise-reduction-cancellation-update.html) —which handles an extremely large volume of video conferencing traffic—and serves as the foundation for much of the private video conferencing tech outside of Zoom, including CoScreen.


Despite its widespread adoption, WebRTC hasn’t caught up to the state of the art in AV technology and still relies on out-of-the-box, last-generation noise reduction solutions.


There has been some work using last-generation AI-based noise reduction systems such as[RNNoise](https://jmvalin.ca/demo/rnnoise/) , which definitely reduce noise—but not nearly as effectively as the modern solutions deployed in collaborative tools by larger companies. Thankfully, due to the flexible nature of WebRTC, the Web Audio API, and WebAssembly, making real-time custom solutions is possible, as demonstrated by the Jitsi Project with their[RNNoise-based filter](https://github.com/jitsi/rnnoise-wasm) .


Adding another server component introduces additional latency, complicates hardware requirements, and increases costs. This has resulted in a host of pay-to-play solutions with prohibitive pricing, leaving most teams working on small WebRTC products grasping at straws when trying to deliver better audio quality to their users.


Google spent years training its models on vast datasets of noisy and clean speech, developing a deep neural network (DNN) capable of distinguishing between clean speech and noise. They then dispatched this model to an array of specialized servers calibrated to process audio signals, decode them in real time, remove noise, and re-encode them for transport. None of this is part of WebRTC or open source—and a small startup like CoScreen wasn’t in a position to build and deploy such sophisticated noise reduction models.


As our customers continued to tell us that our audio quality did not match what they had come to expect from competitors in noisy environments, we started investigating alternative approaches—all of which were either cost-prohibitive or too time-consuming to implement.


Eventually, we encountered a research project aimed at producing a model that could run acceptably, in real-time, and on standard hardware—leaving us with the work of embedding, optimizing, and deploying it alongside our application.


## Building on top of the DTLN project


DTLN is a noise suppression method and implementation that originated from the Communications Acoustics group at the University of Oldenburg. The group produced an MIT-licensed open source[TensorFlow](https://www.tensorflow.org/) implementation—TensorFlow being a popular library for creating and using machine learning models—of their real-time noise suppression method, which competed in Microsoft’s[Deep Noise Suppression (DNS) Challenge](https://github.com/microsoft/DNS-Challenge) in 2020. They placed a respectable eighth out of 17 teams—including many commercial players—in the real-time track. Their achieved[MOS](https://en.wikipedia.org/wiki/Mean_opinion_score) scores—a common way of rating video and audio collaboration quality—were above the state-of-the-art average, and compare favorably to commercially deployed noise suppression systems.


Unfortunately, the model was implemented as a full TensorFlow project, and we had no indication whether it could be optimized enough to run in real time inside CoScreen. So, we kicked off a project to interface the model with a small native library and optimized it until it could perform acceptably within CoScreen. We figured if huge LLMs could be optimized and embedded to work in near-real-time by projects like[llama.cpp](https://github.com/ggerganov/llama.cpp) , there was hope for us doing the same with DTLN.


TensorFlow turns out to be fairly easy to optimize and embed using[TensorFlow Lite](https://www.tensorflow.org/lite) —now known as[LiteRT](https://developers.googleblog.com/en/tensorflow-lite-is-now-litert/) —a robust set of tools for building embeddable models and deploying them to small devices. To test this out, we built an implementation that used TensorFlow Lite and Rust to create a fully embedded Node.js module that could inline-denoise web audio. On a standard M1 MacBook Pro, it can denoise one second of audio in just 33 ms, easily meeting real-time requirements.


We currently employ a CPU-optimized version of TensorFlow Lite, but Apple also provides a Metal-based[plugin](https://developer.apple.com/metal/tensorflow-plugin/) for GPU support. Our Rust implementation can target[Emscripten](https://emscripten.org/) -backed WebAssembly as well, which means it can be easily introduced into any WebRTC-based web client. As a result, **we can have a completely client-embedded noise suppression solution** —deployed alongside our application, without per-minute audio filtering charges or reliance on any third-party provider.


The following diagram shows the possible paths from audio input to the embedded noise suppression module, depending on client type.


## Building a cross-platform noise suppression library with Rust and TensorFlow Lite


To deliver noise reduction to our clients without relying on complicated server components, **we needed a solution that could deploy to both web and native clients** . Python is widely used for server-side work and even used in some client applications, but it’s awkward to fit into a web environment and far from minimal. The minimum[installation size](https://www.tensorflow.org/install/pip) for TensorFlow is approximately 1.1 GB—larger than the entire CoScreen application. All of the examples provided by DTLN were focused on processing static audio clips on the server.


Meanwhile, TensorFlow Lite is designed to be small and lightweight, making it a good fit for minimal client-side projects. It also has a WebAssembly target, meaning we could optimize and deploy noise cancellation directly to our web client users. WebAssembly allows developers to build high-performance web applications using languages like Rust, broadening the types of apps that can be effectively run in the browser. Although there are Rust crates for wrapping TensorFlow, we decided to build and wrap our own interface using a reduced C API for maximum control.


From the start, it wasn’t clear that this would succeed, but we hoped to use Rust’s cross-compiler support and easy-to-configure build system to write a single, small library that could target all our environments. Rust’s build system, Cargo, manages dependencies, compiles packages, and builds executables or libraries while handling cross-platform toolchains. The` build.rs` script in Rust projects allows developers to write custom build logic that executes before the main build process, such as generating code or compiling non-Rust components.


If we had used C++, most of this logic would have required complex and ad hoc build systems like Make or CMake. **Rust’s standardized tooling dramatically improved our ability to quickly iterate, test, and release.** For Node.js integration, we standardized on using[NEON](https://github.com/neon-bindings/neon) , a mature solution for binding Rust to JavaScript. In summary, **Rust gave us a robust, production-ready way to target every platform we needed** .


Experimenting with and bringing in high-quality third-party packages was also incredibly easy in Rust, which helped boost our productivity enormously. At its core, **our library is a simple set of functions** : it takes pulse-code modulation (PCM) audio data from a source, performs a few small transformations to match the DTLN model’s input expectations, runs the model, collects the output, and forwards it back out as denoised PCM audio samples. Most of the additional work involved building useful examples for testing and validating the model—something Rust’s ecosystem of third-party packages made easy, with off-the-shelf libraries for resampling audio and producing[WAV](https://en.wikipedia.org/wiki/WAV) files.


Rust supports several WebAssembly backends:[wasm-bindgen](https://github.com/rustwasm/wasm-bindgen) ,[wasm-pack](https://github.com/rustwasm/wasm-pack) , and Emscripten. When we started,` wasm-pack` wasn’t yet widely adopted. Although` wasm-bindgen` is a more elegantly supported solution in the Rust ecosystem, Emscripten was the oldest, most complete, and best documented option. Since TensorFlow already had an Emscripten target, it was a clear path forward. Emscripten isn’t just for Rust—it can port code from any POSIX-compatible platform to WebAssembly by simulating a full system environment. The tradeoff to providing this extensive, POSIX-compatible set of libraries—and simulating the same POSIX environment you might find on a system like Linux—is that the **resulting binaries can be quite large** .


We experimented with several Rust crates for binding TensorFlow, but the C wrapper ultimately proved to be the simplest and most reliable interface to TensorFlow Lite. The most immediate value Rust gave us was its combination of a rich ecosystem and excellent build tooling. Our hybrid approach let us leverage what we knew—interfacing C with TensorFlow—while using Rust’s strengths for managing builds and cross-platform deployment.


Our noise reduction process, shown in the following diagram, is simple: We split incoming audio into magnitude and phase components, apply the first DTLN model, run an inverse FFT, apply the second DTLN model, and output the denoise result. The interactions with the DTLN TensorFlow models happen through a thin C interface.


In the end, we were able to produce a small Node.js module using NEON, a lightweight WebAssembly library, and a C-ABI-compatible dynamic library—each wrapping the same noise reduction core. This allowed us to deploy noise suppression in a fully native and WebAssembly-based form across all our clients.


## Using WebAssembly for browser embedding


CoScreen is an Electron application, which offers significant flexibility compared to web-based apps—especially when it comes to integrating our noise reduction module. Initially, we focused on building a N-API-wrapped Node.js module, leveraging native code to access low-level system resources for performance advantages, such as using Apple’s ML acceleration or GPU-based processing.


However, due to some limitations in Electron, we required a different approach. Specifically, we had to load the module either in a WebWorker or use` ScriptProcessorNode` , the latter of which did not offer reliable performance. The most stable solution turned out to be using a WebWorker, but Electron’s documentation strongly advises against using native Node.js modules within a WebWorker.


This led us to consider WebAssembly as the most viable option. The key advantage of WebAssembly is that the same implementation used and tested for our desktop application can also be deployed for our web application. The tradeoff is that we lose access to some platform-specific optimizations that could improve performance. Although WebAssembly isn’t as performant as native code and offers fewer optimization opportunities, the benefits of consistency and ease of integration across platforms outweighed those drawbacks. We still provide a native library and a N-API target for those who might make a different choice for their own projects.


To integrate the Rust WebAssembly module into our application using HTML5 standards, we employed the` AudioWorkletProcessor` interface to create what we called a **NoiseSuppressionWorker** . This worker directly processes audio data to suppress noise. When attached to an audio source, it receives audio samples in blocks, feeds them into the underlying WebAssembly module, and replaces those chunks with noise-reduced samples. AudioWorklets operate in their own isolated process and have a priority level suitable for processor-intensive tasks like noise reduction.


The second crucial component is the **NoiseSuppressionModule** , responsible for initializing the` AudioWorkletProcessor` and loading the WebAssembly module. To deliver the WebAssembly module, we compile and bundle it into a single` .js` file that the NoiseSuppressionModule can load into the underlying` AudioWorkletProcessor` .


Refer to the[dtln-rs documentation](https://github.com/DataDog/dtln-rs/blob/main/README.md) for installation and build steps. This project serves as a robust foundation for further audio processing enhancements in both desktop and web applications.


## Bringing next-generation noise suppression to the open source community


The state of the art in video conferencing technology has moved leaps and bounds since the pandemic. All major commercial platforms have now incorporated next-generation noise reduction—but these capabilities generally aren’t accessible to the open source community. A simple, effective, next-generation noise suppression technique exists through DTLN, but it’s not well documented or easy to embed in WebRTC projects.


The **dtln-rs** open source library is our attempt to bridge this gap and provide the open source community with an opportunity to experiment with and embed this type of next-generation noise reduction technology into their own projects.


Want to work on projects like this?[We’re hiring!](https://careers.datadoghq.com/all-jobs/?s=open%20source)


-
-
-
