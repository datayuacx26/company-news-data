---
schema_version: "1.0.0"
document_id: "af2d7917df5baf9aa71aa67766306f8351a14ae4e9511636b44c0fd85db87e09"
company_key: "yc-gather-2"
company: "Gather"
source_id: "yc-gather-2-news-import-dcc281bbc950"
canonical_url: "https://www.gather.town/blog/behind-the-fix-audio-2026"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-23T10:20:21.528696+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:2780798732ac8f66739f76af91842cb09d32980264761cd5683f568f61124936"
---

# Behind the fix: Gather 2.0’s audio overhaul

Gather’s spatial audio is pretty unique compared to traditional collaboration tools. Instead of joining a fixed meeting room, conversations are spontaneous and fluid. Anyone can walk up to a teammate and start talking. The closer you are, the louder they sound. Step away, and they fade out.


That all happens on top of a dynamic virtual world, accessed by users from around the real world. It’s a complex system. For it to work, the audio pipeline powering it has to be rock solid.


When we launched Gather 2.0 in September 2025, it wasn't. The complaints came quickly, and to fix them, we decided to rebuild the pipeline from the ground up.


Today, we're happy to share that Gather 2.0's Audio/Video system is the most reliable we've ever built, **with 50% fewer low ratings since launch.**


This is the story of how we got there, a celebration of the engineering work that made it happen, and a preview of what’s next for AV at Gather.


## What happened at launch


Gather 2.0 was a major release. We redesigned the UI, rebuilt all pixel art assets, added new integrations (Spotify and GitHub), and introduced more granular spatial audio controls, allowing users to fine-tune how audio behaves.


*A screenshot of Gather 2.0, featuring a brand new UI, art style, features, and audio controls.*


The audio system itself was built on everything we learned from Gather 1.0, with meaningful improvements layered on top. Pre-launch beta testing went smoothly, and users were happy. We felt ready for the September launch.


What we didn’t anticipate: Support for Windows 10 also ended in September 2025. This forced users to move to Windows 11, which included an update that changed how real-time audio worked. Most of our beta users had Macs or high-end machines; we didn’t test thoroughly enough on lower-spec hardware or this new Windows update. That’s where things broke.


After the public launch, audio complaints flooded in. Popping noises. Audio freezing, then suddenly speeding up. Calls dropping entirely.


It was far from the experience we had planned to launch, so we made it our top priority to fix.


## Diagnosing the problem


Through October 2025, the team tried every optimization they could think of for the existing JavaScript-based audio pipeline. Every time they released an update, reports kept coming in.


One challenge was that real-time audio processing in JavaScript has limitations. Garbage collection, the process that frees unused memory, can steal CPU cycles from the audio pipeline, causing it to miss tight scheduling deadlines. And under the overall system load of Gather 2.0 (a fairly resource-intensive app), the JavaScript approach just couldn't keep up reliably.


Another challenge was that the users who reported hearing crackling audio might not be the ones actually experiencing performance problems. We’d hear from Mac users on high-performance machines, complaining about bad audio, when the problem was actually caused by their coworkers on Windows. This muddied the diagnostic process, making it harder to determine if we’d actually made progress.


## Moving audio from JavaScript to C++


The team decided to re-architect the post-processing pipeline in native C++, embedded directly in a custom Electron build. This would isolate audio processing from other work in the browser (preventing unpredictable interruptions), and generally improve performance.


They decided to implement it directly in Chromium in order to run the audio pipeline in a dedicated utility process via inter-process communication (IPC). Then it wouldn’t need to compete for resources within the browser.


This new setup would officially free us from JavaScript's overhead, with a pipeline built for the processing priority that spatial audio actually needs.


> **"It's an awesome example of biting off the complexity and just owning it, which led to significant experience boosts for our users."**
> Ben Greenier, Senior Software Engineer II at Gather


## The Windows 11 curveball


Around the same time, support for Windows 10 ended, triggering a wave of users migrating to Windows 11. The new version, Windows 11 25H2, introduced more aggressive power efficiency features, including one called EcoQos.


EcoQos deprioritizes background tasks by reducing CPU clock speed and scheduling priority. The problem: when a Gather user switches to another app (say, a spreadsheet or a browser), Windows marks Gather as no longer "active" and throttles it.


For most applications, this is a reasonable trade-off. For real-time audio, it's catastrophic.


Consistent CPU timing is non-negotiable. With EcoQos throttling our process, the audio pipeline couldn’t keep up, causing the “popping” sounds and “speed-ups” users reported.


The fix required using MMCSS (the Multimedia Class Scheduler Service), a native Windows API that lets an application signal to the OS: “I am doing time-sensitive work; do not throttle me.”


*A screenshot of the MMCSS pull request. As this update rolled out to customers, it directly led to a decrease in low AV ratings.*


This kind of system-level access wasn't possible in the original JavaScript pipeline. By moving to C++, the team could register their audio threads directly with MMCSS, giving Gather high-priority scheduling even when users are multitasking or screen sharing.


## Fine-tuning noise cancellation


With the new pipeline in place, the team also had to reintegrate[Krisp](https://krisp.ai/) , Gather's noise-cancellation partner.


They decided to run the Krisp SDK in a separate, non-sandboxed process and route audio traffic through Chromium’s existing pipeline using IPC. They felt this would be more resilient in case anything happened to Krisp’s code (or our own).


After noise cancellation runs, the team added one more layer: post-Krisp AGC (Automatic Gain Control), which boosts speaker volume after the noise cancellation process has done its job. It's a small detail with a noticeable impact on call quality.


## Where AV stands, and where we're going


Gather 2.0's AV system is the most reliable we’ve ever built, with low AV ratings down 50% since launch. But don’t worry, we’re not declaring victory and moving on! AV performance is a long-term investment, not a project with a finish line.


*Caption: We ask users to rate audio quality on a 1-5 star scale. This chart shows the percentage of low ratings. You can see a spike after a holiday-season dip, followed by generally steady improvements as we moved the audio pipeline to C++ and addressed the Windows changes.*


The team is actively working to improve reconnection speed, fine-tune automatic gain compensation, reduce visual glitches, and continue optimizing the pipeline across hardware configurations. You can follow along on our[roadmap](https://www.gather.town/roadmap) and[changelog](https://www.gather.town/whats-new) .


To everyone who sent in a bug report, flagged an issue, or just stuck with us through the rough patches: **thank you.** Your feedback is what made these audio issues fixable. We’re working hard to continue building the most delightful, energizing, and purposeful workspace for remote teams, and we’re glad you’re part of the Gather community.
