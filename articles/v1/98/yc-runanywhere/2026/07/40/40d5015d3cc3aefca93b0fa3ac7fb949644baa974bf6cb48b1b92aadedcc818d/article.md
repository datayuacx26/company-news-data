---
schema_version: "1.0.0"
document_id: "40d5015d3cc3aefca93b0fa3ac7fb949644baa974bf6cb48b1b92aadedcc818d"
company_key: "yc-runanywhere"
company: "RunAnywhere"
source_id: "yc-runanywhere-news-import-0a1b2b4506bd"
canonical_url: "https://www.runanywhere.ai/blog/runanywhere-sdk-v0-17-5-release"
published_at: null
first_seen_at: "2026-07-24T00:26:00.403075+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:f0399d4da5134778e10f55a2cb73b2aea316d52079bff97476198f4e02822632"
---

# RunAnywhere SDK v0.17.5: Cross-Platform On-Device AI

RunAnywhere released SDK v0.17.5, a cross-platform update that strengthens its on-device AI infrastructure across Swift (iOS/macOS), Kotlin (Android), Flutter, and React Native. The release unifies core runtime components and backend integrations, including llama.cpp for local LLM execution and ONNX-powered speech pipelines for voice activity detection, speech-to-text, and text-to-speech. The result is a more consistent, production-ready foundation for teams building chat, voice, and multimodal AI experiences directly on device.


Rather than focusing on incremental feature changes, v0.17.5 reinforces RunAnywhere's broader mission: make on-device AI the default way apps ship intelligence. By simplifying runtime alignment and backend packaging across platforms, this update reduces integration friction for mobile engineers, improves determinism for enterprise teams, and enables privacy-first, offline-capable AI experiences without the operational complexity typically associated with edge deployments.


## What's New in v0.17.5


### Unified Cross-Platform Runtime


This release brings consistency across all supported platforms:


- **Swift (iOS/macOS)** : Enhanced native integration with Core ML and Metal acceleration
- **Kotlin (Android)** : Optimized JNI bindings and improved memory management
- **Flutter** : Streamlined plugin architecture for both iOS and Android
- **React Native** : Updated native module bindings with better TypeScript support


### Enhanced Backend Support


v0.17.5 strengthens the underlying inference engines:


- **llama.cpp integration** : Updated to the latest version for improved LLM performance and broader model compatibility
- **ONNX Runtime** : Optimized speech pipelines for VAD, STT, and TTS with reduced latency
- **Model packaging** : Improved versioning and differential updates for over-the-air deployments


### Production-Ready Infrastructure


The update includes several improvements for enterprise deployments:


- More deterministic behavior across device fleets
- Reduced memory footprint for constrained devices
- Better thermal management and battery optimization
- Enhanced error handling and recovery mechanisms


## Why This Matters


On-device AI is no longer experimental. Teams are shipping production features that require:


1. **Privacy by default** : Keep sensitive data on-device without cloud round-trips
2. **Offline capability** : Work on planes, subways, and areas with poor connectivity
3. **Low latency** : Deliver instant responses that feel native, not cloud-dependent
4. **Cost control** : Reduce cloud inference bills by running models locally


v0.17.5 makes these capabilities more accessible by reducing the complexity traditionally associated with edge AI deployment. Instead of maintaining separate implementations for each platform, teams can now use a unified SDK with consistent APIs and behavior.


## Getting Started


SDK v0.17.5 is available now across all platforms:


- **Swift (iOS/macOS)** :[Quick Start Guide](https://docs.runanywhere.ai/swift/quick-start)
- **Kotlin (Android)** :[Quick Start Guide](https://docs.runanywhere.ai/kotlin/quick-start)
- **Flutter** :[Quick Start Guide](https://docs.runanywhere.ai/flutter/quick-start)
- **React Native** :[Quick Start Guide](https://docs.runanywhere.ai/react-native/quick-start)
- **GitHub Release** :[v0.17.5 Release Notes](https://github.com/RunanywhereAI/runanywhere-sdks/releases/tag/v0.17.5)


## What's Next


This release sets the foundation for upcoming features:


- Vision model support for multimodal experiences
- Enhanced hybrid routing with fine-grained policy controls
- Expanded model library with domain-specific optimizations
- Advanced analytics and fleet monitoring capabilities


We're committed to making on-device AI the default choice for mobile and edge applications. v0.17.5 is another step toward that vision.


## Feedback and Support


We'd love to hear how you're using the SDK:


- **GitHub Issues** : Report bugs or request features
- **Discord Community** : Join discussions with other developers
- **Email** : Reach out to our team atsupport@runanywhere.ai


Download SDK v0.17.5 today and start building privacy-first, offline-capable AI experiences.


---


*View the full release notes and download the SDK on GitHub:[v0.17.5 Release](https://github.com/RunanywhereAI/runanywhere-sdks/releases/tag/v0.17.5)*
