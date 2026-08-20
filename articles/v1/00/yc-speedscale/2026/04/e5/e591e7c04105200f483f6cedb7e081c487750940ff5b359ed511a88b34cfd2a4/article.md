---
schema_version: "1.0.0"
document_id: "e591e7c04105200f483f6cedb7e081c487750940ff5b359ed511a88b34cfd2a4"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/when-your-observability-literally-stops-traffic/"
published_at: "2026-04-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:15:57.305389+00:00"
content_hash: "sha256:ffaf33e2883e18aa8763aa6423c2acdf339cbbb45dfaa20658e7135e816d3263"
---

# When Your Observability Literally Stops Traffic

Last week, a fleet of autonomous robotaxis in China suddenly stopped working—at scale. Over a hundred vehicles stalled across a city, stranding passengers in traffic and raising immediate concerns about safety, reliability, and trust in autonomous systems.


This wasn’t just a bad day for self-driving cars.


It was a distributed systems failure, one that happened in the physical world, not just in dashboards.


And it exposed something deeper: **observability, as we practice it today, isn’t enough.**


## The Illusion of Visibility


Modern engineering teams have spent the last decade investing heavily in[observability](https://speedscale.com/blog/api-observability-using-open-source) . Metrics, logs, and traces give us unprecedented visibility into how systems behave.


I worked at New Relic for almost 10 years, with some of the largest customers in the Fortune 100, and I saw this problem play out over and over again as teams tried to make sense of increasingly complex architectures.


I’ve seen firsthand how powerful observability can be. It allows teams to detect anomalies faster, understand system behavior at scale, and diagnose issues across sprawling microservice environments. In many ways, it’s become the backbone of modern operations.


But there’s an uncomfortable gap hiding underneath all that visibility.


**When an outage happens, all of the observability in the world can’t help you at that point.**


## Why Observability Alone Falls Short


Now imagine you’re on the engineering team responsible for that system. Your observability stack is doing exactly what it was designed to do. Alerts are firing, dashboards are lighting up, logs are pouring in, and traces are showing cascading failures across services.


You have visibility. You have data. You have clues.


And yet, you’re still stuck.


Because the hardest part of debugging isn’t detecting a failure. It’s being able to reliably understand and validate it. Without a way to recreate what happened, fixes become educated guesses. Edge cases remain uncertain. Confidence in a resolution is fragile.


This was my experience working at New Relic. Teams would have incredibly rich telemetry: beautiful dashboards, detailed traces, and more logs than they knew what to do with. They could often pinpoint *where* something went wrong within minutes.


But the next step was always the bottleneck.


Engineers would ask: *“How do we reproduce this?”*


And too often, there wasn’t a good answer.


What followed was a familiar cycle: spinning up staging environments, attempting to simulate traffic, writing one-off scripts, or waiting for the issue to happen again in production. Even with best-in-class observability, teams were still guessing when it came to[validating fixes](https://speedscale.com/blog/a-developers-guide-to-continuous-performance-testing) .


The core limitation is that observability is fundamentally reactive. It helps you see and analyze failures after they occur. It gives you the tools to investigate and form hypotheses about what went wrong.


But it stops short of enabling you to take the next step.


It doesn’t let you re-run the failure under the same conditions. It doesn’t give you a safe way to test fixes against real-world scenarios. And it doesn’t help you validate behavior before changes are pushed back into production.


In other words, it gives you insight, but not control.


## The Fourth Pillar: Reality


We often talk about the “three pillars” of observability: metrics, logs, and traces. Together, they provide a powerful lens into system behavior.


But incidents like this suggest there’s a missing piece, something more grounded than telemetry.


Reality itself.


What actually happened in production: the exact sequence of requests, the timing, the interactions between services. All of it needs to be captured in a way that can be used again. Not just observed once and stored, but replayed, tested, and understood.


This is where Speedscale changes the equation. Instead of relying on approximations or synthetic tests, it lets teams[capture real production traffic and replay it](https://speedscale.com/blog/api-staging-is-not-production-but-speedscale-makes-it-close) in controlled environments. That makes it possible to reproduce bugs deterministically, explore edge cases safely, and validate fixes before they ever reach users again.


## Physical Systems, Same Old Problems


It’s easy to think of autonomous vehicles as something entirely new, but at their core, they’re still software systems: distributed, event-driven, and highly dependent on real-time coordination.


The same patterns apply. APIs coordinate actions. Event streams drive decisions. Infrastructure handles load and timing constraints. The architecture may be more complex, but the underlying challenges are familiar.


What’s changed is the blast radius.


When a traditional microservice fails, the impact might be a spike in errors or a degraded user experience. When a system like this fails, the consequences spill into the physical world. Streets fill up. People get stuck. Trust erodes quickly.


## Closing Thoughts


The robotaxi outage wasn’t just a failure of autonomous driving technology. It was a reminder of a broader truth about modern systems.


Visibility without reproducibility is not enough.


Having worked in observability at New Relic, I believe deeply in the value these tools provide. They are essential for understanding complex systems. But they are only part of the solution.


The next step isn’t just better dashboards or more sophisticated alerts. It’s the ability to take what happened in production and replay it, turning real-world behavior into something engineers can test, iterate on, and ultimately fix with confidence.


Because when your system is operating in the real world, you don’t just need to see failures.


You need to recreate them, understand them, and eliminate them before they happen again.


[See how Speedscale captures and replays production traffic →](https://speedscale.com/)
