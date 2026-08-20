---
schema_version: "1.0.0"
document_id: "d5f13986fc0720e4f62a4f444c1018617e83adbbea71a3d7e93688b1e0d87209"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/a-new-era-of-software-quality"
published_at: "2026-06-23T06:48:00+00:00"
first_seen_at: "2026-07-22T04:45:46.439360+00:00"
fetched_at: "2026-08-19T18:58:31.052183+00:00"
content_hash: "sha256:e371d9133e4dba9b4a7b3735baabc7921d3c694ce459739b8ac33b1f83827561"
---

# A New Era of Software Quality Starts Today

Today we're announcing a new Momentic - a major platform update, a new brand, and a way for every developer to experience the future of testing for themselves.


### Why we rebuilt the platform


Jeff and I founded Momentic around a problem we lived with every day. As former engineers at Robinhood, Qualtrics, WeWork, and Retool, we watched the widening gap between how fast teams could write code and how confidently they could ship it. AI coding agents have only made this gap impossible to ignore.


More code is shipping faster than ever, and that means[more bugs and incidents](https://www.geekwire.com/2025/how-the-aws-outage-happened-amazon-blames-rare-software-bug-and-faulty-automation-for-massive-glitch/) reaching production. According to[Faros AI's 2026 AI Engineering Report](https://www.faros.ai/blog/ai-acceleration-whiplash-takeaways?utm_source=fnf&utm_medium=linkedin&utm_campaign=faros-june&utm_term=kbeck&utm_content=whitepaper) , monthly incidents are up nearly 58% since AI adoption accelerated. And a[May 2026 CloudBees study](https://www.cloudbees.com/) found that 81% of enterprise technology leaders say they've seen a direct increase in production issues tied to AI-generated code.


This isn't a prediction, it's already happening. The code is shipping, but that was never the real bottleneck. QA, which was already painful, is now falling more behind.


## What's new in Momentic


Over the past few weeks, we've been working closely with AI-native engineering teams whose applications collectively serve hundreds of millions of users, spanning productivity, media streaming, consumer applications, and professional services. They've been invaluable partners in shaping how agentic testing really should work alongside rapidly evolving developer toolchains.


One of our customers said it best:


> “The best testers I've ever worked with know the product better than anybody else. They know the nooks and crannies, what's supposed to happen and what isn't. They know where things are brittle. That's what's missing from agentic code in general.”


That insight became the foundation for everything we built next.


The teams we worked with weren't asking how to write better test scripts. They were asking for an always-on, autonomous system that actually understands their product, knows how it's supposed to behave, and gets smarter over time. Humans just have to review a report, not manually test.


So here’s what Momentic customers now have access to:


### Introducing memory and knowledge base


The best QA tester you’ve ever worked with didn’t just catch bugs. They knew your product's terminology, the flows that were brittle, the edge cases nobody had documented. That knowledge lived in their head, and when a new hire onboarded, they simply ‘got it’ too.


We built a way for that knowledge to live inside the platform instead, with our new[Knowledge Base](https://momentic.ai/docs/ai/knowledge-base#knowledge-base) . Teams define how their product is supposed to behave, what counts as a bug versus an intentional change, what terminology means in their specific context. Every agent, whether it's writing new tests, triaging failures, or proposing fixes, runs on that shared understanding. The more your team puts in, the smarter it gets.


Every team has a different bar for what "quality" means. Some want pixel-perfect accuracy; others just need the user to reach the destination. Now you can customize the platform accordingly.


### Coverage that grows with your product


Writing tests is usually the last thing engineers want to do. And with AI accelerating the number of commits daily, the gap between what's shipping and what's been verified keeps growing.


[Explore Agent](https://momentic.ai/docs/ai/explore) closes that loop automatically. Every time a PR lands, Momentic reads the diff, identifies what changed, and proposes new or updated tests, already scoped to the flows that matter, already consistent with your existing suite. It notices the new features, the renamed components, the edge cases that weren't there last sprint. Over time it gets better at this, because it's learning your product as it goes.


### Failures that mean something


Flaky tests are one of the worst problems to have in your test suite. Not because they're annoying, because they train engineers to ignore failures. And that's exactly when real bugs slip through.


Every failure in Momentic now gets triaged through our[Failure Classification Agent](https://momentic.ai/docs/reliability/failure-recovery#failure-recovery) and analyzes its root cause. Is it a real bug, intentional application change, test setup issue, or transient error? If it's an intentional change that triggered a test failure, Momentic opens a pull request to fix the test itself. If it's a real bug, the team gets a high-signal alert with full context on what broke and why. The result is a test suite that compounds trust over time.


### The spec *is* the test


For too long, test scripts have been artifacts that only the engineer who wrote them can understand.


The[new Momentic test format](https://momentic.ai/docs/get-started/migrate-to-simplified-format#6-author-simplified-format-files) is intent-based and readable by both humans and AI agents. Engineers just have to describe what they want to test in plain English. Plus, AI agents can parse, build, and modify tests more effectively - making the entire development loop faster and more autonomous.


### A new brand


Our platform evolution meant Momentic needed a new expression to match, one built around the same two qualities we engineered into the product itself: ease of use and guardrails that can evolve autonomously with your product.


The grid structure you'll see throughout our new site is intentional. Structure and reliability are still the foundation. But there's motion in it now, an energy that reflects what it feels like when your agent catches a bug before production, when tests update themselves, and the agent unblocked itself mid-execution and kept going. That's the energy the new brand is built to match.


## Quality for everyone


Quality used to be a function of how big your QA team was. Every developer, on every team, regardless of size, deserves quality. And it should be easy, enjoyable, and built into the foundation of how software gets made.


Today, Momentic is open to every software engineering team. It is free to try yourself. Our philosophy hasn't changed since day one: we don't want anything standing between teams and shipping.


Try it out yourself:


` npx @momentic/wizard@latest`
