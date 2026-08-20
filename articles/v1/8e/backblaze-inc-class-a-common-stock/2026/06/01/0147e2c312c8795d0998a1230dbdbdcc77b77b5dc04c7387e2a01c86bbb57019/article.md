---
schema_version: "1.0.0"
document_id: "0147e2c312c8795d0998a1230dbdbdcc77b77b5dc04c7387e2a01c86bbb57019"
company_key: "backblaze-inc-class-a-common-stock"
company: "Backblaze Inc."
source_id: "backblaze-inc-class-a-common-stock-rss-a06767c1ff83"
canonical_url: "https://www.backblaze.com/blog/backblaze-generative-media-hackathon-build-with-b2-genblaze-and-gmi-cloud/"
published_at: "2026-06-23T02:07:13+00:00"
first_seen_at: "2026-07-20T23:18:53.525659+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:08474fc4888397843ade2cf4741247204d669a7679cabc08b943e29f77f0d640"
---

# Backblaze Generative Media Hackathon: Build with B2, Genblaze, and GMI Cloud

The generative AI wave is moving from text into media. ChatGPT taught a generation of developers how to wire up an LLM. The next chapter is harder and more interesting: video, image, audio, and multimodal workflows that combine them. New models for each of these are landing every few weeks, from established labs and from teams nobody had heard of a quarter ago. The quality keeps climbing, and the catalog keeps expanding.


That pace is good news, but it asks more of the apps that depend on it. A pipeline that hard-codes today’s best video model is going to feel dated in a month. The apps that come out ahead will be the ones built on reactive architectures: pipelines that stream progress as events, fan out concurrent work, fall back when a provider stalls, and let you swap in a new model with a single line of code.


That’s where the moat is now. If you’re building an AI media product, the differentiation is in the pipeline: which models you call, in what order, how you fall back when one is slow, how you keep latency tolerable, where the assets land, and how you prove what was generated. The teams winning this space treat the orchestration layer as their core IP. The frontend on top is the easy part.


We’re looking for builds that solve real-world use cases. Show us what becomes possible when the pipeline stops being a bottleneck.


## What we’re running


The Backblaze Generative Media Hackathon is an online competition for developers building generative media apps. $10,000 in cash prizes, 10 GB of free Backblaze B2 storage to start with, and GMI Cloud credits for the first 270 eligible participants.


- **Grand Prize:** $7,000
- **Second Place:** $2,000
- **Third Place:** $1,000


Registration and submissions run June 22 through August 3, 2026 (5 pm ET). Winners are announced August 12.


## The stack


Backblaze B2, Genblaze, and GMI Cloud are the three pieces of a media pipeline that’s both easy to vibe code and serious enough to take to production.


**Backblaze B2** is S3-compatible object storage for the generated assets, the metadata, the provenance manifests, the thumbnails, and the logs. Durable URLs that never expire. Event Notifications fire downstream work without polling. Object Lock makes your provenance records tamper-evident. Lifecycle Rules handle cleanup so intermediate artifacts don’t pile up.


**Genblaze** is our open-source Python SDK for orchestrating generative media workflows. One Pipeline API spans OpenAI, Google, Runway, Luma, Decart, ElevenLabs, Stability Audio, NVIDIA NIM, GMI Cloud, and others. Every run produces a SHA-256-verified provenance manifest. Swap providers with a one-line change. New models drop every couple of months, and your app doesn’t need a rewrite each time.


**GMI Cloud** is a unified inference platform for open source AI. Image, video, audio, chat, reasoning, and multimodal models behind a single API key. If you want to try five video models in a weekend, this is the easiest way.


## What to build


Anything that turns a generative AI capability into something a real person would actually use. Some directions:


- AI video generation or editing apps
- Image generation, remixing, or transformation tools
- Audio, music, or voice generation workflows
- Multimodal apps that combine text, image, video, and audio
- AI media libraries for storing, organizing, and searching generated assets
- Provenance-aware workflows that track how each piece of media was generated
- Agentic media pipelines that generate, evaluate, retry, and store outputs
- Tools for creators, marketers, educators, entertainers, or developers


Judges will weigh real-world utility, production readiness, and how meaningfully your app uses B2 and Genblaze. We’re looking for projects where the pipeline is doing real work. Apps that handle multiple providers, recover from errors, track provenance, and store assets reliably.


## How to enter


Visit the hackathon page on Devpost for registration details, eligibility requirements, prizes, submission guidelines, and important dates:


[https://backblaze-generative-media.devpost.com](https://backblaze-generative-media.devpost.com/)


Build your generative AI media application using Backblaze B2 and Genblaze, then submit your project before the deadline.


If you want a head start on what a Genblaze + B2 app looks like in code, two reference repos already exist.`[genblaze-gen-media-multi-provider-sample](https://github.com/backblaze-labs/genblaze-gen-media-multi-provider-sample)` chains five providers into one prompt-to-MP4 pipeline.[genblaze-gmicloud-pipeline](https://github.com/backblaze-labs/genblaze-gmicloud-pipeline) is a deep multi-model composition inside one provider.


We’re looking forward to seeing what you build.
