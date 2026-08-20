---
schema_version: "1.0.0"
document_id: "4d3c85531243a78bf391a439c51b9c3da0cd70fefb77fe14532e8ca968ab51bc"
company_key: "yc-mentra"
company: "Mentra"
source_id: "yc-mentra-atom-1275414bf67e"
canonical_url: "https://mentraglass.myshopify.com/blogs/blog/mentra-3-0-local-miniapps-full-user-control-and-enterprise-smart-glasses"
published_at: "2026-08-17T09:06:59+00:00"
first_seen_at: "2026-08-17T10:26:12.371310+00:00"
fetched_at: "2026-08-20T00:50:23.357001+00:00"
content_hash: "sha256:1b48ece3420d08438406e4b8115de8c31a78aea6cc5d10f1596cf09078b17978"
---

# Mentra 3.0 - Local Miniapps, Full User Control, and Enterprise Smart Glasses

Nearly two years ago, we launched MentraOS with one goal: to create then open, user-controlled, universal operating system for smart glasses. One place for users to discover miniapps, one platform for developers to build experiences that work across different glasses, one system for OEMs to power their next-generation smart glasses.


MentraOS first reached users through Vuzix Z100 and Even Realities G1. Those early users taught us what people actually need from smart glasses: miniapps and an SDK that are dependable enough to use every day.


Mentra Live expanded that vision with a camera, microphone, and an entirely new set of possibilities. It also showed us where the experience still needed work. Since its launch, we’ve listened closely to users and focused on making its core features faster, easier to use, and far more reliable.


We also learned that running miniapps in the cloud was not the right long-term foundation. It helped us move quickly, but introduced latency, reliability, privacy, and offline limitations that users, developers, and OEMs could feel.


Mentra 3.0 is our answer to everything we’ve learned. We’ve rebuilt most of MentraOS around miniapps that run locally on the phone, introduced an entirely new Mentra Miniapp SDK, significantly improved Mentra Live, and created a new library of miniapps.


Today, we’re launching Mentra 3.0, our biggest update to MentraOS yet.


### MentraOS Changelog


- MentraOS has been almost entirely rewritten.
- All miniapps now run entirely on the phone.
- All miniapps are now far more reliable and respond to user input faster.
- The capsule menu within miniapps has been overhauled, and now has the option to exit or minimize miniapps.
- There’s a brand new[Miniapp SDK](https://docs.mentra.glass/) , which makes it incredibly easy to build miniapps for all MentraOS glasses.
- Miniapps can now expose actions that other miniapps, such as Mentra AI, can trigger.
- The settings page is now easier to navigate.
- Local speech-to-text and text-to-speech models now automatically download over Wi-Fi, and miniapps now automatically use them when internet connection is lost.


### Mentra Live Changelog


We’ve spent the last month completely focusing on improving Mentra Live.


- Taking photos is significantly faster, especially for repeated shots and photos requested by miniapps.
- Photos transferred to your phone over Bluetooth are now much higher resolution, arrive faster, and retain significantly more detail.
- Syncing photos and videos from Mentra Live no longer interrupts your phone’s internet connection.
- Gallery sync is now resumable and recoverable. Interrupted transfers can continue automatically, missing gallery entries are repaired, and photos already downloaded to the Mentra App are safely reconciled with your camera roll.
- We’ve added new camera controls, including 5, 15, or 30 fps recording, and a wider range of camera field-of-view settings.
- Livestreaming is now more reliable and automatically recovers from temporary network interruptions. Mentra Live now uses hardware-accelerated video encoding when available, reducing battery usage and heat.
- Wi-Fi setup is more reliable.
- Miniapps can now pre-warm the Mentra Live camera for near-instant capture, record video, select resolution and frame rate, and request a new capture mode optimized for reading text.


### Even Realities G2 Changelog


We’ve been listening to Even Realities G2 users! Double-tapping now opens the native G2 dashboard, making it easier to switch miniapps directly from your glasses and receive notifications from your iPhone.


We’ve also overhauled G2 display and microphone recovery. Captions and Translation remain responsive after opening or closing the dashboard, text and images render more reliably, and calendar syncing has been improved.


### Miniapp Store


The Mentra Miniapp Store is still a fundamental part of our ecosystem. We are revamping the Mentra Miniapp Store to work with the new local miniapps system. We’re designing it in a way so that other smart glasses OEMs can use our store, so it can be accessed in more than just the Mentra app. As such, we still have work to do on the Miniapp Store. So, the initial release of Mentra 3.0 doesn’t include the Miniapp Store. Instead, all official Mentra miniapps are included by default when installing Mentra 3.0. The Miniapp Store will return later in 2026.


### Merge Miniapp


We’ve completely rewritten Mentra Merge, our proactive AI assistant, from the ground up.


Merge is more intelligent, has a brand new on-phone UI, and is now compatible with Mentra Live.


### Captions Miniapp


Captions miniapp now automatically switches between online and offline transcription automatically.


### Translation Miniapp


Translation has been completely rewritten. It now automatically detects foreign languages, so users no longer need to select a foreign language. It can also translate multiple languages at once.


Vietnamese translation is now supported on Even Realities G2.


### Mentra Notes


We’ve made it easier to export your notes and transcripts from Mentra Notes. Press the “export” button, and you’ll get the option to export to any app on your phone.


### Mentra AI Miniapp


Mentra AI is now smarter and has access to new tools. We’ve added a model selector to allow you to use any AI model you’d like, and have given it the ability to start, stop, and trigger actions within other miniapps. If you’re a miniapp developer, you can specify actions within your own miniapp that Mentra AI can access.


### Mentra Maps Miniapp


Introducing Mentra Maps, an entirely new navigation miniapp making its debut with Mentra 3.0.


Mentra Maps provides visual and spoken turn-by-turn directions through your smart glasses. It’s available on every pair of glasses compatible with MentraOS, and we’d love to hear what you think.


### Teleprompter Miniapp


Teleprompter is back and completely rebuilt from the ground up for the new Mentra Miniapp SDK.


We retired the old version because it no longer met our quality bar. The new Teleprompter runs entirely on your phone, works without an internet connection, and includes both automatic scrolling and an AI Scroll mode that follows your voice.


### Recorder Miniapp


Recorder began as a simple internal debugging tool and was never intended for public release. For Mentra 3.0, we’ve transformed it into a polished miniapp for recording audio from your smart glasses.


Recorder includes pause and resume, live transcription, playback, searchable recordings, and the ability to share either the audio or transcript. It’s compatible with all MentraOS glasses.


### Conclusion and What Comes Next - Enterprise Smart Glasses


Mentra is just getting started. After tens of thousands of glasses connected to MentraOS, we’ve learned a lot about what works, what doesn’t, and what’s coming next. Throughout, we’re staying committed to our vision of building the open OS for smart glasses that gives users full control. And we believe that starts in the enterprise.


Since launching Mentra Live, we’ve shipped thousands of devices. We’ve seen developers, consumers, and businesses using Mentra Live for dozens of different use cases. We’ve also seen companies like Even Realities, Meta, Rokid, and others ship their own visions of what smart glasses are about.


The data from our perspective is clear. The greatest need for smart glasses today is in enterprise. While AI has taken over the world of knowledge work, it’s barely touched the physical world. Yet over 70% of the world’s economy works with their hands. Smart glasses allow AI to see what workers see, hear what they hear, and guide them in the moment. The ability to capture, read, look up, guide, write back, and verify is the loop that smart glasses enable across automotive repair, manufacturing, logistics, HVAC, and frontline work of every kind.


MentraOS is the only smart glasses system today that allows that to happen. It’s open source and user controlled so companies can trust it. It allows businesses to own their own data, use their own AI, and deploy air-gapped. We’re upgrading MentraOS to meet the needs of the enterprise in data security, privacy, air-gapping, deployment, fleet management, and more. Remote expert assistance, automatic documentation, and hands-free visual AI assistants. This is how smart glasses provide value today, and it’s how we build out the OS that will power every pair of smart glasses tomorrow. And we’re just getting started.


– The Mentra Team
