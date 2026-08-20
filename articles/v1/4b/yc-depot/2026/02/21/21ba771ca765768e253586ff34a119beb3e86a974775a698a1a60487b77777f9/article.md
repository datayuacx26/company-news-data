---
schema_version: "1.0.0"
document_id: "21ba771ca765768e253586ff34a119beb3e86a974775a698a1a60487b77777f9"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/the-bottleneck-has-shifted"
published_at: "2026-02-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T22:22:47.419354+00:00"
content_hash: "sha256:1b6a43a8cbf0ff1beae7bfe268ddc55c93ea669daec05069c235616f67cb42e2"
---

# The bottleneck has shifted from writing code to integrating it

It’s officially 2026, and teams are settling back into their routines after the holidays. We’re seeing new teams adopt Depot and existing teams ramp up usage.


What's fueling this? It's AI and LLMs. We're experiencing a fundamental shift in software development.


For decades, the biggest bottleneck to innovation was writing code. But today, that's no longer true. Code and ideas can now iterate as fast as you can articulate them to an AI agent with a powerful model like Opus 4.5 or GPT-5.2 Codex.


Iteration can be done in seconds instead of hours. And a new generation of tools, services, and products are being imagined faster than ever.


The bottleneck has shifted from writing code to integrating it.


## How did we get here?


When LLMs first became available to the public, the developer ecosystem was quick to experiment with them. We saw folks generate code from prompts. It kind of worked and it was neat. But it was also a bit of a mess. The code was often buggy and the quality was often questionable. But it was a start.


We started seeing their potential, but we didn't really trust them.


So we figured out ways to integrate them into our existing workflows. The idea of spinning up agents via things like GitHub Actions or CircleCI became a thing. We quickly started seeing AI agents being "bolted on" to our existing workflows for code reviews, test generation, and more.


In essence, we took this new experimental capability and bolted it on to our existing human-centric workflows.


Then things changed. It felt like overnight, but it was steady improvements in model capabilities every couple of weeks. Models like Opus 4.5 and GPT-5.2 Codex are better at understanding code, better at generating code, better at understanding context, better at understanding codebases.


We shifted from a world where the agents and models were experimental and hard to trust to one where they are reliable, productive, and better than humans at many tasks.


## The new traffic jam that has been created


With that shift, a new traffic jam formed in our human-centric workflows.


Engineering teams operating with agents are now seeing the downstream effects of their newfound productivity. Pull requests pile up faster than humans can review them. CI queues grow because builds and tests are taking too long to run. Merge conflicts multiply as more changes flow simultaneously.


Repurposing our existing workflow that focused on engineers in the middle of everything is now at the root of the bottleneck. Bolting AI into our existing workflows worked okay initially. This worked when writing code was the slow part. When a developer spent days on a feature, 20 minutes of CI wasn't the constraint. **When you can generate a feature in 20 minutes, a 20-minute CI pipeline is unacceptable.**


I'm seeing this with Depot customers right now. They’re coming to us with more and more requests to hyper-optimize very specific parts of their CI workflows.


Can you help make our tests faster? Could you support caching this thing as well? Could you get us a runner in milliseconds for these CI checks that take 30 seconds to complete?


All these questions stem from teams that want to move faster, and they’re seeing the bottlenecks that have always been there but now are more pronounced.


We're producing exponentially more code and iterating faster than ever before. But we can't keep the momentum of this newfound power if we can't move it through review, integration, testing, and deployment faster.


## What got us here won't get us there


The human-centric workflows we built assume developers work at a measured, deliberate pace. Code review happens asynchronously. CI runs are expensive. Integration happens infrequently because changes are large and risky.


None of these hold anymore. AI agents produce working code in minutes, attempt dozens of approaches simultaneously, and generate code 24/7. But they're forced into collaboration patterns designed for humans working business hours on carefully crafted changes.


## The real-time feedback loop is the key


Here's what I believe: **real-time feedback loops are what empower software engineering at scale.**


We need to rethink our workflows to support this new reality. We need tests to run on every commit, merge conflicts to self-resolve automatically, context about how the code was developed should live right next to the code, and builds should be near instant. All of this should be seamless for both humans and agents.


It's not about a human in the loop anymore. It's about humans orchestrating work being done at scale, with engineers deciding what good vs. bad is, what to iterate on, what ideas to explore, and what to ship next.


We've seen this at Depot with build performance. Teams with 60-second builds make fundamentally different decisions than teams with 40-minute builds. They try more things. They validate assumptions faster. They catch issues earlier. **Speed doesn't trade off with quality. Speed enables quality.**


## We need a new paradigm


The solution isn't faster pull requests or cheaper CI pipelines. Those are incremental improvements to a fundamentally outdated model.


We need workflows that integrate continuously, validate automatically, provide real-time context, and work for both humans and agents.


Think about Google Docs. Multiple people work on the same document simultaneously. Changes sync in real-time. Comments live in the document. Spelling and grammar checks run automatically as you type. **Software engineering collaboration should work the same way.**


Every team adopting AI coding tools will hit this bottleneck. Some already have. But we are likely only at 1-2% adoption of AI coding tools. As adoption increases, this bottleneck becomes the most pronounced barrier to break down. Teams that figure out how to bust through it will have a massive competitive advantage. They'll operate with a new paradigm, where agents have everything they need to go from engineers’ ideas to running in production as quickly and autonomously as possible. They will run circles around their competitors who don't have this new paradigm. They’ll operate above their weight class.


## Related posts


- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Now available: Gocache v2 for improved Golang build performance](https://depot.dev/blog/now-available-gocache-v2-faster-improved-golang-build-performance)
- [Now available: Depot ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
