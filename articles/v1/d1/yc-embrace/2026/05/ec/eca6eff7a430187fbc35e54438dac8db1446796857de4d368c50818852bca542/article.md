---
schema_version: "1.0.0"
document_id: "eca6eff7a430187fbc35e54438dac8db1446796857de4d368c50818852bca542"
company_key: "yc-embrace"
company: "Embrace"
source_id: "yc-embrace-rss-35f82ba9b7dd"
canonical_url: "https://embrace.io/blog/beyond-the-golden-signals/"
published_at: "2026-05-21T23:55:55+00:00"
first_seen_at: "2026-07-20T23:20:15.781670+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:c476fc0dde9396c161f4850fcf334db1b1fb3efa27c4712d12f853adbb276602"
---

# Beyond the golden signals: Observability built around the end-user

The golden signals are not wrong, but they only tell you server-side health. You need user-focused observability to tell you whether users are succeeding.


## The problem: A green dashboard that lies


The four[SRE Golden Signals](https://sre.google/sre-book/monitoring-distributed-systems/) (latency, traffic, errors, and saturation) were built to keep server-side systems healthy. They are excellent at one job: telling you whether the service is running. They are nearly useless at the job that actually matters to the business: telling you whether the user is succeeding.


On mobile and web, the application runs on a device the operator does not own, in network conditions the operator cannot see. Latency is measured at the load balancer. Traffic is counted in requests per second. Every one of those signals stops at the edge of your infrastructure. The user’s experience[starts on the other side of that edge](https://embrace.io/blog/observability-is-stuck-in-the-past-your-users-arent/) .


The result is the most familiar story in modern engineering: Every dashboard is green,[every SLO is being met](https://get.embrace.io/mobile-slos-guide) , and users are still 1-starring the app, abandoning checkout, and churning. The signals aren’t able to see:


- A JavaScript exception that white-screens the cart.
- An ANR that freezes Android for eight seconds.
- A successful response that arrived in twelve seconds because the user was on a train.
- A new app version that crashes only on a specific OS, locale, or device class.
- A five-step funnel where step three silently fails for 10% of users.


None of that produces an internal error. None of it shifts a server-side metric. And none of it is captured by tooling designed for a closed system of code-you-wrote running on infrastructure-you-own.


This is not a tooling gap.[It is a worldview gap](https://get.embrace.io/mobile-observability-guide) . The golden signals assume the operator can see everything that matters. On the client, they can’t.


## Why error-focused observability isn't enough


Most teams try to close the gap with the tools they already have: a crash reporter (e.g., Crashlytics, Sentry) or a backend APM platform (e.g., Datadog, New Relic) stretched onto the client. These are error-focused tools. They ask one question, *“What broke?”*


That question produces a stack trace, maybe some breadcrumbs, and an engineer trying to reproduce the issue locally from incomplete context. It misses the things that determine whether a user finishes their journey:


- **No session context** . You see what crashed, not what the user was doing. What screens did they visit? What taps came before? What was the device state?
- **Sampling** . Many tools capture a slice of sessions. For most users, you have no data at all.
- **Event isolation** . A network failure, a UI freeze, and a crash are stored as three unrelated events instead of one connected timeline.
- **No visibility into degradation** . If the app is slow, janky, or burns battery without crashing, error-focused tools are blind to it.
- **Manual reconstruction** . Engineers stitch the story together by hand across logs, traces, and dashboards in different tools.


Crash reporters tell you something broke. Backend APM tells you the server was fine. Neither tells you what the user actually experienced.


## The shift: User-focused observability


[User-focused observability](https://embrace.io/blog/the-future-of-user-focused-observability/) inverts the question. Instead of, *“What broke?”* , it asks, *“What did the user experience?”* The fundamental unit is no longer the error event. It is the user session.


A session captures everything that happened during a single use of the app: every screen, every tap, every network call, every lifecycle event, every device state change, every span, every log. All of it is organized into a timeline that tells the complete story of what the user saw and did.


That changes the workflow:


1. An alert fires, a user reports a bug, or you proactively search for degradation.
2. The engineer opens the user’s session timeline.
3. They see the full sequence — what the user did, what the app did, what the device was doing — in the order it happened.
4. The root cause is visible in context. No guessing, no reproduction needed.


Three things make this possible:


- First, **100% unsampled capture** — every session, every user, no blind spots.
- Second, **correlated signals on a single timeline** — network failures, UI freezes, crashes, ANRs, low-memory warnings, all in the order they occurred.
- Third, a **session-first data model** built on OpenTelemetry, where sessions are root-level spans, traces and logs hang off them, and crashes are captured inside the session rather than as isolated events.


Once the session is the unit of analysis, things that were invisible become first-class: slow launches, UI hangs, abandoned flows, thermal throttling, and the long tail of “didn’t crash but didn’t work.”[User Journeys](https://embrace.io/blog/evolution-of-user-journeys/) turn login, search, and checkout into measurable flows with completion rates and abandonment points —[the metrics the business actually cares about](https://embrace.io/blog/when-business-metrics-drop-but-engineering-cant-explain-why/) .


## What good looks like


A team operating with user-focused observability and a strategic posture can answer questions the golden signals will never reach:


- Which user segments are abandoning checkout, and on which device classes?
- Did the new app version regress startup time for users on Android 12?
- How many sessions had a network timeout followed by a rage-tap and a backgrounding?
- When orders per minute dropped at 14:07, which client-side change correlates with it?
- What is the revenue cost of the slow product detail page we shipped last sprint?


None of those questions can be answered from the four golden signals alone. All of them can be answered when the session is the unit of analysis, capture is unsampled, frontend and backend telemetry are connected, and the team treats user experience as the metric of record.


## The takeaway


The golden signals are not wrong. They are necessary, and they will keep being necessary, because backend services still need to be kept alive. They are simply incomplete. They watch the server. They cannot watch the user.


[User-focused observability](https://embrace.io/blog/user-focused-observability-helps-mobile-teams/) is the missing half: a session-first model that captures the full reality of what users experience on devices and networks the operator does not control, then ties that experience back to[the business outcomes it drives](https://get.embrace.io/kpi-translation-toolkit) . The golden signals tell you the service is up. User-focused observability tells you the user is succeeding. A mature observability strategy needs both, and the teams that pull ahead will be the ones who stop confusing the first for the second.


Deliver incredible mobile experiences with Embrace.


Get started today with 1 million free user sessions.


[Get started free](https://dash.embrace.io/signup/)


Author


[Kayleigh Rex](https://embrace.io/author/kayleigh-rex/) Kayleigh Rex is a Solutions Engineer at Embrace and a veteran of teaching dinosaurs to figure skate. A decade in DevOps and digital transformation has taught her that error-focused observability is blind to the user experience. Your stack can be flawless and your users will still one-star your app if it feels like wading through treacle. She writes about OpenTelemetry, mobile observability, and the things backend teams confidently believe about monitoring front-end and mobile apps but probably shouldn't. When she's not assassinating technical and cultural bottlenecks, she skates for Team Scotland's roller derby squad, fresh off the 2025 World Cup.


Related Content


AI


5 June 2026


• 9 min read


### [What 300 frontend engineering teams told us about the future of AI in observability](https://embrace.io/blog/the-future-of-ai-in-observability/)


We surveyed 300 frontend engineering teams. 89% use AI in their workflows, but only 8% apply it to observability. See what drives the gap and how your team compares.


observability


6 March 2026


• 4 min read


### [Monitoring Vs Observability Vs Telemetry: What’s the Difference](https://embrace.io/blog/monitoring-vs-observability-vs-telemetry-whats-the-difference/)


In this article, we’ll clarify what sets monitoring, observability, and telemetry apart, and why a clear grasp of these terms is crucial for building resilient systems.


observability


14 November 2025


• 6 min read


### [OpenTelemetry experts share the future of browser support](https://embrace.io/blog/opentelemetry-experts-share-the-future-of-browser-support/)


Members of the Browser Special Interest Group share challenges in browser observability and the future of OpenTelemetry support for the web.
