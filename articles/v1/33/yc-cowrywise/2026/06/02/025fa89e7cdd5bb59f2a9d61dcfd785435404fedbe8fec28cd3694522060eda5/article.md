---
schema_version: "1.0.0"
document_id: "025fa89e7cdd5bb59f2a9d61dcfd785435404fedbe8fec28cd3694522060eda5"
company_key: "yc-cowrywise"
company: "Cowrywise"
source_id: "yc-cowrywise-rss-9d19c7453d3c"
canonical_url: "https://cowrywise.com/blog/from-weekend-experiment-to-production-app-building-cowrywise-kids-with-kotlin-multiplatform/"
published_at: "2026-06-10T14:53:53+00:00"
first_seen_at: "2026-07-20T23:23:41.453555+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:2f3649771306595c2815a8684042fa404a8521142e44417962fef576f7a8d225"
---

# Building Cowrywise Kids with Kotlin Multiplatform

In February 2026, I started building native Android and iOS apps for Cowrywise Kids.


It was not on the roadmap. No one assigned it to me. No one asked for a prototype. I just could not stop thinking about the gaps in the experience.


Cowrywise Kids already existed as a web app and PWA, but the more I used it, the more obvious the mobile problems became: unreliable push notifications, no real app store presence, weak offline support, and too many moments where a child could simply drop off.


And children do not fight through friction. They just stop.


# **What Cowrywise Kids Is**


Cowrywise Kids is a financial literacy product for children aged 5 to 17, built by Cowrywise, a Nigerian savings and investment platform. Children go through lessons, play games, complete quizzes, unlock progress, and build basic money knowledge over time. Parents stay connected through an account structure that gives them visibility and feature controls.


This is not a normal content app. It is a learning product for children, which means retention, motivation, safety, and low-friction access matter more than usual. A parent who finds the experience confusing may not re-engage. Every engineering decision in this project had to hold up against those realities.


That context is what made the mobile gap so frustrating to leave alone.


# **Why Cowrywise Kids Needed Native Apps**


The PWA had real limitations. App store discoverability matters for building daily habits in children; parents search Google Play and the App Store, not the mobile web. Push notifications are unreliable in PWAs. Background content preloading is also harder on the web, which matters when the goal is to make the next lesson ready before a child opens the app.


The decision to go native was not hard. The harder question was how to build for both Android and iOS without running two completely separate engineering tracks.


# **Why Kotlin Multiplatform Was the Right Bet**


Three options were on the table:


**Option** **Trade-off**


Native Android + Native iOS Maximum platform control, but duplicated product logic and higher maintenance overhead


Flutter Mature cross-platform UI story, but required adopting Dart, a new UI framework, and a separate ecosystem


Kotlin Multiplatform + Compose Multiplatform Shared Kotlin domain, data, and UI layers, with native platform escape hatches where needed


Kotlin Multiplatform came down to one idea: share what should be shared, keep what should be native. Business logic, networking, persistence, and state management written once in Kotlin, running on both platforms. UI through Compose Multiplatform. Platform-specific behaviour through expect/actual.


The trade-offs going in were real: Compose Multiplatform is still evolving, Room Multiplatform is relatively new, and iOS debugging is not as mature as it is on Android. Those were accepted in exchange for a genuinely shared codebase. In retrospect, they were the right trade-offs.


# **What Compose Multiplatform Actually Changed**


One of the biggest misconceptions around Kotlin Multiplatform is that shared business logic is where the story ends. With Compose Multiplatform every screen: auth, course list, lesson detail, quiz flow, leaderboard, profile, is shared. Design system, theming tokens, animations, state-driven rendering, all shared. The adaptive layout system that handles tablets and landscape, shared.


The benefit wasn’t just fewer files, It was consistency. A bug fixed in the LMS fixes it everywhere. A UI change ships to both stores at once. That’s the real return, not just lines of code saved, but divergence prevented.


One place this showed up in an unexpected way: the MVI pattern. Every screen follows:


Because the reducers live in shared code, both platforms get identical state transition behaviour.


The quiz flow is a useful example. It had multiple distinct states: loading, active question, answer selected, correct answer, incorrect answer, submission pending, submission failed, completed, and retry.


Without shared reducers, Android and iOS could have drifted apart subtly over time, a slightly different error state here, a missed transition there. With the MVI layer in shared code, the same intent produced the same state transition on both platforms, every time. That kind of consistency is hard to audit for and easy to lose. KMP made it structural rather than disciplinary.


# **The Lesson That Changed the Entire Architecture**


The most important technical decision in the project wasn’t Kotlin Multiplatform. It wasn’t Compose. It wasn’t Room. It wasn’t even offline sync.


It was simply a Boolean.


This might seem insignificant at first glance but it changed the way the entire learning experience works. The original progression model was straightforward: *Read a lesson → Complete a quiz → Unlock the next lesson* . This was perfectly reasonable, until internet connection was considered especially in an app for kids.


The web app gated lesson progression behind quiz completion. That works fine on a reliable connection. For most kids who rely on their parent for internet connection it meant: no network = quiz can’t submit = lesson stays locked, even if they’d read and understood the content.


The solution was an **isRead** flag that decoupled reading from assessment. Reading sets **isRead = true** locally and unlocks the next lesson immediately. Quizzes are still required to mark a course as complete, learning validation is preserved, but reading progression is offline-capable. No backend changes. No new API contract.


# **Offline-First Is a Product Philosophy, Not a Technical Feature**


A lot of apps claim offline support. What they really mean is: data was cached. That’s not the same thing. True offline-first means the application continues to deliver value even when the network disappears. That mindset shaped almost every engineering decision in the Learn module.


The rule was simple: the Learn module UI never reads from the network. Data flows strictly in one direction:


The UI has no idea whether it’s rendering cached data or fresh data. It just renders state.That single decision ended up influencing nearly every offline-first pattern that followed.


Sometimes the most impactful architectural decisions are also the smallest ones.


# **What Building for iOS Taught Me**


I’m primarily an Android engineer. A few things I wish I’d known before touching the iOS side of a KMP project.


- iosMain is Kotlin, not Swift. Writing an actual implementation for iOS means writing Kotlin that calls iOS APIs via Kotlin/Native interop. The syntax stays in Kotlin. There’s no language context-switch, but SwiftUI ergonomics are gone and IDE support is thinner than on Android. Expect the adjustment.


- api vs implementation in your Gradle build has iOS consequences. Declaring a dependency as implementation keeps its types internal to the module, they won’t appear in the generated XCFramework headers. If the Swift shell needs those types, api is required. The failure mode is completely silent: the type simply doesn’t exist in Swift autocomplete. One-word Gradle fix. Hours to diagnose without knowing to look there.


- BGTaskScheduler identifiers must be declared in Info.plist. If a background task identifier isn’t listed in BGTaskSchedulerPermittedIdentifiers, the system silently refuses to schedule it. no error, no crash. This only surfaced on a physical device, which is a good reminder that iOS background execution behaviour on the simulator is not representative of production.


# **The Small UX Details Children Actually Notice**


The most meaningful UX change in this project had nothing to do with architecture.


The web app applied a single static colour to all active courses. That is technically fine. But children notice progress differently from adults; visual reward matters in ways that are easy to underestimate on a spec sheet.


Course themes became dynamic: as children unlock new courses, each course card takes on its own distinct colour. The unlock moment becomes visible. Progress becomes something you can see, not just track.


The response to that change was disproportionate to the engineering effort it required. Product quality often comes from this kind of decision, not from any single large feature, but from dozens of small ones made by someone paying close attention to how the product actually feels to use.


# **What I Would Do Differently**


1. Invest in iOS device testing from day one. Too much early development happened on the simulator. Background execution, memory behaviour at the Kotlin/Swift boundary, and certain rendering edge cases only surface on real hardware.
2. Design shared ViewModel scopes earlier. The pattern for sharing ViewModels across screens emerged mid-project. It works well, but some earlier flow architecture had to be refactored to align with it. Starting with a clear scoping model would have saved that rework.


# **Shipping, Review, Crashes, and Real Users**


After months of iteration, offline edge cases, Nav3 crashes, and App Store review cycles, Cowrywise Kids shipped to both Google Play and the Apple App Store.


The Nav3 crashes were worth understanding in their own right. Jetpack Navigation 3 is powerful but has rough edges in early adoption, particularly around back-stack handling in multi-module setups. If you are considering it, test deep navigation flows early. The issues are solvable, but they surface late if you are not looking.


The App Store review cycles added time that better upfront planning would have reduced. The lesson was not that compliance is a barrier; it is that, for a children’s product, child-safety policy, privacy disclosures, and data-handling expectations are part of the product design, not a post-engineering concern.


Early usage has made the bet feel worth it: children are returning to continue lessons on real devices, in real homes, and under real network conditions. The product is now discoverable in the places parents already search, and learning continues in conditions where the web experience would have dropped off.


# **Final Lesson**


The product is live. Children are using it. Lessons are being completed on real devices, in real homes, with real network constraints.


That is what made the hard parts worth it. Not the shared code percentage. Nor the architecture diagram. Nor the fact that Kotlin now runs across both platforms.


The real win is simpler: a child can open the app, keep learning, and move forward, even when the network does not.


For a product designed to help children build better money habits early, reliability is not just a technical concern. It is part of the learning experience.
