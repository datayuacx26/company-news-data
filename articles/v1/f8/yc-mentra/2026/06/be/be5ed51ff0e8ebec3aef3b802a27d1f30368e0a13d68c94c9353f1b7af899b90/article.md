---
schema_version: "1.0.0"
document_id: "be5ed51ff0e8ebec3aef3b802a27d1f30368e0a13d68c94c9353f1b7af899b90"
company_key: "yc-mentra"
company: "Mentra"
source_id: "yc-mentra-atom-1275414bf67e"
canonical_url: "https://mentraglass.myshopify.com/blogs/blog/mentra-roadmap-update-moving-to-miniapps-on-the-phone"
published_at: "2026-06-09T04:32:05+00:00"
first_seen_at: "2026-07-27T03:44:49.649440+00:00"
fetched_at: "2026-08-20T00:50:23.357001+00:00"
content_hash: "sha256:486f1ca5555dffa55f620fb9fbef3ea5aace5feb51bc869d895b54d75924770f"
---

# Mentra Roadmap Update: Moving to Miniapps on the Phone

Over the last year, MentraOS has grown because of this community.


Developers have built apps, tested new ideas, given us direct feedback, debated our architecture, and helped us understand what smart glasses software actually needs to become. At the same time, we’ve been working closely with several OEM partners preparing to launch with MentraOS later this year and next year. Their feedback has been strongly aligned with what we’ve heard from the community: the platform needs to become more local, more reliable, lower latency, more private, and easier to integrate deeply.


Thank you to everyone who has helped us get here and shared feedback on MentraOS. We love that this has already become a community-driven project, with thousands of developers, users, and companies helping shape the future of smart glasses.


In this note, we’re outlining the next phase of that journey.


## How MentraOS works today


Today, MentraOS apps run in the cloud.


The current architecture looks roughly like this:


Glasses → phone → Mentra cloud relay → developer cloud app, and back again


This model let us move quickly. It let MentraOS support multiple pairs of smart glasses, reach tens of thousands of downloads, and let developers ship apps without building native mobile integrations. It helped prove that smart glasses need an app platform.


But the current architecture has limits. Because apps run in the cloud, the phone needs a relay to connect the glasses to those apps. Otherwise, every app would need its own streams for audio, display, camera, and control, which would quickly get bad for data usage, battery life, latency, and reliability.


The relay solved an important problem, but it also created new tradeoffs. A platform centered around Mentra cloud infrastructure raises concerns around control, privacy, enterprise deployment, and OEM ownership. We have done a lot of work to improve those issues, but they are not fully solvable with the current architecture.


A simple example is a button press. If an app needs to respond to a hardware button on the glasses, that event goes from the glasses, to the phone, to the Mentra cloud relay, to the developer’s cloud app, and back again. Even in a good case, that can mean hundreds of milliseconds before the app can respond. For interaction on smart glasses, that is not good enough.


## The new architecture: apps run locally on the phone


A core requirement for MentraOS is that smart glasses should be able to run multiple apps at once, even though the glasses only have one active connection to the phone.


That means apps cannot each connect to the glasses independently. If every app owned its own Bluetooth connection, you would be limited to one app at a time, and every developer would need to rebuild the same glasses integrations.


MentraOS 3.0 solves this by putting all apps inside one phone host app.


OEM apps that ship with MentraOS include the Mentra runtime. If an OEM releases glasses powered by MentraOS, their mobile app can host the same local runtime as the Mentra app. As a developer, your app runs locally inside that runtime and can work across MentraOS-supported glasses.


The Mentra app is our open source, hackable host app for building, testing, and distributing apps across supported glasses.


The new architecture looks roughly like this:


Glasses → phone app → local miniapp, and back again


Miniapps will be built with the


Mentra Miniapp SDK


. They run locally on the phone, inside the host app, and use the Mentra runtime to control glasses input and output, app lifecycle, permissions, storage, networking, and native phone features.


For developers, this means:


- much lower latency


- better reliability


- local-first app behavior


- direct connection to your own backend


- local data storage


- better privacy and compliance options


- less dependency on Mentra cloud infrastructure


- one app that can work across multiple glasses supported by MentraOS


This is the architecture that makes MentraOS feel like an operating system for smart glasses: fast, local, extensible, and built for deployments that matter.


## Mentra Bluetooth SDK


As more companies have started deploying and experimenting with MentraOS, we have heard a consistent need from businesses, OEMs, and enterprise teams: some deployments need direct app control, on-premise support, offline behavior, stricter compliance paths, or deeper integration into an existing mobile app.


For that, we’ve modularized MentraOS so the Bluetooth connection layer between the phone and glasses can be used independently.


That is now available as the


Mentra Bluetooth SDK


.


We believe direct access should be official, documented, and compatible with the broader MentraOS ecosystem, not something teams have to reverse engineer.


The Bluetooth SDK is best for:


- B2B deployments


- single-purpose applications


- direct integrations into an existing mobile app


- cases where one app controls the glasses directly


The tradeoff is that with the Bluetooth SDK, you own everything above the Bluetooth connection. You need to build or integrate your own ASR, TTS, AI layer, glasses scanning and connection flow, app distribution, permissions, lifecycle handling, and power-saving behavior.


With the Mentra Miniapp SDK, those platform pieces come out of the box. It is usually 3–10x faster to build on the Miniapp SDK, and your app can still run across the MentraOS ecosystem.


You can start here:


[https://github.com/Mentra-Community/Mentra-Bluetooth-SDK-Starter-Kit](https://github.com/Mentra-Community/Mentra-Bluetooth-SDK-Starter-Kit)


For most developers, the


Mentra Miniapp SDK


is the better path. It gives you the app platform, not just the connection layer.


The Bluetooth SDK gives you direct control for single-purpose deployments.


The Mentra Miniapp SDK gives you the platform for building apps.


## Roadmap and migration


*Timeline updated: July 20, 2026*


**June 10:**


- End active support for Cloud SDK miniapps for Mentra Live. They still work, but we no longer build the Cloud SDK.


- Going forward, developers building direct camera or B2B integrations for Mentra Live should use the


[Mentra Bluetooth SDK](https://github.com/Mentra-Community/Mentra-Bluetooth-SDK-Starter-Kit) , available now for Android, iOS, and React Native.


**August 3:**


- MentraOS 3.0 is launched!


- Support ends for all Cloud SDK miniapps. Cloud SDK miniapps will no longer work in the main Mentra app at this point.


- All official Mentra miniapps will have been ported to the new Miniapp SDK as part of this update and will continue to function normally.


- New Miniapp SDK released in private beta. Reach out for early access.


- To continue to use old Cloud SDK miniapps, we will offer[MentraOS Legacy](https://mentraglass.com/legacy) .


**September 2026:**


- Public launch of new Miniapp SDK, used to develop miniapps for display glasses, such as Even Realities G2, Even Realities G1, and Vuzix Z100.


**October 2026:**


- Support ends for[MentraOS Legacy](https://mentraglass.com/legacy) . Cloud SDK miniapps no longer work.


For anyone with an existing miniapp, we will publish a detailed porting guide and provide support if you hit issues during migration.


## Continued Cloud support


This community has already built over 1,000 apps using the Cloud SDK. Some of those applications were deployed in the real world and are still being used til today. We want to make sure we maintain proper support as we support your transition to the new Mentra Miniapp SDK. We will continue legacy support of the Cloud SDK for at least 2 months after the launch of the Miniapp SDK. For details on how to continue using the Cloud SDK after the launch of the Miniapp SDK, see[MentraOS Legacy](https://mentraglass.com/Legacy) .


## Why this is better for developers, users, and businesses


This change is about making MentraOS a better platform for you to build and deploy your apps.


Apps built with the Mentra Miniapp SDK will be faster, more reliable, more private, and more powerful. Developers will be able to build apps that feel instant, work closer to the edge, store data locally, connect directly to their own backend, and still distribute through the Mentra ecosystem.


Users get apps that feel more responsive and dependable. Businesses get a cleaner path for privacy, compliance, offline behavior, and direct integration into their own systems. OEMs get more control over their own user experience while still giving developers one runtime that can work across many different glasses.


This is the version of MentraOS the community has been asking for: local apps, lower latency, better reliability, and one SDK for the next generation of smart glasses.
