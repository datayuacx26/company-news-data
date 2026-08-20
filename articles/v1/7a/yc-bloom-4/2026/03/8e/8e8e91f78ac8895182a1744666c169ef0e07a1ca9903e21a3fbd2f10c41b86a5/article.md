---
schema_version: "1.0.0"
document_id: "8e8e91f78ac8895182a1744666c169ef0e07a1ca9903e21a3fbd2f10c41b86a5"
company_key: "yc-bloom-4"
company: "Bloom"
source_id: "yc-bloom-4-rss-7e172d21f9c7"
canonical_url: "https://bloom.diy/blog/apple-is-banning-vibecoding-apps-we-re-building-for-android"
published_at: "2026-03-23T04:00:00+00:00"
first_seen_at: "2026-07-24T19:36:45.864102+00:00"
fetched_at: "2026-07-28T20:53:13.933878+00:00"
content_hash: "sha256:8726ad812ce13735a82d6dda4cfa9e87513c7fd1a94bdd6282fd9c4040f2ae59"
---

# Apple Is Banning Vibecoding Apps. We're Building for Android.

# Apple Is Banning Vibecoding Apps. We're Building for Android.


Apple is blocking vibe coding apps from the App Store while adding AI coding to Xcode. Here's what happened and why we launched Bloom on Android.


Sirian Maathuis


March 23, 2026 ·


7 min read


Something changed last week. Apple quietly started blocking updates for vibe coding apps on the App Store. Tools that let people build software from their phones using natural language prompts can no longer ship updates to their iOS apps. Apple told developers their apps violate App Store Guideline 2.5.2, a rule that says apps can't "download, install, or execute code which introduces or changes features or functionality of the app." The reasoning: when you use a vibe coding app to build something new, the app's functionality changes. Apple says that's not allowed. Meanwhile, Apple just shipped Xcode 26.3 with built-in agentic coding powered by OpenAI and Anthropic. Apple's message is clear: AI coding is fine, as long as Apple controls it.


[Part 1: Everyone can build apps now. Just not on iPhone.](https://x.com/CreateWithBloom/status/2036072867738071114)


[Part 2: Software is becoming yours. Just not on iOS.](https://x.com/CreateWithBloom/status/2036435705438642408)


## What Actually Happened


On March 18, 2026,


[The Information reported](https://www.theinformation.com/articles/apple-cracks-vibe-coding-apps) that Apple had been quietly preventing vibe coding apps from releasing updates on the App Store.


[9to5Mac](https://9to5mac.com/2026/03/18/apple-pushing-back-on-vibe-coding-iphone-apps-developers-say/) ,


[MacRumors](https://www.macrumors.com/2026/03/18/apple-blocks-updates-for-vibe-coding-apps/) , and


[AppleInsider](https://appleinsider.com/articles/26/03/18/bad-vibes-apple-blocks-updates-for-some-ai-coding-apps-in-the-app-store) all confirmed it. Here's what we know:


- **Multiple vibe coding apps**


haven't been able to push updates to their iOS apps since early 2026. Apple told some developers to open generated projects in an external browser instead of inside the app. Others were told to remove the ability to create software targeting Apple devices entirely.


- **Bloom was affected too.**


Our iOS app is production-ready, but we've hit sudden rejections that contradict previous approvals. We're continuing to work with Apple to get our updates approved and serve Bloom users on iOS.


- **Apple cited Guideline 2.5.2**


, which says apps must be "self-contained" and can't execute code that changes their functionality after review.


- **Apple also cited Section 3.3.1(B)**


of the Developer Program License, which says interpreted code can't "change the primary purpose of the Application."


We know this isn't just happening to other companies.


**Bloom has experienced it firsthand.**


We received sudden App Store rejections with little to no explanation, contradicting reviews that had previously approved the same functionality. Features that passed review for months were flagged without warning and without a clear path to compliance. Nothing about our app changed. Apple's enforcement did. Apple's official position: this isn't a new policy. They say these are long-standing rules and they've been communicating with developers about compliance. That framing is technically true and completely misses the point. If the rules haven't changed, why are apps that passed review for months suddenly being rejected?


## The Double Standard


Three weeks before blocking these apps, Apple released


[Xcode 26.3](https://www.apple.com/newsroom/2026/02/xcode-26-point-3-unlocks-the-power-of-agentic-coding/) with full support for agentic AI coding. The update integrates Claude and OpenAI Codex directly into Xcode so developers can generate entire features, migrate APIs, and create login flows using natural language prompts. Apple's own marketing language for Xcode 26.3: "Coding agents understand goals and autonomously work toward them." That's vibe coding. Apple just calls it something different when they're the ones selling it. The difference: Xcode runs on a Mac and costs nothing (because it feeds Apple's developer ecosystem and App Store revenue). Vibe coding apps run on a phone and let anyone build software without going through Apple's gatekeeping process. Web apps built with these tools bypass the App Store entirely, and Apple's 30% commission along with it. This isn't about safety or quality. It's about control and revenue.


## Why This Matters For Everyone Building Software


Vibe coding is the biggest shift in software creation since the smartphone. For the first time, people without technical backgrounds can describe what they want and get a working application. Not a mockup. Not a wireframe. A real app with a database, authentication, and a backend. That's threatening to Apple's business model because:


1. **It removes the developer bottleneck.**


Apple's ecosystem depends on professional developers building apps and selling them through the App Store. If anyone can build their own tools, fewer apps flow through the store.


2. **Web apps bypass the 30% cut.**


Vibe-coded web apps don't need the App Store. No App Store means no review process and no commission for Apple.


3. **Mobile creation competes with Xcode.**


Building on your phone is faster and more accessible than building on a Mac with Xcode. Apple wants AI coding to happen inside their tools, not third-party ones.


The apps being blocked aren't distributing malware. They aren't tricking users. They're giving people the ability to build software on their own phones. Apple decided that's a problem.


## What We're Doing About It


We believe everyone should be able to build apps on any device. That conviction doesn't change because Apple disagrees. Today, we're opening early access to


[Bloom on Android](https://bloom.diy/android) . You can now build, preview, and share full-stack mobile apps from your Android device. Same AI-powered generation. Same real backend with database, auth, and real-time sync. Same instant sharing. No gatekeeper deciding what you're allowed to create. If Apple won't let people build apps on iPhone, we'll make sure they can build them on Android. And everything you build with Bloom still works on iOS, Android, and the web from a single codebase. The apps work everywhere. We just can't let Apple dictate where you build them.


## What This Means for Founders & Developers


If you're building an MVP or prototyping an idea, here's the practical impact:


- **Bloom on iOS is coming soon.**


We have an iOS app, but we've already received unexplained rejections from Apple, and we expect the pressure to continue.


- **Bloom on Android is now available**


in early access. If you want to build apps from your phone without worrying about Apple's next policy change, Android is the move.


- **Everything you build is cross-platform.**


Your app runs on iOS, Android, and the web regardless of which device you build it on. Your users don't know or care where you built it.


- **Web works too.**


You can always build at


[https://bloom.diy/](https://bloom.diy/) from any browser on any device. No app store required.


## The Bigger Picture


Apple's track record here is consistent. When a new category of software threatens their control over the ecosystem, they restrict it first and find the policy justification second. They did it with


[game streaming apps](https://www.theverge.com/2020/9/18/20912689/apple-cloud-gaming-streaming-xcloud-stadia-app-store-guidelines-702) . They did it with


[browser engines](https://open-web-advocacy.org/walled-gardens-report/#apple-ios-browser-engine-ban) . Now they're doing it with vibe coding. The good news: pressure works. Apple eventually allowed game streaming. The EU forced them to allow alternative browser engines. The same will likely happen with vibe coding apps. But we're not waiting for Apple to change its mind. We're building for the platforms that let people create freely.


---


## FAQ


### Are Vibe Coding Apps Banned from the iPhone?


Not entirely, but the crackdown is broad. Apple has blocked updates for several vibe coding apps, citing App Store Guideline 2.5.2. Bloom has also received sudden, unexplained rejections that contradict past reviews. The affected apps are still available to download, but shipping updates has become unpredictable for the entire category.


### What is Apple's Guideline 2.5.2?


It's an App Store rule that says apps must be "self-contained" and cannot download or execute code that changes their functionality after passing review. Apple is applying this rule to vibe coding apps because they allow users to generate new software inside the app.


### Why is Apple Blocking vibe coding apps?


Apple cites policy compliance, but the timing tells a different story. Vibe coding tools let people build web apps that bypass the App Store (and Apple's 30% commission). They also compete with Apple's own development tools. The blocks started shortly after these apps gained significant traction.


### Can I Still Build Apps with Bloom on iPhone?


You can always build at


[https://bloom.diy/](https://bloom.diy/) from any browser, and Bloom on Android ensures you have a way to build from your phone no matter what Apple decides.


### What is Bloom on Android?


Bloom on Android is now in early access. It lets you design, build, and share full-stack mobile apps directly from your Android device.


[Sign up here →](https://bloom.diy/android)


### Does this affect the apps I've already built with Bloom?


No. Apps you've built with Bloom run on iOS, Android, and the web regardless of Apple's policies around builder tools. Apple is restricting the tools used to create apps, not the apps themselves.
