---
schema_version: "1.0.0"
document_id: "96a32f0569ddd6301f00b7cbd898a6a37e67985360c901f5c9252ff377ca54cd"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/building-google-docs-like-live-collaboration-for-a-cross-platform-app-used-by-millions-in-rust-"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:bf376fe3ee993ea4c1c6c8a4e7998d75c51223b75694e9eb2c903c1cfef32184"
---

# Building Google-Docs-like live collaboration for a cross-platform app used by millions (in Rust)

Imagine yourself with 3 different clients (iOS, Android and web), each written in a different language, each already used by millions of users. Now imagine that you wanted those clients to communicate together. Not simply client <> server, but that if a user edits an image on iOS, that image would display in the exact same way on Android. Instantly. Reliably. Same for text. Oh and AI effects too.


That’s the situation we faced about 2 years ago at[Photoroom](https://www.photoroom.com/) . We provide editing tools for e‑commerce images and we wanted guarantee our users that their content was always in sync, even when their teammates where working on it at the same time.


The good news is that we were already sharing our rendering code between platforms ([read about it in our 2021 article](https://www.photoroom.com/inside-photoroom/building-cross-platform-image-renderer) ). The bad news is that it was written in C. Not the best language to decode payloads back and forth over the network.


We ended up picking Rust and built a cross-platform live-collaboration engine. It’s been live (no pun intended) on all 3 platforms for a little while now. Here’s a quick demo:


## Why Rust? Why not React Native?


Besides attempting to make it to the home page of Hacker News, we picked Rust because… it would be easier to hire than for C++. We know how attractive the language can be to experienced developers. We also considered Golang, but its runtime makes it more complicated to run on the web.


This may seem like a lot of pointless work to run on 3 platforms when solutions like React Native exist. We strongly believe in providing the very best image editing experience to our users, and we don’t think it’s possible to do so with React Native (but this stance might change in the future).


## How it works


In a nutshell, the Rust code is compiled on iOS, Android and the web. The rendering of the UI is done natively (SwiftUI, Compose, React), same for the network calls (URLSession, OkHttp, fetch), same fore rendering (Metal, OpenGL, WebGL/WebGPU through wgpu). But all the logic is inside the Rust code.


Wait, that’s a bit lightweight for an explanation, isn’t it? We’re quite proud of the work we did, that’s why we detailed the nitty-gritty[in a 5-parts blog post series](https://www.photoroom.com/inside-photoroom/building-live-collaboration-in-rust-for-millions-of-users-part-1) .


If this sort of work sounds exciting to you, we’re hiring a head of cross-platform to provide a seamless experience to millions of users.[Learn more here](https://jobs.ashbyhq.com/photoroom/dc994a7c-e104-46e1-81c3-b88d635398b9) .
