---
schema_version: "1.0.0"
document_id: "bf69d2f14f8fc704c8dbe49b5f2a0a0f43ece07f505d7d0517a667327f6f268f"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/depot-raises-series-a"
published_at: "2026-03-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:2582cf0c23d0fd8e87a45f11ddbbbb9848fca97dc2e313e657093233a63a4ec4"
---

# We've raised a $10M Series A from Felicis, Y Combinator, and Pioneer Fund

I'm excited to announce that we have raised $10M in Series A funding from our existing investors, Felicis, Y Combinator, and Pioneer Fund. We're setting out to build the future software development platform we all need, starting with the bottleneck we know best: CI. 🎉


It's a big milestone for us, but also for the thousands of teams that use Depot every day to build and ship their software. This simply wouldn't be possible without you.


I wanted to take a moment to share a bit more about what this means for Depot, our customers, and the future of software delivery as I see it today.


## From seed stage to defining a category around build acceleration


If we rewind to the version of Depot when we were just two founders getting into YC, we were focused on one core problem: Docker image builds sucked. We faced the same annoying grind in our day jobs at Thorn and later at Era Software. Watching a multi-platform image build in GitHub Actions takes 45 minutes, all because of flaky networks, bizarre cache limits, and painful emulation.


We got so frustrated with the problem that we just decided to go build the solution we wanted for ourselves. That POC made our own container builds 5x faster. I can still remember the surprise and telling Jacob, "There's no way that's possible." We were both blown away by the results from what could now be best described as a trivial solution when compared to what Depot does today.


But it turned out that spirit to go solve our own problems was a springboard to starting a company, getting into YC, and ultimately building a suite of products that powers some of the best engineering teams in the world.


Furthermore, the concept of build acceleration and the importance of making builds exponentially faster were considered fringe ideas at the time.


Most investors thought we were crazy and that "this could never be a real business." Turns out, they were wrong.


We have defined a category around build acceleration and the importance of making builds exponentially faster, well before it became the mainstream focus (more on that in a second).


Depot as a team has been relentlessly focused on this problem for the last 3 years. We have expanded our focus from container builds to GitHub Actions runners, local builds, remote caching, agent sandboxes, and, as you may have heard, our own Depot CI system built with performance as a top-level entity to power builds for engineers and agents.


## The bottleneck has shifted


Over the last year, we've seen software engineering teams enter a new phase where AI can generate code in minutes. With tools like Cursor, Claude Code, Codex, and many others, teams can now go from idea to running code in minutes with a few sentences of prompt. It's become table stakes for teams to adopt AI coding tools.


But the bottlenecks haven't been eliminated. They've been shifted.


The bottleneck has shifted from writing code to integrating it. Everything that comes after the code is written is now the bottleneck:


- Source control systems are slow or unreliable
- CI pipelines are slow, flaky, and unreliable
- Code review processes designed for a very different pace of engineering
- Context trapped in systems isolated from where code is being written and work is happening


This pain isn't new. It's been there for a long time. But it's now more visible and exponentially more painful than ever before.


## What we're building in the short term: Depot CI


The CI systems we have today weren't built for this moment. GitHub Actions, CircleCI, Buildkite — they were designed for a world where humans write code, batch it up, push it, and wait. A pipeline that takes 15 minutes was annoying but survivable.


That world is gone.


A team of 10 engineers with agents at their disposal can now operate like a team of 100. The volume of code being committed, the number of branches being worked, the rate at which things need to be validated — it has fundamentally changed. The old CI systems aren't just slow in this world. They're architecturally wrong for it.


So we built Depot CI from scratch.


Depot CI is a programmable CI engine built with performance as a first-class citizen — not bolted on, not an afterthought, not a checkbox. Fast orchestration, composable compute, and an API-first design that works for both engineers sitting at a terminal and agents running autonomously in a loop.


If you're already running GitHub Actions workflows, you can bring them directly. Depot CI runs them faster and more reliably. But GitHub Actions compatibility is just one frontend. Under the hood, Depot CI is something new: a system designed from day one to handle the velocity at which software is actually being built right now.


Three years of building the fastest container builds and GitHub Actions runners in the industry taught us exactly where CI breaks down at scale. Depot CI is what we would have built if we'd started with that knowledge on day one.


We're launching Depot CI soon. If you want early access,[join the waitlist](https://forms.gle/KnmY8WL3vhqVtBRs5) .


## What comes after?


Depot CI is the bridge, not the destination.


The destination is a software delivery platform where the entire pipeline — source control, CI, builds, validation — is designed from the ground up for the pace that engineers and agents are working at today. Not adapted from something built a decade ago. Built for this moment.


We're already seeing where the next bottlenecks are forming. Source control systems that weren't designed for the volume of commits agents produce. Code review workflows that assume a human is reading every line. Context trapped in one system that another system desperately needs. These aren't GitHub's fault — they built what made sense for the world that existed. That world has changed faster than any platform can adapt.


We know what we're building toward. We'll share more as we get closer.


## Thank you for your support


The AI explosion has raised the ceiling for what teams can build. Now we need the delivery layer to catch up.


We're going to make the steps between your idea and running code in production fast, reliable, and effortless. I believe it's our responsibility to build the tools and systems that bring that idea to life.


But we can't do it alone.


We need your feedback to tell us what new speed bumps you're hitting or things we could make easier for you. I'm grateful for every single one of you who has built with us along the way.


To the folks who have joined us to build Depot, thank you for believing in us and for bringing your brilliant minds and ideas to the table.


To Felicis, Y Combinator, and Pioneer Fund, and all of our other investors along the way, thank you for backing two scrappy engineers turned founders and for believing in us, even in the times when we struggle to believe in ourselves.


Thanks for building with us.


– Kyle and Jacob


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
