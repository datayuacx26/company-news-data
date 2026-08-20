---
schema_version: "1.0.0"
document_id: "34c6bd47cebc20835c89117a517e03a36b487eb6dfdebf8b160d62aa88f1c665"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/i-hate-testing/"
published_at: "2025-05-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:4fd82e23a51038b5a2be5eb6ad41e6dfeb12fac3c7d463fdbf255b8c3d825c25"
---

# I Hate Testing!

Hi everyone, I’m Alan, Head of Customer Success here at Speedscale. If you’d told me a few years ago that I’d be working for a company knee-deep in the testing space, I probably would have laughed. You see, for the better part of the last decade, my world has revolved around **observability** . I’ve lived and breathed metrics, logs, and traces. My passion has been understanding complex systems, pinpointing elusive issues in production, and helping engineering teams answer that critical question: “Why is this happening?”


Now, it’s important to distinguish between observability and testing, as they serve different primary purposes, even if they’re increasingly interconnected. **Observability, at its core, is about understanding the internal state of your systems by examining their outputs, especially in live environments.** It’s about asking questions of your system to understand its behavior, diagnose problems when they occur, and get a clear picture of performance in real-time. It often helps us react intelligently when things go wrong.


**Testing, on the other hand, is fundamentally about being proactive.** It’s the discipline of verifying that your code does what it’s supposed to do, and just as importantly, that it *doesn’t* do what it’s *not* supposed to do—ideally, long before it ever sees a real user. This is the heart of “shifting left”: moving quality assurance earlier in the development lifecycle to catch and fix issues when they are cheaper and easier to resolve. The goal of testing is to actively seek out weaknesses and potential failures in a controlled manner to prevent them from becoming production incidents.


So, why the jump from the often reactive world of “what went wrong” (observability) to the proactive world of “let’s make sure it doesn’t go wrong” (testing)? It’s because, ironically, my deep-seated frustration with *how traditional proactive testing was often executed* is what led me here. While observability gave us incredible insights into production realities, our methods for proactively ensuring quality often fell short.


Let’s be honest, how many of us have uttered the phrase “I hate testing” under our breath (or maybe even shouted it at a stubborn CI pipeline)? For years, from my observability vantage point, I saw the same painful patterns emerge:


- **The Time Sink:** Hours, days, sometimes weeks, spent writing and rewriting test scripts. Crafting the perfect unit test, the comprehensive integration test, the elusive end-to-end scenario. It often felt like more time was spent *preparing* to test than actually building valuable features.
- **The Unrealistic Environment:** We’d meticulously build staging or test environments, only for them to be pale imitations of production. Different data, different traffic patterns, different configurations, different scale. Then we’d act surprised when tests passed in staging but everything exploded in production. The classic “works on my machine” syndrome, scaled up.
- **The Flaky Test Nightmare:** Intermittent failures, tests that pass one run and fail the next for no discernible reason. Chasing these ghosts was a maddening exercise, eroding trust in the entire testing suite.
- **The Coverage Guesswork:** “Are we testing the right things? Are we testing *enough* things?” Coverage metrics told part of the story, but did they really reflect the myriad ways users would interact with our applications in the wild? Often, the answer was a painful “no,” discovered only after a production incident.
- **The “It’s Not My Job” Silo:** Testing often felt like a separate phase, a hurdle to overcome, rather than an integral part of the development lifecycle. Developers wanted to develop, and testing was… well, testing. This often led to it being rushed, descoped, or inadequately resourced.
- **The Mocking Morass:** As applications became more distributed and microservice-oriented, the complexity of mocking dependencies became a significant bottleneck. Creating and maintaining realistic mocks that accurately reflected the behavior of other services was a herculean task, often resulting in oversimplified or outdated simulations.


From my observability vantage point, this was particularly revealing. Day in and day out, observability tools gave us a clear, data-rich picture of what was *actually* happening in our production environments – real user interactions, the true behavior of dependencies, and all those unpredictable edge cases. We were deeply immersed in understanding this production reality, often in a reactive way when issues arose.


However, when it came to our *proactive* efforts in pre-production testing, it often felt like we weren’t fully leveraging this wealth of real-world insight. Instead of testing against the known complexities revealed by observability, we were often back to manually scripting scenarios, relying on synthetic data, and making educated guesses about production conditions. It was like having a detailed blueprint of reality from observability, but then trying to build a model from a vague sketch when it came to testing. This disconnect was a constant source of frustration, especially knowing that **even a little well-aimed proactive effort in testing, if truly reflecting these real-world conditions, could lead to significantly better code quality and, in many cases, prevent the very outages we were working so hard to diagnose and fix post-release.** We were missing a crucial opportunity to make our proactive work count more.


**Then I discovered Speedscale.**


It was a pivotal moment. The approach Speedscale took to testing resonated deeply with my observability background. The core idea was simple yet powerful: instead of trying to *guess* what production looks like, **why not use actual production traffic to drive our testing?**


This was the connection I had been missing. It meant we could move away from manually scripting countless scenarios and hoping they covered reality. Instead, we could test against the genuine complexity and unpredictability of real user interactions and system behaviors. The focus shifted from crafting synthetic tests to leveraging authentic production insights for proactive validation. This felt like a natural extension of an observability mindset – taking the clear picture of reality we already had and applying it to make our proactive efforts far more meaningful.


This is why I joined Speedscale. Because I don’t actually hate testing. I hate *ineffective, unrealistic, time-consuming, and confidence-sapping* testing. I believe that by leveraging the reality of our production environments, we can transform testing from a necessary evil into a powerful enabler of innovation and reliability.


If you’ve ever shared my frustrations with traditional testing, or if you’re passionate about building resilient, high-performant applications, I’d love to chat. Maybe, just maybe, we can even get you to stop hating testing too 😀.
